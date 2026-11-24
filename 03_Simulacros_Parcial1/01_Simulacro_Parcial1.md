---
asignatura: fundamentos-computadores
parcial: 1
tipo: simulacro-examen
duracion: "120 minutos"
puntuacion_maxima: 10
fuentes:
  - "01_Mapa_y_Estrategia/01_Radar_de_Examen.md"
  - "02_Apuntes_Maestros/Tema_1/ (completo)"
  - "02_Apuntes_Maestros/Tema_2/ (completo)"
  - "02_Apuntes_Maestros/Tema_3/ (completo)"
  - "02_Apuntes_Maestros/Tema_4/ (completo)"
estado: listo-para-estudio
---

# 📝 Simulacro Oficial de Examen — Primer Parcial (Fundamentos de Computadores)

> [!danger] INSTRUCCIONES DEL TRIBUNAL (Condiciones Reales de Examen)
> - **Duración máxima:** $2\text{ horas}$ ($120\text{ minutos}$).
> - **Material permitido:** Únicamente bolígrafo y papel. **PROHIBIDO el uso de calculadora** ni dispositivos electrónicos.
> - **Estructura y calificación:** **Bloque A** ($5 \times 0.80 = 4.00\text{ puntos}$) + **Bloque B** ($4 \times 1.50 = 6.00\text{ puntos}$) = **$10.00\text{ puntos}$**. Aprobado eliminatorio: $\ge 5.00$.
> - **REGLA DE ORO DE CORRECCIÓN:** **SIEMPRE JUSTIFICA LA RESPUESTA.** Responder *"Sí"*, *"No"* o escribir un valor numérico correcto sin razonamiento técnico que demuestre comprensión **se calificará con $0.00$ puntos**.
> - **Consejo:** Haz el examen completo en papel cronometrándote antes de desplegar los callouts de solución al final.

---

# BLOQUE A — Cuestiones Cortas de "Justifica la Respuesta" ($4.00\text{ puntos}$)

---

## Cuestión A1 — Arquitectura Harvard vs Von Neumann y Ciclo de Instrucción ($0.80\text{ puntos}$)
*Temas 1 y 3 (Diap. 122–124) — Nivel sobresaliente / matrícula*

Observa el siguiente cronograma interno del microcontrolador **PIC16F877A** funcionando con un cristal de $f_{\text{osc}} = 4\text{ MHz}$:

```wavedrom
{
  "signal": [
    { "name": "OSC1 (4 MHz)",    "wave": "P...........", "period": 1 },
    ["Fases",
      { "name": "Q1",            "wave": "10..10..10.." },
      { "name": "Q2",            "wave": "010..10..10." },
      { "name": "Q3",            "wave": "0.10..10..10" },
      { "name": "Q4",            "wave": "0..10..10..1" }
    ],
    { "name": "PC",              "wave": "=...=...=...", "data": ["PC", "PC+1", "PC+2"] },
    { "name": "Fetch",           "wave": "=...=...=...", "data": ["Fetch INST(PC)", "Fetch INST(PC+1)", "Fetch INST(PC+2)"] },
    { "name": "Execute",         "wave": "=...=...=...", "data": ["Execute INST(PC-1)", "Execute INST(PC)", "Execute INST(PC+1)"] }
  ],
  "foot": {
    "text": "Cada fase dura 4 ciclos de OSC1 (Q1–Q4); Fetch(PC+1) y Execute(PC) ocurren en paralelo"
  }
}
```

1. **($0.50\text{ pts}$)** Un compañero afirma: *"Este diagrama corresponde al ciclo de instrucción de una máquina de Von Neumann que ejecuta $1$ instrucción cada $1\ \mu\text{s}$ gracias a solapar Fetch y Execute."* ¿Es correcta su afirmación? Justifica qué arquitectura usa realmente el PIC16F877A y **por qué una máquina de Von Neumann sencilla NO puede realizar este solapamiento**.
2. **($0.30\text{ pts}$)** En una máquina de Von Neumann convencional, si un error en una instrucción de salto hace que el Contador de Programa (`PC`) pase a apuntar a una dirección donde habíamos almacenado un dato numérico, ¿detectará la memoria o la CPU automáticamente que ese patrón binario es un dato y no una instrucción? Justifica qué ocurre exactamente.

---

## Cuestión A2 — Decisiones de Diseño de IEEE 754 y Trampas con Reales ($0.80\text{ puntos}$)
*Tema 2 (Teoría de Reales — Sin conversiones numéricas)*

En el software de control de un brazo robótico escrito en C con variables `float` (estándar **IEEE 754 de $32\text{ bits}$**), un ingeniero escribe el siguiente fragmento:

```c
if (posicion == 0.35) {
    ratio = par_motor / delta;
}
```

1. **($0.45\text{ pts}$)** Explica con rigor **por qué la condición `posicion == 0.35` puede fallar** aunque matemáticamente el cálculo debiera dar $0.35$ (y cómo corregirla), e indica **qué caso especial de IEEE 754** (valores de característica $C$ y mantisa $M$) devuelve `par_motor / delta` si `delta` vale exactamente `0.0`, explicando por qué **NO** devuelve $+\infty$ y en qué situación física real sí aparecería $+\infty$.
2. **($0.35\text{ pts}$)** Justifica estas dos decisiones de diseño del formato IEEE 754:
   - (a) Por qué el bit de signo se sitúa en el bit $31$ (MSB), separado de la mantisa por la característica.
   - (b) Por qué el exponente se representa en **Exceso a $127$** en lugar de **Complemento a 2**, si ambos ocupan $8\text{ bits}$.

---

## Cuestión A3 — Direccionamiento de E/S, Trampa PIO y Registros Híbridos ($0.80\text{ puntos}$)
*Tema 4 (Direccionamiento, PIO y Registros de Interfase)*

Un alumno examina el esquema de conexión de un microprocesador con una línea de control $\overline{M}/IO$ y plantea las dos afirmaciones siguientes:

- **Afirmación 1:** *"Para leer $1\text{ byte}$ del puerto de E/S `0x05` (E/S diferenciada) y almacenarlo en la dirección `0x40` de la RAM por PIO, la CPU ejecuta primero `IN $3, 0x05` y a continuación `OUT 0x40, $3`. Y si solo quisiera consultar un bit de estado del puerto `0x05`, necesitaría igualmente las $2$ instrucciones."*
- **Afirmación 2:** *"El registro `INTCON` del PIC16F877A es exclusivamente un registro de control porque su nombre termina en `CON` (de Control), al igual que `ADCON0`."*

