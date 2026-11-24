---
asignatura: fundamentos-computadores
parcial: 1
tipo: chuleta-supervivencia
fuentes:
  - "01_Mapa_y_Estrategia/01_Radar_de_Examen.md"
  - "02_Apuntes_Maestros/Tema_1/ (completo)"
  - "02_Apuntes_Maestros/Tema_2/ (completo)"
  - "02_Apuntes_Maestros/Tema_3/ (completo)"
  - "02_Apuntes_Maestros/Tema_4/ (completo)"
estado: listo-para-estudio
---

# ⚡ Chuleta de Supervivencia — Primer Parcial (Fundamentos de Computadores)

> [!danger] Reglas de Examen (Grábatelo antes de abrir el cuadernillo)
> - **Formato:** Papel y lápiz, **SIN calculadora**, **NO es tipo test**. Normalmente **$10$ preguntas** ($1\text{ pt}$ c/u, o alguna doble de $2\text{ pts}$; nunca preguntas de $3\text{ pts}$). Parcial eliminatorio si $\ge 5.0$.
> - **REGLA DE ORO ABSOLUTA:** **"Siempre justifica la respuesta."** Responder "Sí", "No" o dar un número aislado sin razonar **vale $0$ puntos** aunque aciertes.
> - **NO PIERDAS TIEMPO HOY ESTUDIANDO ESTO (Excluido del examen escrito):**
>   1. Suma en BCD y circuito sumador BCD (T2, Diap. $38$–$44$).
>   2. Conversiones numéricas a mano de Coma Flotante / IEEE 754 / `EJ1` (T2, Diap. $57$–$61$, $66$–$70$).
>   3. Código Hamming **SEC/DED** (T2, Diap. $87$–$88$).
>   4. *Endianness* y lecturas desalineadas en papel (solo prácticas de MARS).
>   5. Procesador monociclo r-MIPS y segmentación MIPS de $5$ etapas / riesgos / `NOPs` (T3, Diap. $56$–$121$). *(¡CUIDADO: las Diap. $122$–$124$ del PIC16F87XA SÍ ENTRAN!)*
>   6. **DMA completo** (T4, Diap. $52$–$78$, pasa al 2.º parcial).

---

## 1. Tema 1 — Computador, Von Neumann vs Harvard y Ciclo FDE

### 1.1. Ecuación estructural y definiciones relámpago
$$\text{CPU} = \text{UC (Unidad de Control)} + \text{UD (Unidad de Datos / ALU)} + \text{Registros}$$
- **3 manipulaciones:** **Almacenar** (Memoria), **Procesar** (CPU), **Transmitir** (E/S).
- **Test "¿Es computador?" (HW + SW sin recablear):**
  - *Dirección de tiro naval (1.ª GM):* **NO** (función fija, cambiar algo exige tocar HW).
  - *Telar de Jacquard:* **SÍ**, mecánico de **propósito específico** (cambia tela cambiando tarjetas perforadas), **NO** de programa almacenado (tarjetas externas).
  - *ENIAC:* exigía recablear tableros $\rightarrow$ **Von Neumann (1945)** introduce **programa almacenado** puramente electrónico.
  - *Propósito general:* ejecuta cualquier algoritmo **siempre que tenga los periféricos adecuados** (ej. resistencia para calentar vasija química).

### 1.2. Von Neumann vs Harvard y el Ciclo de Instrucción (`FDE`)
- **Indistinguibilidad ("Pregunta del Millón"):** En Von Neumann instrucciones y datos viven en la **misma memoria** como cadenas de `0` y `1` indistinguibles. Los distingue la **fase del ciclo**:
  - **`Fetch`** (dirección generada por **`PC`** $\rightarrow$ `MAR` $\rightarrow$ lectura hacia `IR`, luego `PC` avanza $+1$ o $+4\text{ B}$) $\implies$ **Instrucción**.
  - **`Execute`** (dirección procedente del operando) $\implies$ **Dato**.
  - *Si `PC` salta a datos:* la CPU intenta ejecutarlos como código $\rightarrow$ cuelgue / *segmentation fault* / *hackeo*.
