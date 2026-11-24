---
asignatura: fundamentos-computadores
tema: 4
parcial: 1
fuentes:
  - "tema_4_organizaci_n_de_la_e_s.md (Diap. 1-51 SÍ ENTRA; Diap. 52-78 DMA EXCLUIDO)"
  - "C. Transcripción 9.md"
  - "C. Transcripción 14.md"
  - "C. Transcripción 15.md"
  - "Transcripcion clase 15.md (apoyo secundario)"
  - "C. Transcripción 16.md"
  - "C. Transcripción 17.md"
  - "C. Transcripción 18.md"
  - "C. Transcripción 19.md"
  - "C. Transcripción 20.md"
  - "C. Transcripción 21.md"
  - "C. Transcripción 22.md"
estado: listo-para-estudio
---

# Tema 4: Organización de la Entrada / Salida (MOC)

> [!info] Alcance Exacto en el Primer Parcial (¡DMA EXCLUIDO!)
> El Tema 4 es el bloque más extenso y con mayor carga práctica del Primer Parcial ($9$ clases completas, **Clases 14 a 22**). El profesor delimitó expresamente su alcance en las **Clases 20 y 22**:
>
> | Estado | Contenidos del Tema 4 |
> | :--- | :--- |
> | **✅ SÍ ENTRA EN EL 1er PARCIAL** | • **I — Estructura de la E/S (Diap. $1$–$12$):** Sistema procesador, interfase, periférico, *driver*, convenio CPU-céntrico (`Read`/`Write`), hardware de adaptación (ventilador/transistor, *pull-up*, display $7$-segmentos), registros (Estado, Control, Datos e híbridos), **Modelo de Programación** y *Watchdog Timer*.<br>• **Direccionamiento de la interfase (Diap. $13$–$25$):** Colisión de bus TTL, *Chip Select* (`CS`), lógica de decodificación, ARM AHB / ESP32-C3 (multiplexor), **E/S mapeada por memoria** (fragmentación — *vending*) vs **E/S diferenciada** (`IN`/`OUT`, $\overline{M}/IO$) y modelo mixto (ATmega48).<br>• **II — Métodos de E/S: PIO (Diap. $26$–$34$):** PIO vs idea general de DMA, las $2$ instrucciones de transferencia PIO, ADC de $10\text{ bits}$ del PIC (`ADRESH`/`ADRESL`, `ADCON0` `GO/DONE`), **Bloqueo de programa vs Consulta periódica** (latencia, multinúcleo, batería/*sleep*) y protocolo `DAV`/`DAC`/`RTR`/`DENV`.<br>• **Interrupciones hasta Controlador inclusive (Diap. $35$–$51$):** Línea `INT`, atención al fin de instrucción, cambio de contexto (*péndulo invertido*), **interrupción autointerrumpida** (`GIE` / flancos), vector y tabla de vectores (`INTA`), vector único `0x0004` (`RETFIE`), **orígenes múltiples por *polling*** (OR cableado con resistencia, `INTCON`/`PIR`/`PIE`, máscara `AND`, $11$ muestreos ADC), bases de tiempo con **`Timer0`** y **Controlador de Interrupciones hardware** (codificador con prioridad, `NUEVA < ACTUAL`, **Vector Base** por excepciones, APIC). |
> | **❌ EXCLUIDO DEL 1er PARCIAL (Pasa al 2º Parcial)** | • **E/S por DMA completo (Diap. $52$–$78$):** Arquitectura DMA tradicional, arbitraje de bus (`BR`/`BG`, `HOLD`/`HLDA`), arbitración paralela centralizada, DMA *Buffered I/O* vs *Direct I/O*, chipsets modernos y GDMA en ESP32.<br>• *Citas literales:* *"El DMA no entra para este parcial; eso entrará para el segundo parcial"* (*Clase 20*) y *"Tema 1, Tema 2, Tema 3 y Tema 4 menos DMA"* (*Clase 22*). |
> | **🧪 GUÍA PRÁCTICA PIC / PICGenios** | • Los detalles operativos de **PIC Simulator IDE** y **PICSimLab** (`ALL_DIGITAL`, `WaitMs 50`, `RC0` relé, `RB0` RTC, conflicto `PORTD`/`TRISD` entre teclado matricial y $7$-segmentos, y prohibición de `WaitMs` para temporizar en el examen de laboratorio) están integrados con etiquetas claras en las sub-notas **4.1** y **4.5**. |

---

## Mapa de Sub-notas del Tema 4

Para garantizar **cero pérdida de contenido** sobre las $51$ diapositivas evaluables y las $9$ clases completas, el Tema 4 se divide en **5 sub-notas atómicas**:

1. **[[4.1_Estructura_ES_Interfase_y_Modelo_Programacion|4.1. Estructura de la E/S, Hardware de Adaptación y Modelo de Programación]]** *(Diap. 1–12 | Clases 14, 15 y 17)*
   - Esquema general (`Sistema procesador`–`Interfase`–`Periférico`), *driver* y sentido CPU-céntrico (`Read` = Entrada, `Write` = Salida).
   - Esquemas `circuitikz` y `wavedrom` de los **3 casos hardware de PICGenios**: ventilador de $12\text{ V}$ con transistor, pulsador con **resistencia *pull-up*** y sus flancos, y **display de $7$ segmentos multiplexado** (`1024`).
   - Registros de Estado, Control y Datos (con aviso de registros híbridos); **Modelo de Programación** (brazo robótico, `PIC16F877` $\rightarrow$ `877A`, LCD `HD44780`); *Watchdog Timer* y guía práctica de la placa PICGenios.
2. **[[4.2_Direccionamiento_Interfase_Mapeada_vs_Diferenciada|4.2. Direccionamiento de la Interfase: Mapeada vs Diferenciada]]** *(Diap. 13–25 | Clases 15, 16 y 17)*
   - Bus multipunto TTL, **[[Colisión de Bus]]** (`circuitikz`) y línea ***Chip Select* (`CS`)**.
   - Decodificación burda (`A7=1`) vs fina (`A1, A0` $\rightarrow$ `SEL1, SEL0` y `A7..A2` $\rightarrow$ `CS`), y buses modernos (**ARM AHB / ESP32-C3** con multiplexor de bajo consumo).
   - **E/S mapeada por memoria** (problema de la **máquina de *vending* de $60$ motores** y fragmentación) vs **E/S diferenciada** (`IN`/`OUT`, $\overline{M}/IO$), modelo mixto y *datasheet* real del **ATmega48**.
3. **[[4.3_Metodos_ES_PIO_y_Consulta_de_Estado|4.3. Métodos de E/S: PIO y Consulta de Estado (Handshaking)]]** *(Diap. 26–34 | Clases 18, 19 y 20)*
   - Visión general PIO vs DMA; las **2 instrucciones de transferencia PIO** (trampa `IN` + `SB` en diferenciada) y la excepción de 1 sola instrucción para consultar estado.
   - Caso real del **ADC de $10\text{ bits}$ del PIC** (`ADRESH`, `ADRESL`, registro híbrido `ADCON0` bit `GO/DONE`).
   - **Bloqueo de programa vs Consulta periódica:** tiempo real (ABS Toyota híbrido vs F1), monocore vs multinúcleo (*Hyper-Threading*, botón "Cancelar"), consumo en batería (TFG estación meteorológica $\rightarrow$ `sleep`) y protocolo de $4$ líneas **`DAV`/`DAC`/`RTR`/`DENV`**.
4. **[[4.4_Interrupciones_Hardware_Enmascaramiento_y_Vectores|4.4. Interrupciones Hardware, Enmascaramiento y Vectores]]** *(Diap. 35–44 | Clases 19, 20 y 21)*
   - Línea hardware `INT`; diferencias entre subrutina (`GoSub`), subproceso (*thread*) e interrupción (`On Interrupt`).
   - Ciclo de atención al **fin de instrucción en curso** (salvar `PC`), coste del cambio de contexto (**TFG del péndulo invertido** donde *polling* vence a interrupciones).
   - **Interrupción autointerrumpida** (`wavedrom` oficial, BSOD / Kernel Panic) y sus **2 soluciones** (flanco o enmascaramiento `GIE`/`RETFIE`).
   - **Vector de interrupción**, Tabla de Vectores (TVI), ciclo `INTA` y el **vector único `0x0004`** del PIC (`ORG 0x0004`, 4 instrucciones de contexto).
5. **[[4.5_Origenes_Multiples_Polling_Timer0_y_Controlador_Interrupciones|4.5. Orígenes Múltiples: Polling, Timer0 y Controlador de Interrupciones]]** *(Diap. 45–51 | Clases 21 y 22)*
   - **OR cableado** (`circuitikz` con resistencia de protección contra cortocircuito), *polling* de registros de estado, prioridad por software, `IRQ 9` compartida en Windows XP.
   - Arquitectura híbrida del PIC: **`INTCON`** (control + estado), **`PIE`/`PIR`**, discriminación con máscara `AND` / `INTCON.2`, y los **$11$ muestreos hasta el ADC (`ADIF`)**.
   - **Programación de bases de tiempo con `Timer0`** ($f_{\text{osc}}/4$, nunca para, limpiar `TMR0IF` manualmente, precarga a `6` para $250\ \mu\text{s}$ o `56` para $200\ \mu\text{s}$, variable `Word` para $5000$ ticks $= 1\text{ s}$).
   - **Controlador de Interrupciones hardware** (máscaras `AND`, codificador con prioridad, `NUEVA < ACTUAL`, **registro Vector Base + sumador** por excepciones internas como división por cero, y **APIC**).

---

## Radar Rápido de Examen — Tema 4

> [!danger] Aviso / Trampa de Examen — Top 8 Trampas del Tema 4
> 1. **Registros híbridos (*Clases 18 y 21*):** *"Suele caer porque es muy sencilla, pero soléis responderla mal."* Tanto **`INTCON`** (máscaras `GIE`/`IE` + flags `IF`) como **`ADCON0`** (bit `GO/DONE`) son **registros de control Y de estado a la vez**.
> 2. **Trampa de la línea `M/IO` (*Clase 16*):** *"No os dejéis engañar si en un diagrama os pongo una línea `M/IO`."* Fijarse siempre en las **instrucciones**: `LB`/`SB` (`LD`/`ST`) $\Rightarrow$ mapeada por memoria; `IN`/`OUT` $\Rightarrow$ diferenciada.
> 3. **Trampa de las 2 instrucciones en PIO diferenciada (*Clase 18*):** En una lectura periférico $\rightarrow$ CPU $\rightarrow$ memoria, la primera es `IN $3, puerto`, pero la segunda sigue siendo **`SB $3, mem` (¡NUNCA `OUT`!)**, porque escribe en memoria.
> 4. **Resistencia *pull-up* y flancos (*Clase 15*):** Evita cortocircuito al pulsar y fija `1` en reposo; genera **flanco de bajada al pulsar** y **flanco de subida al soltar**.
> 5. **Interrupción autointerrumpida (*Clase 20*):** Si `INT` sigue a `1` al acabar la 1ª instrucción de la ISR, reentra provocando BSOD / Kernel Panic. Soluciones: **disparo por flanco** o **enmascaramiento automático (`GIE = 0` $\rightarrow$ `RETFIE`)**.
> 6. **OR cableado (*Clase 21*):** Requiere referencia común a tierra y **resistencias de protección** en cada salida para evitar cortocircuito si un periférico pone $+5\text{ V}$ y otro $0\text{ V}$.
> 7. **Reglas de `Timer0` (*Clase 21*):** Trabaja a $f_{\text{osc}}/4$, **nunca se para**, **`TMR0IF` no se limpia solo** y para contar $1\text{ s}$ ($5000$ ticks de $200\ \mu\text{s}$ con precarga `56`) exige variable **`Word` ($16\text{ bits}$)**.
> 8. **Registro de Vector Base en el Controlador de Interrupciones (*Clase 22*):** Imprescindible porque las primeras entradas (`0, 1, 2, 3...`) de la tabla de vectores están reservadas por la CPU para **excepciones internas** (como una división por cero).

---

## Conexiones con el Grafo del Primer Parcial

- [[Tema_1_Indice]] (Tema 1): [[Arquitectura Von Neumann]] (bus multipunto CPU–Memoria–E/S), [[Ciclo de Instrucción]] (por qué las interrupciones se atienden al final de la instrucción salvando el `PC`), RAM volátil vs Flash no volátil.
- [[Tema_2_Indice]] (Tema 2): [[Binario Natural]] sin signo en tipos del PIC (`Bit` empaquetado, `Byte` con riesgo de *underflow* $0-1=255$, `Word` de $16\text{ bits}$ para $4000\text{ ms}$ en *vending* o $5000$ ticks en `Timer0`); CRC en ficheros Intel HEX.
- [[Tema_3_Indice]] (Tema 3): [[3.4_Arquitectura_Harvard_y_Pipeline_PIC16F87XA]] explica el origen de $f_{\text{osc}}/4$ en `Timer0` y la medición de tiempos de segmentos de código con interrupción; [[3.2_Modos_de_Direccionamiento]] explica el modelo unificado en bancos del PIC frente a MIPS; instrucciones `LB`/`SB` de [[3.3_Arquitectura_y_Ensamblador_MIPS]].

---

## Checklist de Dominio del Tema 4

- [ ] Dibujar y justificar el hardware de adaptación del ventilador (transistor), la resistencia *pull-up* (y sus flancos), el barrido multiplexado de $7$-segmentos y la colisión de bus TTL (`CS`).
- [ ] Distinguir sin dudar E/S mapeada por memoria de E/S diferenciada, diseñar una decodificación fina sin fragmentar el espacio de memoria y escribir las $2$ instrucciones exactas de lectura/escritura PIO en ambos modelos.
- [ ] Justificar en $2$–$3$ líneas cuándo elegir Bloqueo de programa, Consulta periódica, Interrupciones o modo `sleep` según requisitos de latencia, concurrencia monocore/multinúcleo y consumo de batería.
- [ ] Explicar paso a paso el ciclo de atención a interrupción, la interrupción autointerrumpida, el ciclo `INTA` con TVI, el *polling* con OR cableado (`INTCON`/`PIE`/`PIR`) y el controlador de interrupciones hardware (codificador con prioridad, `NUEVA < ACTUAL` y **Vector Base**).
- [ ] Calcular precargas exactas de `Timer0` (ej. `6` para $250\ \mu\text{s}$, `56` para $200\ \mu\text{s}$ a $4\text{ MHz}$) y dimensionar la variable contadora (`Word`) para bases de tiempo largas.