Identifica, corrige y justifica **todos los errores técnicos** presentes en ambas afirmaciones (citando instrucciones exactas y bits concretos de ambos registros).

---

## Cuestión A4 — Hardware de Adaptación: *Pull-Up* y OR Cableado ($0.80\text{ puntos}$)
*Tema 4 (Hardware eléctrico de PICGenios y Orígenes múltiples)*

1. **($0.40\text{ pts}$)** Un pulsador conectado a `RB0` con una **resistencia *pull-up*** a $+5\text{ V}$ dispara la interrupción externa del PIC16F877A configurada por **flanco de subida**. Al probarlo, el alumno se queja de que la acción no ocurre al presionar el botón, sino al soltarlo. Justifica qué forma de onda eléctrica genera el circuito (orden de flancos) y explica **qué ocurriría físicamente si el alumno eliminara la resistencia *pull-up* conectando $+5\text{ V}$ directamente al pin `RB0`**.
2. **($0.40\text{ pts}$)** Tres periféricos (`P1`, `P2`, `P3`) comparten la línea `INT` de la CPU mediante un **OR cableado** en lógica TTL ($+5\text{ V}$ / $0\text{ V}$). Si el esquema dibuja simplemente sus tres salidas unidas al mismo cable, ¿qué **dos elementos eléctricos implícitos** son indispensables para que el circuito funcione sin averiarse cuando `P1` activa `INT` (`1`) y `P3` está en reposo (`0`)? Justifica qué fallo se produciría sin ellos.

---

## Cuestión A5 — Estrategias de E/S y Controlador de Interrupciones Hardware ($0.80\text{ puntos}$)
*Tema 4 (PIO, Interrupciones y Controlador de Interrupciones)*

1. **($0.45\text{ pts}$)** Justifica qué técnica de gestión de E/S elegirías en cada uno de estos dos sistemas empotrados monocore y qué fallo produciría elegir la contraria:
   - **Sistema 1:** Estación meteorológica en el monte alimentada por **batería** que lee un sensor de humedad cada $2\text{ minutos}$.
   - **Sistema 2:** Controlador de un **péndulo invertido** que necesita atender $30\,000$ eventos de sensor por segundo.
2. **($0.35\text{ pts}$)** En un **Controlador de Interrupciones hardware** con $4$ líneas de entrada (`INT#0` a `INT#3`), el codificador con prioridad entrega $2\text{ bits}$ (`00`, `01`, `10`, `11`). ¿Por qué el controlador **NO** envía directamente esos valores (`0..3`) como vector de interrupción a la CPU, sino que incorpora un **registro `Vector Base`** y un sumador? Además, ¿cómo cambiarías la prioridad entre `P2` y `P3` en este controlador frente a un sistema por *polling*?

---

# BLOQUE B — Problemas de Desarrollo Técnico ($6.00\text{ puntos}$)

---

## Problema B1 — Dimensionamiento Alfanumérico, Enteros de $8\text{ bits}$ y Hamming SEC ($1.50\text{ puntos}$)
*Tema 2 completo (en papel, sin calculadora)*

### Apartado 1.1 — Dimensionamiento de memoria y transmisión de un dron ($0.50\text{ pts}$)
Un dron de telemetría muestrea simultáneamente dos magnitudes a **$5\text{ muestras/segundo}$** durante **$1\text{ minuto}$**:
- **Altitud relativa:** entero en el rango $[-15,\; 600]\text{ m}$ (sin unidad `'m'`).
- **Nivel de batería:** entero en el rango $[0,\; 100]\%$ (sin símbolo `'%'`).

1. ¿Cuánta RAM mínima (en bytes) necesita el dron para almacenar ese minuto completo en **binario puro** (usando Complemento a 2 para la altitud y Binario Natural para la batería, en bytes completos)?
2. Si el dron transmite cada muestra como **dos líneas de texto** (línea 1: altitud; línea 2: batería) en **ASCII de $7\text{ bits}$**, calcula el tamaño mínimo del buffer por muestra en el **peor caso** si el emisor usa saltos de línea de **Windows** (`CR+LF`) frente a **Linux** (`LF`). ¿Qué consecuencia grave ocurre si un receptor **Linux** dimensiona su buffer para sí mismo y recibe desde **Windows**?
3. Si cambiamos la codificación de esa transmisión numérica de **ASCII de $7\text{ bits}$** a **UTF-8**, ¿en cuántos bytes aumenta cada muestra? Justifica.

### Apartado 1.2 — Conversiones e interpretación de enteros en $8\text{ bits}$ ($0.50\text{ pts}$)
Completa las dos tablas siguientes (muestra los pasos breves en tu hoja; indica *"No válido"* o *"No representable"* donde proceda):

**a) Interpretar la cadena de $8\text{ bits}$ `10000101` en decimal:**

| Cadena ($8\text{ bits}$) | Binario Natural | Signo-Magnitud (SM) | Complemento a 2 (C2) | Exceso $128$ | BCD ($2$ dígitos) |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **`10000101`** | ? | ? | ? | ? | ? |

**b) Codificar el entero decimal $-35_{10}$ en $8\text{ bits}$:**

| Decimal | Binario Natural | Signo-Magnitud (SM) | Complemento a 2 (C2) | Exceso $128$ | BCD ($2$ dígitos) |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **$-35_{10}$** | ? | ? | ? | ? | ? |

### Apartado 1.3 — Código Hamming SEC $(7,4)$ con paridad par ($0.50\text{ pts}$)
Usando la disposición oficial $D_7\; D_6\; D_5\; P_4\; D_3\; P_2\; P_1$:
1. Codifica el dato de $4\text{ bits}$ **`1011`** ($D_7=1,\; D_6=0,\; D_5=1,\; D_3=1$) calculando $P_4, P_2, P_1$ y escribiendo la palabra completa de $7\text{ bits}$.
2. El receptor recibe la palabra **`0111101`**. Calcula el síndrome $(C_4 C_2 C_1)_2$, indica si existe error, señala exactamente qué bit está corrupto y reconstruye el dato original de $4\text{ bits}$.

---

## Problema B2 — Formato de Instrucción, Modos de Direccionamiento y MIPS ($1.50\text{ puntos}$)
*Tema 3 (Parte 1 completa)*