- **RAM vs ROM/Flash:** RAM = **volátil** (proteger con SAI/batería o condensadores en SSD); ROM/Flash = **no volátil** (al encender, `PC = 0` apunta a ROM con el *bootloader*; apagar actualizando BIOS/Smart TV = *"ladrillo"*). **`HALT = 0000`** por seguridad (RAM vacía llena de ceros para limpia la CPU).
- **Traducción:** $\text{Alto Nivel} \xrightarrow{\text{Compilador}} \text{Ensamblador} \xrightarrow{\text{Ensamblador}} \text{Módulos Objeto} \xrightarrow{\text{Linker}} \text{Ejecutable} \xrightarrow{\text{Loader}} \text{RAM}$.
- **2 casos reales de ensamblador:** (1) ingeniería inversa sin fuente (dron interceptado); (2) tiempo real estricto (AVE a $350\text{ km/h}$).

> [!tip] Pregunta de Matrícula (T1 + T3 Diap. 122–124): Solapamiento en PIC16F877A
> - El **PIC16F877A** es **arquitectura Harvard** (Flash de programa de **$14\text{ b}$** separada de RAM de datos de **$8\text{ b}$**), **NO Von Neumann**.
> - Reloj externo dividido entre $4$ ($Q_1$–$Q_4$): $T_{\text{CY}} = \frac{4}{f_{\text{osc}}}$ ($1\ \mu\text{s}$ a $4\text{ MHz}$).
> - Cada instrucción tarda $8$ ciclos ($4$ *Fetch* + $4$ *Execute*), pero el ***pipeline* de $2$ etapas** solapa `Fetch(PC+1)` con `Execute(PC)` $\Rightarrow$ **$1$ instrucción cada $4$ ciclos** (saltos `CALL`/`GOTO` fuerzan `Flush`/`NOP` $\Rightarrow 2\text{ TCY}$).
> - **¿Por qué Von Neumann NO puede solaparlos?** Porque durante *Execute* puede leer/escribir datos en memoria a la vez que *Fetch* lee la instrucción siguiente $\Rightarrow$ **colisión de $2$ accesos simultáneos a la única memoria**.

---

## 2. Tema 2 — Representación de la Información

### 2.1. Alfanuméricos, Cadenas y Buffers (¡Trampas clásicas!)

| Estándar | Tamaño / carácter | Detalles y trampas clave |
| :--- | :---: | :--- |
| **ASCII $7\text{ bits}$** | **$1\text{ B}$ ($8\text{ b}$, MSB=`0`)** | Solo $128$ símbolos ($0..127$, inglés; sin `¿` ni tildes). **¡Nunca $7\text{ bits}$ en RAM!** |
| **Unicode (UCS-2)** | **$2\text{ B}$ fijos** | Todos los idiomas; regularidad de buffer en apps muy internacionalizadas; lleva cabecera **BOM** ($2\text{ B}$). |
| **UTF-8** | **$1$ a $5\text{ B}$** *(hoy $1$–$4$)* | **Mismos caracteres que Unicode.** Primeros $128$ (dígitos `'0'`–`'9'`, inglés, `NULL`) = **$1\text{ B}$ (¡0 B de diferencia con ASCII!)**. Tildes (`á`, `ó`) y Kanji = $2\text{ B}$; Hiragana/Katakana = $3\text{ B}$. Sin BOM. |

- **Códigos ASCII que conviene recordar:** `NUL` = `0x00` ($\neq$ `'0'` = `0x30`), `TAB` = `0x09`, `LF` = `0x0A`, `CR` = `0x0D`, `Space` = `0x20`, `'-'` = `0x2D` (**¡el signo menos ocupa $1\text{ B}$!**).
- **Fines de línea:** **Windows** = `CR+LF` (**$2\text{ B}$**); **Linux/Unix** = `LF` (**$1\text{ B}$**); **Mac clásico** = `CR` ($1\text{ B}$).
  - Receptor Linux ($7\text{ B}$) recibiendo de Windows ($9\text{ B}$) $\Rightarrow$ **desbordamiento y pérdida de datos**.