### Apartado 2.1 — Diseño de formato de instrucción ($0.60\text{ pts}$)
Diseña el formato de instrucción de longitud fija de **$32\text{ bits}$** para un procesador con las siguientes especificaciones:
- **$45$ instrucciones** distintas en su ISA.
- **$16$ registros** de propósito general de $32\text{ bits}$.
- Todas las instrucciones tienen **$2$ operandos (`OP1` y `OP2`)**:
  - `OP1` referencia **siempre** un registro (modo fijo).
  - `OP2` puede utilizar cualquiera de estos **$4$ modos**: *inmediato*, *directo a registro*, *directo a memoria* o *relativo a PC*.

Dibuja la estructura de campos de los $32\text{ bits}$ indicando el rango de bits y la anchura justificada de cada campo (`OPCODE`, `OP1`, selector de modo $MD_{op2}$ y `OP2`). Si `OP2` codifica un salto relativo de **$-5$ instrucciones hacia atrás**, ¿qué sistema de representación usa ese campo y por qué?

### Apartado 2.2 — Evaluación de modos de direccionamiento ($0.50\text{ pts}$)
En un procesador ficticio de palabras de $16\text{ bits}$ ($4\text{ bits}$ por campo), se ejecuta la instrucción `ADD A, B, C` ($A \leftarrow B + C$) codificada como **`0100 1001 0011 0101`** (por tanto, el campo **`B = 0011`**). Suponiendo que todos los datos están en **binario natural sin signo** y que el estado actual es:
- Registro implícito `Base = 1000`.
- Banco de registros: `R3 (0011) = 1010`.
- Memoria: `Mem[0011] = 0110`, `Mem[1010] = 1100`, `Mem[1011] = 0100`.

Determina el valor decimal de `B` que se sumará a `C` si el operando `B` utiliza cada uno de los siguientes modos:
1. **Inmediato**
2. **Directo absoluto a registro**
3. **Directo absoluto a memoria**
4. **Directo relativo con registro implícito (`Base`)**
5. **Indirecto por registro**

### Apartado 2.3 — Arquitectura MIPS (*Load-Store*, modos mixtos y pseudoinstrucciones) ($0.40\text{ pts}$)
1. Identifica y justifica el modo de direccionamiento de **cada operando** en las instrucciones MIPS: `beq $s1, $s2, loop` y `lw $t0, 12($s0)`.
2. Explica por qué en MIPS real no existe la instrucción `li $t0, 0x12345678` y cómo la traduce el ensamblador MARS a código máquina real.

---

## Problema B3 — Mapa de E/S, Fragmentación y Hardware de Periféricos ($1.50\text{ puntos}$)
*Tema 4 (Estructura, Hardware de Adaptación y Direccionamiento)*

Un microcontrolador de $8\text{ bits}$ con bus de direcciones $A_7 \dots A_0$ ($256\text{ bytes}$ de espacio de direcciones, `0x00`–`0xFF`) controla una cinta clasificadora industrial con **$55$ estaciones**. El programa en C necesita almacenar un *array* contiguo con la información de cada estación:
- Tiempo de accionamiento en milisegundos (entre $500\text{ ms}$ y $3500\text{ ms}$).
- Sentido de desvío ($1\text{ byte}$).
- Estado de la estación ($1\text{ byte}$).

El sistema conecta $2$ periféricos mediante **E/S mapeada por memoria**:
- **Periférico 1 (Motor y sensores):** $4$ registros (`E`, `C`, `DE`, `DS`).
- **Periférico 2 (Display):** $3$ registros (`E`, `C`, `DS`).

### Apartado 3.1 — Dimensionamiento y fragmentación ($0.40\text{ pts}$)
Calcula el tamaño en bytes del *array* contiguo de las $55$ estaciones (justificando cuántos bytes exige el tiempo en ms). Si el diseñador de hardware decide ubicar Periférico 1 en `0x40` (`0x40`–`0x43`) y Periférico 2 en `0xA0` (`0xA0`–`0xA3`), **¿cabrá el *array* en memoria?** Justifica numéricamente calculando el tamaño de cada bloque libre resultante.

### Apartado 3.2 — Diseño de decodificación fina sin fragmentación ($0.60\text{ pts}$)
Rediseña el mapa de direcciones de ambos periféricos para eliminar la fragmentación:
1. Indica qué direcciones hexadecimales asignas a cada registro de Periférico 1 y Periférico 2, y qué bloques quedan libres para la RAM.
2. Explica qué líneas exactas de $A_7 \dots A_0$ van conectadas a `SEL1, SEL0` y cuáles a la lógica de decodificación (`CS #M`, `CS #1`, `CS #2`), y **por qué el `CS` evita una colisión de bus TTL**.
3. ¿Por qué la $4.\text{ª}$ dirección asociada al Periférico 2 debe marcarse como **Reservada**?

### Apartado 3.3 — Hardware de adaptación en campo ($0.50\text{ pts}$)
1. El motor de la cinta trabaja a $12\text{ V}$ y consume $120\text{ mA}$, mientras el pin del microcontrolador solo soporta $5\text{ V}$ y un máximo de $25\text{ mA}$. Dibuja o describe con precisión el hardware de adaptación necesario para encenderlo sin quemar el micro.
2. El contador de piezas se muestra en **$4$ displays de $7$ segmentos** que comparten un único puerto de $8\text{ bits}$ (`PORTD`) y cuentan con $4$ transistores de habilitación individuales (`RA2`–`RA5`). Explica cuál es el problema hardware al mostrar el número **`2048`** y cómo lo resuelve el software.

---

## Problema B4 — Interrupciones, Bases de Tiempo con `Timer0` y Medición de Código ($1.50\text{ puntos}$)
*Temas 3 y 4 (PIC16F877A: Interrupciones, `INTCON` y `Timer0`)*

Disponemos de un microcontrolador **PIC16F877A** con un oscilador externo de **$f_{\text{osc}} = 4\text{ MHz}$** en el que debemos generar una **base de tiempo exacta de $1\text{ segundo}$** usando la interrupción de **`Timer0`** y atender al mismo tiempo el pulsador externo conectado a **`RB0`**.

### Apartado 4.1 — Cálculo de precarga de `Timer0` y variable contadora ($0.50\text{ pts}$)
1. Justifica cuánto tiempo dura cada incremento (*tick*) de `Timer0` a $4\text{ MHz}$ y cada cuántos microsegundos desborda si se deja contar desde `0` hasta `255`.
2. Para obtener una base de tiempo limpia de exactamente **$250\ \mu\text{s}$** por interrupción, ¿qué valor de **precarga** debes escribir en `Timer0`?
3. Usando esa base de $250\ \mu\text{s}$ (sin emplear el *prescaler*), ¿cuántas interrupciones debemos contar en la ISR para completar **$1\text{ segundo}$**, y de qué **tipo de dato entero del BASIC del PIC** (`Bit`, `Byte`, `Word`) debe ser obligatoriamente la variable contadora? Justifica.

### Apartado 4.2 — Diagnóstico de errores en la ISR y autointerrupción ($0.50\text{ pts}$)
Un alumno configura `GIE = 1`, `TMR0IE = 1` e `INTE = 1`, pero comete estos dos errores al programar:
- **Error 1:** No pone `TMR0IF = 0` ni antes de habilitar las interrupciones ni dentro de la rutina de servicio (`On Interrupt`).
- **Error 2:** Dentro de la rutina única en `0x0004`, discrimina el origen con un `If INTCON.2 = 1 ... Else If INTCON.1 = 1 ...` (rama `Else` excluyente).

Explica **qué dos comportamientos erróneos concretos** causarán ambos fallos (teniendo en cuenta las dos reglas críticas de `Timer0` y la gestión de orígenes múltiples por *polling*). Además, explica brevemente qué es la **interrupción autointerrumpida** y cómo la impide el hardware del PIC mediante `GIE` y `RETFIE`.

### Apartado 4.3 — Medición del tiempo de ejecución de un código crítico $S$ ($0.50\text{ pts}$)
Queremos usar `Timer0` para medir con exactitud cuántos microsegundos tarda en ejecutarse un bloque de código crítico $S$ que tiene aproximadamente **$300\text{ instrucciones}$**:
1. ¿Por qué no basta con poner `Timer0 = 0` al inicio de $S$ y leer `cuenta = Timer0` al final?
2. ¿Por qué NO podemos vigilar el desbordamiento (`TMR0IF`) mediante *polling* por software dentro de $S$?
3. Si habilitamos la interrupción de `Timer0` para contar desbordamientos en una ISR, **qué perturbación exacta en la cuenta introduce esa interrupción** (explicando el código en `ORG 0x0004`) y cómo debemos corregirla?

---

---

# ✅ SOLUCIONES DETALLADAS Y RÚBRICA DE CORRECCIÓN

> [!info] Cómo autocorregirte con rigor (Metodología Adversaria)
> Despliega cada solución **solo tras haber escrito tu respuesta completa en papel**. Aplica estrictamente la rúbrica de penalizaciones: si dejaste un "Sí/No" sin el "por qué" físico o arquitectónico, date **$0$ puntos** en ese subapartado igual que haría el tribunal real.

---