- **Cadenas en C / MIPS:** terminan en **`NULL` (`0x00`) $\Rightarrow$ sumar siempre $+1\text{ byte}$**. En MARS usar **`.asciiz`** (no `.ascii`, que imprime basura con `syscall 4`). En Python las cadenas son objetos con metadato de longitud.
- **Texto vs Binario:** en binario puro, $5$ enteros $< 255$ ocupan **$5\text{ B}$** (tamaño fijo, sin delimitadores) frente a $21\text{ B}$ en texto Windows.

### 2.2. Tabla de Pesos, Potencias y Hexadecimal (Sin Calculadora)

| Pesos $1\text{ byte}$ | $2^7$ | $2^6$ | $2^5$ | $2^4$ | $2^3$ | $2^2$ | $2^1$ | $2^0$ |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Decimal** | **$128$** | **$64$** | **$32$** | **$16$** | **$8$** | **$4$** | **$2$** | **$1$** |

- **Límites clave:** $1\text{ B}$ ($8\text{ b}$): $0..255$ ($2^8 = 256$); $10\text{ b}$: $2^{10} = 1024$ (en RAM exige **$2\text{ B}$**); $2\text{ B}$ ($16\text{ b}$): $0..65\,535$ ($2^{16} = 65\,536$).
- **Hexadecimal ($4\text{ bits} = 1\text{ dígito}$):** `A=10 (1010)`, `B=11 (1011)`, `C=12 (1100)`, `D=13 (1101)`, `E=14 (1110)`, `F=15 (1111)`.

### 2.3. Mapa Maestro de Enteros ($n$ bits y caso $n = 8$)

| Sistema | Rango general ($n$ b) | Rango $8\text{ bits}$ | Codificación / Decodificación | Caso trampa `10000000` | Notas clave |
| :--- | :---: | :---: | :--- | :---: | :--- |
| **Binario Natural (BN)** | $[0,\; 2^n - 1]$ | $[0,\; 255]$ | Suma de pesos $2^i$ (método rápido: restar pesos). | **$128$** | Tipos `Byte`/`Word` PIC (**underflow:** $0 - 1 = 255$). |
| **Signo-Magnitud (SM)** | $[-(2^{n-1}-1),\; 2^{n-1}-1]$ | $[-127,\; +127]$ | MSB = signo (`0`+, `1`-); resto = $\|X\|$ en BN. | **$-0$** | **Doble cero** (`00000000` y `10000000`). Extensión: mover MSB y rellenar con `0`. |
| **Complemento a 2 (C2)** | $[-2^{n-1},\; 2^{n-1}-1]$ | **$[-128,\; +127]$** | Negativos: $\overline{\|X\|} + 1$. Inversa: $-1$ e invertir (o $\text{C1}+1$). Peso MSB: $-2^{n-1}$. | **$-128$** | **Rango asimétrico** (cero único). HW simple (MIPS). Extensión: **replicar MSB**. $\text{ovf} = C_{n-2} \oplus C_{n-1}$. |
| **Exceso a $M$** | $[-M,\; 2^n - 1 - M]$ | $[-128,\; +127]$ *(si $M=128$)* | Cod.: $(X + M)_2$. Dec.: $V_{\text{nat}} - M$. | **$0$** *(si $M=128$)* | Orden binario natural continuo $\rightarrow$ exponente IEEE 754 ($M=127$). **Truco:** para $M=128$, es C2 con el MSB invertido. |
| **BCD** | $0..9$ por cuarteto ($4\text{ b}$) | $00..99$ | Cada dígito decimal en $4\text{ bits}$ BN. | **$80$** | **`1010`–`1111` INVÁLIDOS**; **NO hay negativos**. Usado en bus aviónica ARINC 429. |