> [!success]- Solución y Rúbrica — Bloque A (Cuestiones A1 a A5 — $4.00\text{ puntos}$)
>
> ### Solución A1 ($0.80\text{ pts}$)
> 1. **($0.50\text{ pts}$):** **Falso en cuanto a la arquitectura (sí acierta en $1\ \mu\text{s}$).**
>    - El PIC16F877A **NO es una máquina de Von Neumann**, sino una **arquitectura Harvard**, porque separa físicamente la memoria de programa (Flash de $14\text{ bits}$) de la memoria de datos (RAM de $8\text{ bits}$) con buses independientes.
>    - A $4\text{ MHz}$, cada fase dura $4$ ciclos ($T_{\text{CY}} = 4 / 4\text{ MHz} = 1\ \mu\text{s}$); aunque cada instrucción tarda $8$ ciclos totales, el *pipeline* de $2$ etapas completa $1$ instrucción cada $1\ \mu\text{s}$ (salvo saltos que fuerzan `Flush`/`NOP`).
>    - **Por qué Von Neumann NO puede solaparlos:** durante `Execute`, la instrucción en curso puede leer o escribir datos en memoria (especialmente en el PIC, que opera directamente sobre RAM) **al mismo tiempo** que `Fetch` lee la siguiente instrucción de memoria. En Von Neumann, al existir una **única memoria**, se produciría una **colisión por dos accesos simultáneos a la misma memoria**, imposible en memorias sencillas. En Harvard ambos accesos van por memorias y buses separados sin conflicto.
> 2. **($0.30\text{ pts}$):** **No lo detectará.** En Von Neumann instrucciones y datos son cadenas binarias indistinguibles en la misma memoria; la CPU distingue una instrucción únicamente porque accede a ella en la fase *Fetch* mediante el `PC`. Por tanto, cargará el dato en `IR` e intentará decodificarlo y ejecutarlo como código máquina, causando un fallo de ejecución / cuelgue (*segmentation fault*) o la ejecución de código inyectado (*hackeo*).
> - **Rúbrica (qué quita puntos):** Decir "no es Von Neumann" sin nombrar **Harvard** ni explicar la **colisión de $2$ accesos simultáneos a la única memoria**: $-0.35\text{ pts}$. No mencionar el papel del **`PC` en *Fetch*** en el punto 2: $-0.15\text{ pts}$.
>
> ---
>
> ### Solución A2 ($0.80\text{ pts}$)
> 1. **($0.45\text{ pts}$):**
>    - **Por qué falla `posicion == 0.35`:** Muchas fracciones decimales exactas (como $0.35$) generan representaciones binarias **periódicas infinitas** que deben truncarse/redondearse en los $23\text{ bits}$ finitos de la mantisa ($0.3499999\dots$), arrastrando error en operaciones sucesivas y haciendo fallar la igualdad bit a bit. **Corrección:** usar umbral de tolerancia, p. ej. `if (abs(posicion - 0.35) < 0.001)`.
>    - **División por `0.0`:** Devuelve **`NaN`** (*Not a Number*), codificado con **$C = 255$ (`11111111`) y $M \neq 0$**. **NO** devuelve $+\infty$ porque la división por cero no está definida matemáticamente y es un error (igual que variables sin inicializar).
>    - **Cuándo aparece $\pm\infty$ ($C = 255,\; M = 0$):** ante un desbordamiento por factor de escala o al evaluar una función válida cerca de una **asíntota** (dividiendo por un valor muy pequeño distinto de cero).
> 2. **($0.35\text{ pts}$):**
>    - **(a) Signo en bit $31$ (MSB):** para mantener el **mismo convenio de signo que C2 y SM** (`MSB = 1` negativo, `MSB = 0` positivo sin inspeccionar bits intermedios).
>    - **(b) Exceso a $127$ frente a C2:** porque los exponentes crecen de forma **continua y monótona en binario natural** desde el más negativo (`00000000`) al más positivo (`11111111`), permitiendo que el hardware **compare rápidamente** qué número en coma flotante es mayor sin que el bit de signo del C2 invierta el orden.
> - **Rúbrica:** Responder que `X / 0.0` da $+\infty$: **$-0.25\text{ pts}$** (trampa mortal). No indicar los valores de $C$ y $M$ para `NaN`: $-0.10\text{ pts}$. Decir que Exceso $127$ "ahorra bits": **$-0.15\text{ pts}$** (ambos usan $8\text{ bits}$).
>
> ---
>
> ### Solución A3 ($0.80\text{ pts}$)
> - **Error 1 (`OUT` en lugar de `SB` — $0.30\text{ pts}$):** En una transferencia de **lectura** PIO con E/S diferenciada (Periférico $\rightarrow$ CPU $\rightarrow$ RAM), la primera instrucción lee del puerto con `IN $3, 0x05`, pero la segunda **escribe en la memoria RAM (`0x40`)**, NO en un puerto de E/S; por tanto debe ser **`SB $3, 0x40`** (instrucción de escritura en memoria), **¡NUNCA `OUT`!**.
> - **Error 2 (2 instrucciones para estado — $0.20\text{ pts}$):** Para una simple **consulta de estado** (valor "de usar y tirar" para comprobar un bit), **basta $1$ sola instrucción** (`IN $3, 0x05` hacia un registro de CPU para testearlo), sin necesidad de llevar el dato hasta memoria RAM. *(Nota extra de rigor: recordar además que para identificar E/S mapeada vs diferenciada mandan siempre las instrucciones usadas, no solo la presencia de una línea `M/IO` en el diagrama).*
> - **Error 3 (`INTCON` y `ADCON0` — $0.30\text{ pts}$):** **Falso.** Ambos son **registros híbridos: de control Y de estado a la vez** (práctica habitual en microcontroladores cortos de registros).
>   - `INTCON` contiene bits de **control** (`GIE`, `PEIE`, `TMR0IE`, `INTE`, `RBIE`) y *flags* de **estado** (`TMR0IF`, `INTF`, `RBIF`).
>   - `ADCON0` contiene el bit híbrido **`GO/DONE`**: escribir `1` inicia la conversión (**control**), leer `1`/`0` indica si está en curso o terminada (**estado**).
> - **Rúbrica:** No corregir `OUT` por `SB` justificando que el destino es espacio de memoria: $-0.30\text{ pts}$. Afirmar que `INTCON` es solo de control: $-0.30\text{ pts}$.
>
> ---
>
> ### Solución A4 ($0.80\text{ pts}$)
> 1. **($0.40\text{ pts}$):** Con resistencia *pull-up* a $+5\text{ V}$, la línea `RB0` está en reposo a **`1` lógico ($+5\text{ V}$)** y al pulsar el botón se conecta a tierra (**`0` lógico, $0\text{ V}$**). Por tanto: **al pulsar** ocurre un **flanco de bajada (`1` $\rightarrow$ `0`)** y **al soltar** ocurre un **flanco de subida (`0` $\rightarrow$ `1`)**. Al haber configurado disparo por flanco de subida, la interrupción salta al soltar el botón.
>    - **Si se elimina la resistencia *pull-up*** (conectando $+5\text{ V}$ directo al pin), en cuanto se pulse el botón se une $+5\text{ V}$ directamente con tierra (`0 V`) sin resistencia limitadora, produciendo un **cortocircuito** que quema la fuente, el pulsador o el microcontrolador.
> 2. **($0.40\text{ pts}$):** Faltan implícitamente: **(1) referencia común a tierra (`GND`)** y **(2) una resistencia de protección ($R_{\text{prot}}$)** en serie a la salida de cada periférico. Sin $R_{\text{prot}}$, cuando `P1` saca `1` ($+5\text{ V}$) y `P3` saca `0` ($0\text{ V}$, tierra) en la misma línea compartida, se genera un **cortocircuito directo** entre $+5\text{ V}$ y masa (similar a una colisión de bus) que daña los circuitos y tira abajo el nivel alto de `INT`.
> - **Rúbrica:** Invertir los flancos del *pull-up*: $-0.25\text{ pts}$. Omitir el cortocircuito en cualquiera de los dos apartados: $-0.20\text{ pts}$ cada uno.
>
> ---
>
> ### Solución A5 ($0.80\text{ pts}$)
> 1. **($0.45\text{ pts}$):**
>    - **Estación meteorológica (batería, cada $2\text{ min}$):** Usar **modo de bajo consumo (`sleep`) + interrupción de temporizador hardware**. Usar bloqueo de programa (`while`) mantendría la CPU al $100\%$ durante $120\text{ s}$ agotando la batería en horas.
>    - **Péndulo invertido ($30\,000$ eventos/s):** Usar ***polling* (consulta de estado)**. Con decenas de miles de interrupciones por segundo, el *overhead* de salvar y restaurar el contexto continuamente en la pila/RAM consume más tiempo de CPU que el propio algoritmo de control, desestabilizando el péndulo.
> 2. **($0.35\text{ pts}$):**
>    - **Por qué `Vector Base`:** Porque las primeras entradas de la Tabla de Vectores de Interrupción (`0, 1, 2, 3...`) están **reservadas por el hardware del procesador para excepciones internas** (como una **división por cero**, `9/0`, que lanza la rutina del SO). Sumando un `Vector Base` configurado por PIO (ej. `60`), los periféricos reciben vectores libres (`60..63`) sin colisionar.
>    - **Cambio de prioridad:** En el **controlador hardware**, la prioridad está cableada en las entradas `#0..#3` del codificador $\Rightarrow$ exige **cambiar físicamente de *slot* de expansión**. En ***polling*** basta **reordenar dos líneas `if` por software**.
> - **Rúbrica:** Recomendar interrupciones para los $30\,000$ eventos/s sin advertir el *overhead* de cambio de contexto: $-0.25\text{ pts}$. No mencionar las **excepciones internas de CPU** al justificar `Vector Base`: $-0.20\text{ pts}$.

---

> [!success]- Solución y Rúbrica — Problema B1 ($1.50\text{ puntos}$)
>
> ### Apartado 1.1 — Dron de telemetría ($0.50\text{ pts}$)
> 1. **RAM en binario puro ($0.20\text{ pts}$):**
>    - *Número total de muestras:* $5\text{ muestras/s} \times 60\text{ s} = \mathbf{300\text{ muestras}}$.
>    - *Altitud $[-15,\; 600]$ en C2:* $1\text{ byte}$ en C2 solo llega hasta $+127 < 600 \implies$ requiere **$2\text{ bytes}$** ($16\text{ bits}$, rango $[-32\,768,\; +32\,767]$; aunque bastarían $11\text{ bits}$, la RAM usa bytes completos).
>    - *Batería $[0,\; 100]$ en BN:* $100 \le 255 \implies$ cabe en **$1\text{ byte}$**.
>    - *Total:* $(2 + 1)\text{ B/muestra} \times 300\text{ muestras} = \mathbf{900\text{ bytes}}$.
> 2. **Buffer de texto en ASCII $7\text{ bits}$ ($0.20\text{ pts}$):**
>    - *Recordatorio:* cada carácter ASCII de $7\text{ bits}$ ocupa **$1\text{ byte}$ ($8\text{ bits}$)**.
>    - *Peor caso línea 1 (Altitud):* **`"-15"`** ($3\text{ B}$, el signo `'-'` cuenta) y **`"600"`** ($3\text{ B}$) $\implies \mathbf{3\text{ bytes}}$.
>    - *Peor caso línea 2 (Batería):* **`"100"`** $\implies \mathbf{3\text{ bytes}}$.
>    - **Emisor Windows (`CR+LF` = $2\text{ B/línea}$):** $3 + 3 + (2 \times 2) = \mathbf{10\text{ bytes/muestra}}$.
>    - **Emisor Linux (`LF` = $1\text{ B/línea}$):** $3 + 3 + (2 \times 1) = \mathbf{8\text{ bytes/muestra}}$.
>    - *Consecuencia si Linux ($8\text{ B}$) recibe de Windows ($10\text{ B}$):* **desbordamiento del buffer y pérdida irrecuperable de datos** (además de error de *parsing*).
> 3. **ASCII vs UTF-8 ($0.10\text{ pts}$):** **$0\text{ bytes}$ de diferencia.** Los dígitos `'0'`–`'9'`, el signo `'-'`, `CR` y `LF` pertenecen a los $128$ primeros códigos donde UTF-8 coincide exactamente con ASCII ($1\text{ B}$/carácter).
> - **Rúbrica:** Usar $1\text{ B}$ para altitud hasta $600$, o multiplicar por $7\text{ bits}$ en ASCII, o decir que UTF-8 ocupa $2\text{ B}$ por dígito: $-0.15\text{ pts}$ por cada error.
>
> ### Apartado 1.2 — Tablas de enteros $8\text{ bits}$ ($0.50\text{ pts}$ — $0.05\text{ pts}$ por celda)
>
> **a) Cadena `10000101`:**
> - **BN:** $128 + 4 + 1 = \mathbf{133}$.
> - **SM:** MSB=`1` (negativo), magnitud `0000101` $= 5 \implies \mathbf{-5}$.
> - **C2:** MSB=`1` (negativo); inv(`10000101`) = `01111010`, $+1 =$ `01111011` $= 64+32+16+8+2+1 = 123 \implies \mathbf{-123}$ *(comprobación: $-128 + 5 = -123$)*.
> - **Exceso $128$:** $133 - 128 = \mathbf{+5}$.
> - **BCD:** `1000` $= 8$, `0101` $= 5 \implies \mathbf{85}$ (ambos cuartetos $\le 9$, válido).
>
> **b) Entero $-35_{10}$** *(magnitud $+35 = 32 + 2 + 1 = \texttt{00100011}_2$)*:
> - **BN:** **No representable** (solo enteros sin signo $[0, 255]$).
> - **SM:** **`10100011`**.
> - **C2:** inv(`00100011`) = `11011100`, $+1 =$ **`11011101`** *(comprobación: $-128 + 64 + 16 + 8 + 4 + 1 = -35$)*.
> - **Exceso $128$:** $-35 + 128 = 93 = 64 + 16 + 8 + 4 + 1 =$ **`01011101`** *(igual a C2 con MSB invertido)*.
> - **BCD:** **No representable** (BCD no admite números negativos).
>
> ### Apartado 1.3 — Código Hamming SEC $(7,4)$ ($0.50\text{ pts}$)
> 1. **Codificación de `1011`** ($D_7=1,\; D_6=0,\; D_5=1,\; D_3=1$) ($0.25\text{ pts}$):
>    - $P_4 = D_7 \oplus D_6 \oplus D_5 = 1 \oplus 0 \oplus 1 = \mathbf{0}$
>    - $P_2 = D_7 \oplus D_6 \oplus D_3 = 1 \oplus 0 \oplus 1 = \mathbf{0}$
>    - $P_1 = D_7 \oplus D_5 \oplus D_3 = 1 \oplus 1 \oplus 1 = \mathbf{1}$
>    - **Palabra SEC ($D_7 D_6 D_5 P_4 D_3 P_2 P_1$):** **`1010101`**.
> 2. **Recepción de `0111101`** ($D_7=0, D_6=1, D_5=1, P_4=1, D_3=1, P_2=0, P_1=1$) ($0.25\text{ pts}$):
>    - $C_4 = P_4 \oplus D_7 \oplus D_6 \oplus D_5 = 1 \oplus 0 \oplus 1 \oplus 1 = \mathbf{1}$
>    - $C_2 = P_2 \oplus D_7 \oplus D_6 \oplus D_3 = 0 \oplus 0 \oplus 1 \oplus 1 = \mathbf{0}$
>    - $C_1 = P_1 \oplus D_7 \oplus D_5 \oplus D_3 = 1 \oplus 0 \oplus 1 \oplus 1 = \mathbf{1}$
>    - **Síndrome:** $(C_4 C_2 C_1)_2 = 101_2 = \mathbf{5} \implies$ **error en el bit de posición $5$ ($D_5$)**.
>    - Invirtiendo $D_5$ (`1` $\rightarrow$ `0`): palabra corregida `0101101` $\implies$ **dato original de $4\text{ bits}$: `0101`**.
> - **Rúbrica:** Leer el síndrome en orden invertido $(C_1 C_2 C_4)_2$: $-0.15\text{ pts}$. No extraer el dato final de $4\text{ bits}$ corregido: $-0.10\text{ pts}$.

---