### 2.4. Reales: Punto Fijo e IEEE 754 (Solo Teoría + 4 Casos Especiales)
- **Punto Fijo ($e$ enteros, $d$ fraccionarios):** Resolución **uniforme** $= 2^{-d}$, error $e_a \le 2^{-d}$.
  - **Prohibido `if (A == B)`** (ej. $0.35_{10}$ es periódico infinito en binario y se trunca) $\implies$ usar **`if (abs(A - B) < 0.001)`**.
  - **En microcontroladores $8\text{ bits}$ sin FPU (PIC/Arduino):** usar **enteros escalados** (multiplicar por $100$ para $2$ decimales, operar en enteros y dividir al mostrar).
- **IEEE 754 Simple Precisión ($32\text{ b} = 4\text{ B}$; Doble $= 64\text{ b} = 8\text{ B}$):**

| Bit $31$ ($1\text{ b}$) | Bits $30\dots 23$ ($8\text{ b}$) | Bits $22\dots 0$ ($23\text{ b}$) | Fórmula Normalizada ($0 < C < 255$) |
| :---: | :---: | :---: | :---: |
| **`S`** (Signo SM) | **`C`** (Exceso **$127$**) | **`M`** (tras `1.` implícito) | $V = (-1)^S \times 1.M_2 \times 2^{C - 127}$ |

- **3 Decisiones de Diseño (Preguntas fijas):**
  1. **Signo en bit $31$ (MSB), separado de `M`:** unificar convenio con C2 y SM (`MSB = 1` negativo).
  2. **Exponente en Exceso $127$ (no C2):** crece monótonamente como BN $\Rightarrow$ **comparador hardware mucho más rápido**.
  3. **Bit `1.` implícito:** regala **$+1\text{ bit}$ extra de precisión** ($24\text{ b}$ efectivos en $23\text{ b}$).
- **Los 4 Casos Especiales de IEEE 754 (MEMORIZAR):**

| $C$ (Dec / Bin) | $M$ | Valor | Cuándo ocurre en la vida real |
| :---: | :---: | :--- | :--- |
| **`0`** (`00000000`) | $= 0$ | **$\pm 0$** | Porque `1.M` normalizado nunca puede ser $0$. |
| **`0`** (`00000000`) | $\neq 0$ | **Desnormalizado:** $(-1)^S \times \mathbf{0.M} \times 2^{\mathbf{-126}}$ | Valores menores que $2^{-126}$ (subdesbordamiento gradual; ¡exponente $-126$, no $-127$!). |
| **`255`** (`11111111`) | $= 0$ | **$\pm\infty$** | Desbordamiento por factor de escala o **asíntota** (dividir por número muy pequeño $\neq 0$). |
| **`255`** (`11111111`) | $\neq 0$ | **`NaN`** (*Not a Number*) | **División por cero ($X/0$)** (¡error, NO infinito!), $0/0$, $\sqrt{-1}$, variables sin inicializar. |

### 2.5. Redundancia, Distancia de Hamming y Hamming SEC $(7,4)$
- **Teorema:** $M \ge C + D + 1$ ($D \ge C$).
  - $M=1$: nada (sin redundancia; radiación solar en Marte cambia un bit y da otra instrucción válida).
  - $M=2$: paridad ($1\text{ bit}$ XOR) $\rightarrow$ **detecta $1$, NO corrige**.
  - $M=3$: **Hamming SEC** $\rightarrow$ **detecta y corrige $1$**.
- **Dimensionamiento Hamming SEC:** $2^P \ge N + P + 1 \implies$ para $N=4$: $P=3$ ($7\text{ bits}$ totales).
- **Posiciones ($1..7$):** Paridades en **potencias de $2$ ($1, 2, 4$)**; orden: **$D_7\; D_6\; D_5\; P_4\; D_3\; P_2\; P_1$**.
- **Truco matricial (sin memorizar — índices en binario):**