> [!success]- Solución y Rúbrica — Problema B2 ($1.50\text{ puntos}$)
>
> ### Apartado 2.1 — Diseño de formato de instrucción ($0.60\text{ pts}$)
> - **`OPCODE`:** $45$ instrucciones ($0..44$ en BN) $\Rightarrow 2^5 = 32 < 45 \le 64 = 2^6 \implies \mathbf{6\text{ bits}}$ (bits $31\text{--}26$).
> - **`OP1`:** $16$ registros ($0..15$) $\Rightarrow 16 = 2^4 \implies \mathbf{4\text{ bits}}$ (bits $25\text{--}22$). Como su modo es siempre fijo a registro, requiere **$0\text{ bits}$ de selector de modo**.
> - **$MD_{op2}$:** `OP2` elige entre $4$ modos $\Rightarrow 4 = 2^2 \implies \mathbf{2\text{ bits}}$ (bits $21\text{--}20$).
> - **`OP2`:** Bits restantes $= 32 - (6 + 4 + 2) = \mathbf{20\text{ bits}}$ (bits $19\text{--}0$).
>
> | Campo | `OPCODE` | `OP1` | $MD_{op2}$ | `OP2` |
> | :--- | :---: | :---: | :---: | :---: |
> | **Rango de bits** | $31\text{--}26$ ($6\text{ b}$) | $25\text{--}22$ ($4\text{ b}$) | $21\text{--}20$ ($2\text{ b}$) | $19\text{--}0$ ($20\text{ b}$) |
>
> - **Salto de $-5$ instrucciones:** Se codifica en **Complemento a 2** para poder sumarlo directamente al `PC` usando el mismo sumador binario hardware tanto para saltos hacia adelante como hacia atrás.
> - **Rúbrica:** Añadir bits de modo innecesarios para `OP1`: $-0.15\text{ pts}$. Suma distinta de $32\text{ bits}$: $-0.20\text{ pts}$. No justificar C2 en el salto negativo: $-0.15\text{ pts}$.
>
> ### Apartado 2.2 — Modos de direccionamiento con `B = 0011` ($0.50\text{ pts}$ — $0.10\text{ pts}$ c/u)
> 1. **Inmediato:** el campo es el propio valor $\Rightarrow 0011_2 = \mathbf{3}$.
> 2. **Directo absoluto a registro:** índice `0011` $= R_3 \Rightarrow 1010_2 = \mathbf{10}$.
> 3. **Directo absoluto a memoria:** dirección `0011` $\Rightarrow \text{Mem}[0011_2] = 0110_2 = \mathbf{6}$.
> 4. **Directo relativo con registro implícito (`Base`):** $DE = \text{Base} + \text{Desp} = 1000_2 + 0011_2 = 1011_2 \implies \text{Mem}[1011_2] = 0100_2 = \mathbf{4}$.
> 5. **Indirecto por registro:** índice `0011` $\rightarrow R_3 = 1010_2 \rightarrow \text{Mem}[1010_2] = 1100_2 = \mathbf{12}$.
>
> ### Apartado 2.3 — MIPS ($0.40\text{ pts}$)
> 1. **Modos mixtos ($0.25\text{ pts}$):**
>    - En **`beq $s1, $s2, loop`**: `$s1` y `$s2` usan **directo absoluto a registro** ($5\text{ b}$ c/u); `loop` usa **directo relativo con registro implícito (`PC`)** (el `PC` no es accesible al programador y viene implícito en el `OPCODE`).
>    - En **`lw $t0, 12($s0)`**: `$t0` usa **directo absoluto a registro**; `12($s0)` usa **directo relativo con registro explícito** (*Base addressing*: $DE = R[\text{\$s0}] + 12$).
> 2. **Pseudoinstrucción `li $t0, 0x12345678` ($0.15\text{ pts}$):** Como las instrucciones MIPS miden exactamente $32\text{ bits}$, es imposible meter `OPCODE`, registro y un inmediato de $32\text{ bits}$ en $32\text{ bits}$ (formato I solo admite $16\text{ b}$). MARS la traduce automáticamente en **dos instrucciones máquina reales**: **`lui`** (carga `0x1234` en los $16\text{ b}$ altos) seguida de **`ori`** (combina `0x5678` en los $16\text{ b}$ bajos).

---

> [!success]- Solución y Rúbrica — Problema B3 ($1.50\text{ puntos}$)
>
> ### Apartado 3.1 — Dimensionamiento y fragmentación ($0.40\text{ pts}$)
> - **Tamaño del *array*:** Guardar hasta $3500\text{ ms}$ en BN supera $255$ ($1\text{ B}$), por lo que exige **$2\text{ bytes}$** ($16\text{ b}$). Cada estación ocupa $2 + 1 + 1 = 4\text{ B} \implies 55 \times 4 = \mathbf{220\text{ bytes contiguos}}$.
> - **¿Cabe con P1 en `0x40–0x43` y P2 en `0xA0–0xA3`?** **NO.** Aunque sobran $256 - 8 = 248\text{ B} > 220\text{ B}$ en total, el espacio de memoria queda **fragmentado** en tres bloques libres disjuntos:
>   - `0x00`–`0x3F`: **$64\text{ B}$**.
>   - `0x44`–`0x9F`: $160 - 68 = \mathbf{92\text{ B}}$.
>   - `0xA4`–`0xFF`: $256 - 164 = \mathbf{92\text{ B}}$.
>   - Ningún bloque contiguo alcanza los $220\text{ B}$ necesarios para el *array* en C.
>
> ### Apartado 3.2 — Decodificación fina sin fragmentación ($0.60\text{ pts}$)
> 1. **Mapa agrupado (ejemplo óptimo al final del espacio, o equivalente al inicio `0x00–0x07`):**
>    - **RAM contigua libre:** `0x00` a `0xF7` = **$248\text{ bytes contiguos}$** ($> 220\text{ B}$ ✔).
>    - **Periférico 1 (`0xF8`–`0xFB`):** `0xF8` (`E`), `0xF9` (`C`), `0xFA` (`DE`), `0xFB` (`DS`).
>    - **Periférico 2 (`0xFC`–`0xFF`):** `0xFC` (`E`), `0xFD` (`C`), `0xFE` (`DS`), `0xFF` (**Reservada**).
>    *(Nota: igualmente válido colocar P1 en `0x00–0x03`, P2 en `0x04–0x07` y RAM en `0x08–0xFF` = $248\text{ B}$).*
> 2. **Cableado y `CS`:** Los $2$ bits bajos (**`A1, A0`**) van directamente a **`SEL1, SEL0`** de ambos periféricos (seleccionan cuál de sus $4$ registros locales); los $6$ bits altos (**`A7..A2`**) entran en la **lógica de decodificación**, que activa exclusivamente una única señal **`CS`** (`CS #M`, `CS #1` o `CS #2`). Al garantizar que solo un dispositivo acopla sus salidas de escritura al bus de datos cada vez (dejando el resto en alta impedancia / lectura), impide que dos dispositivos pongan simultáneamente $+5\text{ V}$ y $0\text{ V}$ provocando un cortocircuito (**colisión de bus TTL**).
> 3. **Por qué `0xFF` (o `0x07`) es Reservada:** Para preservar compatibilidad futura del **Modelo de Programación**: si en una revisión posterior se añade un $4.\text{º}$ registro al Periférico 2, el espacio ya está protegido y ningún software previo entrará en conflicto.
> - **Rúbrica:** Elegir direcciones base que NO terminen en `00` binario (ej. `0xF9`): $-0.20\text{ pts}$. No explicar la colisión TTL (`+5 V` vs `0 V`): $-0.15\text{ pts}$.
>
> ### Apartado 3.3 — Hardware de adaptación ($0.50\text{ pts}$)
> 1. **Motor $12\text{ V}/120\text{ mA}$ ($0.25\text{ pts}$):** Supera tensión ($5\text{ V}$) y corriente máxima ($25\text{ mA}$) del pin. Se interpone un **transistor NPN como interruptor digital** (con resistencia de base): el pin del micro entrega apenas unos pocos miliamperios ($\sim 3\text{ mA}$) a la base para saturar el transistor y cerrar a tierra el circuito independiente de $12\text{ V}$ del motor, de forma transparente al programador.
> 2. **Displays $7$-segmentos `2048` ($0.25\text{ pts}$):** Al compartir el mismo bus `PORTD`, si se encendieran los $4$ displays a la vez mostrarían el mismo dígito. El software aplica **multiplexación por barrido periódico rápido**: coloca el dígito `'2'` en `PORTD`, activa solo el transistor de `DIS1` (`RA2 = 1`), lo mantiene un instante, lo apaga y repite sucesivamente con `'0'` (`RA3`), `'4'` (`RA4`) y `'8'` (`RA5`) suficientemente rápido para engañar a la retina humana por **persistencia visual**.

---

> [!success]- Solución y Rúbrica — Problema B4 ($1.50\text{ puntos}$)
>
> ### Apartado 4.1 — `Timer0` y base de $1\text{ s}$ ($0.50\text{ pts}$)
> 1. **Periodo y desbordamiento ($0.15\text{ pts}$):** Por el reloj interno de $4$ fases ($Q_1$–$Q_4$) y *pipeline* de $2$ etapas del PIC, `Timer0` avanza a $f_{\text{Timer0}} = f_{\text{osc}}/4 = 4\text{ MHz}/4 = 1\text{ MHz} \implies T_{\text{tick}} = \mathbf{1\ \mu\text{s}}$. Como es un contador de $8\text{ bits}$ ($0..255$), al pasar de $255 \rightarrow 0$ genera acarreo (`TMR0IF = 1`) cada **$256\ \mu\text{s}$**.
> 2. **Precarga para $250\ \mu\text{s}$ ($0.15\text{ pts}$):** $\text{Precarga} = 256 - 250 = \mathbf{6}$ (cargar `Timer0 = 6` al inicio y en cada ISR para saltarse los $6$ ticks sobrantes).
> 3. **Cuenta de $1\text{ s}$ y tipo de dato ($0.20\text{ pts}$):** $1\text{ ms} = 4 \times 250\ \mu\text{s} \implies 1\text{ s} = 4 \times 1000 = \mathbf{4000\text{ interrupciones}}$. Como $4000 > 255$ (exige al menos $\lceil \log_2 4000 \rceil = 12\text{ bits}$), no cabe en `Byte` ($8\text{ b}$): es obligatorio usar **`Word` ($16\text{ bits}$, sin signo $0..65\,535$)**.
>
> ### Apartado 4.2 — Diagnóstico de errores y autointerrupción ($0.50\text{ pts}$)
> - **Consecuencia del Error 1 ($0.20\text{ pts}$):** Como **`Timer0` nunca se para** y **`TMR0IF` no se limpia automáticamente por hardware**: (a) antes de habilitar ya estará `TMR0IF = 1` de vueltas previas, causando un **disparo espurio inmediato** nada más poner `GIE = 1`; (b) al no poner `TMR0IF = 0` dentro de la ISR, al salir con `RETFIE` el *flag* sigue a `1` y **vuelve a entrar inmediatamente en un bucle infinito de interrupciones** bloqueando el programa principal.
> - **Consecuencia del Error 2 ($0.15\text{ pts}$):** Como el PIC solo tiene un **vector único (`0x0004`)** y discrimina los orígenes por ***polling*** en `INTCON`, si `Timer0` (`INTCON.2`) y el pulsador `RB0` (`INTCON.1`) activan su petición simultáneamente, un `Else If` excluyente solo atendería a `Timer0` y dejaría `INTF = 1` sin atender en esa entrada (obligando a otra reentrada innecesaria o perdiendo prioridad/latencia; deben ser dos `If` independientes).
> - **Autointerrupción y solución hardware ($0.15\text{ pts}$):** Si `INT` sigue a `1` al acabar la 1.ª instrucción de la ISR, la CPU volvería a interrumpirse a sí misma entrando dos veces seguidas en la ISR (BSOD / Kernel Panic). En el PIC el hardware pone automáticamente **`GIE = 0`** al entrar en la interrupción (enmascaramiento global automático), bloqueando nuevas interrupciones hasta que la instrucción final **`RETFIE`** recupera `PC` de la pila y vuelve a poner **`GIE = 1`**.
>
> ### Apartado 4.3 — Medición del bloque $S$ de $\sim 300$ instrucciones ($0.50\text{ pts}$)
> 1. **Por qué falla la lectura simple ($0.15\text{ pts}$):** `Timer0` es de $8\text{ bits}$ ($0..255$); con $\sim 300$ instrucciones **desbordará y volverá a `0`**, leyendo $\sim 44$ sin poder distinguir si pasaron $44$ o $256 + 44 = 300$ instrucciones.
> 2. **Por qué NO sirve *polling* software de `TMR0IF` ($0.15\text{ pts}$):** Porque insertar código de comprobación dentro de $S$ se ejecuta en la **misma CPU** y **perturba/altera el sistema bajo test**, cambiando el número de instrucciones y el tiempo que queremos medir.
> 3. **Perturbación con interrupción y corrección ($0.20\text{ pts}$):** Como `Timer0` **nunca se para**, seguirá incrementándose mientras se ejecuta el **cambio de contexto** que inserta el compilador en **`ORG 0x0004`** (**$4$ instrucciones de guardado** de `W`/`STATUS` al entrar + cuerpo de la ISR + **$4$ instrucciones de restauración** y `RETFIE` al salir). Al entrar en la primera línea de la ISR leeremos `4` (o `5` si dio tiempo a contar $+1$ antes de entrar), por lo que debemos restaurar `Timer0` al salir descontando las instrucciones de contexto y la ISR (además del pequeño ajuste hardware de escritura en `Timer0`).