| Paridad / Síndrome | $D_7$ (`111`) | $D_6$ (`110`) | $D_5$ (`101`) | $D_3$ (`011`) | Ecuación Emisor (Par) | Ecuación Receptor (Síndrome) |
| :---: | :---: | :---: | :---: | :---: | :--- | :--- |
| **$P_4$ / $C_4$** | `1` | `1` | `1` | `0` | $P_4 = D_7 \oplus D_6 \oplus D_5$ | $C_4 = P_4 \oplus D_7 \oplus D_6 \oplus D_5$ |
| **$P_2$ / $C_2$** | `1` | `1` | `0` | `1` | $P_2 = D_7 \oplus D_6 \oplus D_3$ | $C_2 = P_2 \oplus D_7 \oplus D_6 \oplus D_3$ |
| **$P_1$ / $C_1$** | `1` | `0` | `1` | `1` | $P_1 = D_7 \oplus D_5 \oplus D_3$ | $C_1 = P_1 \oplus D_7 \oplus D_5 \oplus D_3$ |

- **Diagnóstico:** $(C_4 C_2 C_1)_2 = 000_2 \Rightarrow$ sin error; si $\neq 000_2$, el número en BN **indica el índice exacto ($1..7$) del bit erróneo** $\Rightarrow$ invertir ese bit.

---

## 3. Tema 3 — Formato de Instrucción, Modos y Arquitectura MIPS

- **Cálculo de campos:** Para $N$ elementos numerados desde `0` en BN $\implies k = \lceil \log_2 N \rceil\text{ bits}$ ($64 \rightarrow 6\text{ b}$, $32 \rightarrow 5\text{ b}$, $9 \rightarrow 4\text{ b}$).
  - **Cuidado con $MD$:** Solo cuenta los modos que puede adoptar *ese operando concreto* (si `OP1` es siempre registro $\Rightarrow 0\text{ b}$ de modo; si `OP2` elige entre $2$ modos $\Rightarrow 1\text{ bit}$).
- **Saltos relativos hacia atrás:** desplazamiento negativo codificado en **Complemento a 2** (ej. retroceder $3 \Rightarrow -3$ en C2).
- **Modos de Direccionamiento en MIPS (¡Atención a modos mixtos en la misma instrucción!):**
  - **Inmediato:** el campo operando es el propio valor (`addi`, `ori`).
  - **Directo Absoluto a Registro:** índice de registro (`add`, `sub`, y **`$s1, $s2` en `beq`**).
  - **Directo Absoluto a Memoria:** **NO existe en MIPS** (sí existe en el **PIC16F877A**, donde RAM y registros conviven en bancos con acumulador único `W`).
  - **Directo Relativo con Registro Implícito (`PC`):** $DE = PC + \text{desp}$ (**destino de `beq`/`bne`**; `PC` no es accesible al usuario).
  - **Directo Relativo con Registro Explícito (*Base*):** $DE = R[\text{rs}] + \text{desp}$ (**`lw`/`sw`/`lb`/`sb`**, ej. `20($s2)`).
  - **Indirecto:** "un saltito más" (dirección de dirección); no existe en MIPS.
- **MIPS = RISC + *Load-Store* ($32\text{ b}$, $32$ registros, `$0` siempre `0`):** ALU solo opera con registros $\Rightarrow$ obligatorio `lw`/`lb` antes y `sw`/`sb` después.
  - **Formatos ($32\text{ b}$):** **R:** `op(6)=0 | Rs(5) | Rt(5) | Rd(5) | shamt(5) | funct(6)`; **I:** `op(6) | Rs(5) | Rt(5) | Imm(16)`; **J:** `op(6) | Target(26)`.
  - **Pseudoinstrucción `li` de $32\text{ b}$:** no cabe en $32\text{ b}$ $\Rightarrow$ MARS genera **`lui` + `ori`**. Coherencia: `.word` $\leftrightarrow$ `lw` (`+4`), `.byte` $\leftrightarrow$ `lb` (`+1`).

---

## 4. Tema 4 — Organización de la E/S (Sin DMA)

### 4.1. Reglas Nemotécnicas y Hardware Eléctrico (PICGenios)
- **Sentido CPU-céntrico:** **`R` (*Read*) = Entrada** (Perif $\rightarrow$ CPU); **`W` (*Write*) = Salida** (CPU $\rightarrow$ Perif).
- **Nemotecnia `TRISB`:** **`0` = `O`utput (Salida)**, **`1` = `I`nput (Entrada)**.
- **Modelo de Programación:** desacopla HW y SW $\rightarrow$ cambiar motor o pasar de `PIC16F877` a `877A` (o clonar LCD `HD44780`) **sin reescribir el *driver***.
- **Ventilador (`RC2`):** $12\text{ V}$ vs pin $5\text{ V}/25\text{ mA}$ $\implies$ **transistor NPN como interruptor digital** ($\sim 3\text{ mA}$ en base).
- **Resistencia *pull-up* (`RB0`, `RB1`):** fija `1` ($+5\text{ V}$) en reposo y **evita cortocircuito** al pulsar (conecta a `0 V`). Genera **flanco de BAJADA al pulsar** y **flanco de SUBIDA al soltar**.
- **Display $7$-segmentos multiplexado (`1024`):** bus compartido `PORTD` + $4$ transistores `DIS1..DIS4` (`RA2..RA5`) $\implies$ **barrido periódico rápido** (persistencia visual).
- ***Watchdog Timer*:** contador hardware decremental que resetea el micro si el software se cuelga (ej. máquina Coca-Colas).

### 4.2. Colisión de Bus, Decodificación y Mapeada vs Diferenciada
- **Colisión de Bus TTL:** dos dispositivos escribiendo a la vez (`+5 V` y `0 V` sin $R$) = **cortocircuito**. Solución: decodificador + **`CS` (*Chip Select*)** o **Multiplexor** (ARM AHB / **ESP32-C3**, evita corrientes de fuga en batería).
- **Decodificación fina:** `A1, A0` $\rightarrow$ `SEL1, SEL0` ($4$ registros); `A7..A2` $\rightarrow$ `CS #M`, `CS #1`, `CS #2`. Dejar posiciones sobrantes **Reservadas** (`0x93`) por compatibilidad futura.
- **Mapeada (`LB`/`SB`, `LD`/`ST`) vs Diferenciada (`IN`/`OUT`, línea $\overline{M}/IO$):**
  - Mapeada: simple/rápida, pero puntero erróneo puede activar E/S y **fragmenta la RAM** (problema *vending* $60 \times 4 = 240\text{ B}$ $\rightarrow$ agrupar E/S al inicio/final).
  - Diferenciada: sin fragmentación, pero requiere instrucciones especiales (Intel x86, $16\text{ b}$), más lenta.
  - **ATmega48 (modelo mixto):** `0x00–0x3F` con `IN`/`OUT` ($6\text{ b}$), **`+0x20`** con `LD`/`ST`; `0x60–0xFF` **solo** `LD`/`ST`.

### 4.3. PIO, ADC del PIC, Bloqueo vs Periódica y Protocolo 4 Líneas
- **PIO = $2$ instrucciones por dato** (registro CPU intermediario):
  - Lectura mapeada: `LB` + `SB`. **Lectura diferenciada: `IN` + `SB` (¡NUNCA `OUT`!).**
  - Escritura diferenciada: `LB` + `OUT`. **Solo consultar estado: $1$ instrucción** (`LB` o `IN`).
- **ADC $10\text{ b}$ del PIC:** **$2$ registros de datos** (`ADRESH`, `ADRESL`) + registro híbrido **`ADCON0` (`GO/DONE`)** (`1` escribe = arrancar; leer `1` = en curso / `0` = listo). Secuencia: comprobar `0` $\rightarrow$ escribir `1` $\rightarrow$ esperar `0` $\rightarrow$ leer datos.
- **Bloqueo (`while`) vs Periódica vs `sleep`:**
  - *Bloqueo:* latencia mínima (**ABS Toyota híbrido**), pero paraliza monocore (no "Cancelar", salvo multinúcleo/`fork` — la GPU no corre hilos SO) y **CPU al $100\%$ agota batería** (TFG estación meteorológica $\rightarrow$ **`sleep` + interrupción temporizador**).
  - *Periódica:* concurrencia (parpadeo punto decimal), latencia impredecible (ratón/Windows).
- **Protocolo $4$ líneas:** Salida = `DAV` (Interf $\rightarrow$ Perif) / `DAC` (Perif $\rightarrow$ Interf); Entrada = `RTR` (Interf $\rightarrow$ Perif, **se desactiva al recibir dato para proteger `DE`**) / `DENV` (Perif $\rightarrow$ Interf).

### 4.4. Interrupciones, *Polling*, `Timer0` y Controlador Hardware
- **Atención al fin de instrucción:** contexto mínimo = **`PC`** (en PIC el HW apila `PC` y el código en `0x0004` salva `W`/`STATUS` en $4$ inst. + $4$ de retorno).
- **Cuándo *polling* gana a interrupciones:** decenas de miles de INT/s (**TFG péndulo invertido**) $\rightarrow$ el cambio de contexto satura la CPU.
- **Interrupción autointerrumpida:** `INT = 1` tras 1.ª inst. de ISR $\rightarrow$ reentrada doble $\rightarrow$ **BSOD / Kernel Panic**. Soluciones: **(1) disparo por flanco**, **(2) enmascaramiento global automático** (`GIE = 0` al entrar, `RETFIE` pone `GIE = 1` al salir).
- **Vector y `INTA`:** `(1)` `INT` $\rightarrow$ `(2)` CPU activa `INTA` $\rightarrow$ `(3)` periférico pone vector en Bus Datos $\rightarrow$ `(4)` CPU indexa **TVI**. En el **PIC**, vector único **`0x0004`** sin TVI ni `INTA`.
- **Orígenes múltiples por *Polling*:**
  - **OR cableado:** exige **`GND` común + resistencia de protección $R_{\text{prot}}$** por salida.
  - Prioridad = orden de los `if` (fácil de cambiar por SW), pero último de la cola lento (**`ADIF` en `PIR` = $11.\text{º}$ muestreo**). También en SO complejos (`IRQ 9` Windows XP).
  - **`INTCON` = Control (`GIE, PEIE, TMR0IE, INTE, RBIE`) + Estado (`TMR0IF, INTF, RBIF`)**. Filtrar con máscara **`AND != 0`** (o `INTCON.2`).
- **`Timer0` ($8\text{ b}$, $f_{\text{osc}}/4 = 1\ \mu\text{s}$ a $4\text{ MHz}$, $256\ \mu\text{s}$/vuelta):**
  - **Nunca para** y **`TMR0IF` no se limpia solo** (limpiar a `0` antes de habilitar y en cada ISR; `WaitMs 50` por condición de carrera en `INTF`).
  - **Precarga:** $\text{Timer0} = 256 - N \implies$ **`6`** para $250\ \mu\text{s}$ ($4$/ms); **`56`** para $200\ \mu\text{s}$ ($5$/ms). Para $1\text{ s}$ ($5000$ ticks): variable **`Word` ($16\text{ b}$)**.
  - Medir bloque $S > 255$ inst.: usar interrupción `Timer0` (no *polling* SW que altera $S$) descontando $4+4$ inst. de cambio de contexto + ISR.
- **Controlador de Interrupciones (y APIC):** periférico configurado por **PIO** con: **(1) máscaras `AND` individuales**, **(2) codificador con prioridad ($2^n \rightarrow n$; cambiar prioridad exige cambiar de *slot* físico)**, **(3) comparador `NUEVA < ACTUAL` + registro nivel en servicio**, **(4) Registro `Vector Base` + sumador** ($\text{Vector} = \text{Base} + \text{nº}$, imprescindible porque `0, 1, 2, 3...` están reservados para **excepciones internas de la CPU** como división por cero).

---

## 🏆 TOP 10 TRAMPAS DEL PRIMER PARCIAL (Lo que hace perder puntos)

| # | Trampa Clásica / Falsa Ilusión | ❌ Error típico que suspende | ✅ Respuesta Blindada de $10$ |
| :-: | :--- | :--- | :--- |
| **1** | **"Sí / No" sin justificar** | Acierta la respuesta en 1 palabra y deja en blanco el porqué. | **Justificar siempre** en $2$–$3$ líneas el mecanismo físico/arquitectónico. Sin justificación = **$0$ puntos**. |
| **2** | **Dimensionamiento de texto** | Decir que ASCII $7\text{ b}$ ocupa $7\text{ bits}$, olvidar `NULL` (`+1 B`), signo `'-'` o `CR+LF` ($2\text{ B}$), o creer que UTF-8 ocupa más que ASCII para dígitos. | ASCII = **$1\text{ B}$**; sumar signo `'-'`, `NULL` en C/MIPS, `CR+LF` ($2\text{ B}$) en Windows vs `LF` ($1\text{ B}$) en Linux; dígitos en ASCII y UTF-8 ocupan **exactamente lo mismo ($1\text{ B}$)**. |
| **3** | **Tabla de enteros $8\text{ b}$ (`10000000` y BCD)** | Decir `10000000` = `0` en C2 o `-128` en SM, o decodificar `1010`–`1111` / negativos en BCD. | `10000000`: BN=$128$, SM=$-0$, C2=$-128$, Exc.128=$0$, BCD=$80$. En BCD `1010..1111` son **inválidos** y **no existen negativos**. |
| **4** | **División por cero en IEEE 754** | Responder que $X / 0.0$ devuelve $+\infty$. | Devuelve **`NaN`** ($C=255, M \neq 0$) porque es un error matemático. **$\pm\infty$** ($C=255, M=0$) es para **asíntotas** o desbordamiento por escala. |
| **5** | **Bits de modo $MD$ y modos mixtos MIPS** | Usar $\lceil\log_2(\text{total modos})\rceil$ para cada operando, o asignar un solo modo a todo `beq`. | Solo se codifican bits de modo para operandos variables. En `beq`: registros = **directo a registro**; salto = **relativo con registro implícito (`PC`)**. |
| **6** | ***Pipeline* del PIC16F877A = Von Neumann** | Decir que es Von Neumann porque tiene *Fetch* y *Execute*. | Es **Harvard** (Flash $14\text{ b}$ $\neq$ RAM $8\text{ b}$). En Von Neumann solapar ambos produciría **colisión de $2$ accesos simultáneos a la única memoria**. |
| **7** | **`INTCON` / `ADCON0` (`GO/DONE`)** | Responder "solo control" o "solo estado" por su nombre. | Son **registros híbridos: CONTROL y ESTADO a la vez** (`GIE`/`IE` control + `IF` estado en `INTCON`; `GO` control + `DONE` estado en `ADCON0`). |
| **8** | **Línea `M/IO` y 2.ª instrucción PIO** | Concluir diferenciada solo por ver `M/IO`, o escribir `IN` + **`OUT`** en lectura diferenciada. | Mandan las **instrucciones** (`LB`/`SB` = mapeada; `IN`/`OUT` = diferenciada). Lectura diferenciada = **`IN` + `SB`** (el paso 2 escribe en **memoria**, nunca `OUT`). |
| **9** | **Flancos *pull-up* y OR cableado** | Pensar que pulsar da flanco de subida, u olvidar $R_{\text{prot}}$ en el OR cableado. | *Pull-up* $\Rightarrow$ **bajada al pulsar, subida al soltar**. En OR cableado faltan **`GND` común y $R_{\text{prot}}$** para evitar cortocircuito TTL (`+5 V` vs `0 V`). |
| **10** | **`Timer0` y `Vector Base`** | Olvidar limpiar `TMR0IF`, usar `Byte` para $5000$ ticks, o decir que el `Vector Base` es para ampliar más de $4$ periféricos. | `Timer0` nunca para, `TMR0IF=0` manual, $5000$ exige **`Word` ($16\text{ b}$)**. `Vector Base` existe para **no chocar con `0, 1, 2, 3...` reservados a excepciones internas de CPU** (división por cero). |
