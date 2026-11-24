---
asignatura: fundamentos-computadores
tema: 2
parcial: 1
fuentes:
  - "tema_2.md (Diap. 1-91)"
  - "C. Transcripción 5.md"
  - "C. Transcripción 6.md"
  - "C. Transcripción 7.md"
  - "C. Transcripción 8.md"
  - "C. Transcripción 9.md"
  - "C. Transcripción 10.md"
  - "C. Transcripción 11.md"
  - "C. Transcripción 12.md"
  - "C. Transcripción 13.md"
  - "C. Transcripción 14.md"
  - "C. Transcripción 17.md"
  - "C. Transcripción 20.md"
  - "C. Transcripción 21.md"
  - "C. Transcripción 22.md"
estado: listo-para-estudio
---

# Tema 2: Representación de la Información (MOC)

> [!info] Alcance Exacto en el Primer Parcial
> Debido a la sobrematriculación ($\sim 120$ alumnos), **este curso se suprime el test en ordenador de Representación de la Información** y sus ejercicios pasan al **examen escrito en papel del Primer Parcial** (*Clases 8, 10, 20 y 22*).
>
> | Estado | Contenidos del Tema 2 |
> | :--- | :--- |
> | **✅ SÍ ENTRA (Práctico + Teórico)** | • **Representación alfanumérica** (ASCII 7 bits, caracteres de control, `CR+LF` vs `LF`, Unicode vs UTF-8, cadenas en C con `NULL`, ficheros texto vs binarios y dimensionamiento de buffers).<br>• **Conversiones e interpretación de enteros a mano ($8$–$10$ bits):** Binario Natural, Signo-Magnitud, Complemento a 2, Exceso a $M$ y decodificación BCD.<br>• **Teoría de Punto Fijo y Coma Flotante (IEEE 754):** campos, signo en MSB, exponente en Exceso $127$, `1.` implícito, **los 4 casos especiales (`±0`, desnormalizados, `±∞`, `NaN`)**, prohibición de `==` y enteros escalados.<br>• **Códigos redundantes:** Distancia de Hamming ($M=1,2,3$), paridad simple y **Código Hamming SEC ($7,4$)**. |
> | **❌ EXCLUIDO DEL EXAMEN ESCRITO** | • **Algoritmo y circuito de suma en BCD** (Diap. $38$–$44$, *Clase 8*).<br>• **Ejercicios numéricos a mano de conversión a/desde Coma Flotante / IEEE 754 / `EJ1`** (Diap. $57$–$61$ y $66$–$70$, *Clases 8, 10, 12, 20 y 22*).<br>• **Código Hamming SEC/DED** (Diap. $87$–$88$, *Clase 22*).<br>• ***Endianness* (Little-Endian vs Big-Endian)** (*Clase 7*).<br>• **Diseño interno de puertas de sumadores/restadores** (tratados como *caja negra* por falta de Electrónica Digital previa). |

---

## Mapa de Sub-notas del Tema 2

El Tema 2 ($91$ diapositivas oficiales + $8$ clases principales divididas en dos bloques cronológicos, Clases $5$–$8$ y $10$–$13$) se organiza en **4 sub-notas atómicas completas**:

1. **[[2.1_Introduccion_y_Representacion_Alfanumerica|2.1. Introducción y Representación Alfanumérica]]** *(Diap. 1–16 | Clases 5, 6 y 7)*
   - Espacio físico de representación (binario, finito, byte/palabra).
   - **ASCII de 7 bits** (siempre $8\text{ bits}$ en memoria), caracteres no imprimibles (`TAB`, `CR`, `LF`, `NULL`, `EOT`) y finales de línea por SO (Windows vs Linux vs Mac).
   - Páginas de códigos, **Unicode ($2\text{ B}$ fijos)** vs **UTF-8 ($1$–$5\text{ B}$)**, HTML del Campus Virtual UMA y *Magic Number* (`BOM`).
   - Longitud de cadenas en C/MIPS (`.ascii` vs `.asciiz`, terminador `NULL`) vs Python; ficheros de texto vs binarios (`.pkl`); problema completo del **Sensor IoT**.
2. **[[2.2_Enteros_Posicionales_y_No_Posicionales|2.2. Representación de Enteros (Posicionales y No Posicionales)]]** *(Diap. 17–48 | Clases 7, 8, 9, 10, 17 y 21)*
   - **Posicionales:** Binario Natural (conversión rápida por pesos, límites $255$ y $65\,535$, *underflow* en PIC), Signo-Magnitud (doble cero) y Complemento a 2 (rango asimétrico $[-128, +127]$).
   - **No posicionales:** Exceso a $M$ (sensor de temperatura y Kelvin) y BCD (patrones inválidos, sin negativos, bus ARINC 429; suma BCD marcada como excluida).
   - *Endianness* (Little vs Big Endian, solo informativo/prácticas).
   - Tablas resueltas paso a paso (Diap. $45$–$48$) y problema completo del **Velocímetro de avión de pasajeros**.
3. **[[2.3_Reales_Punto_Fijo_y_Coma_Flotante_IEEE754|2.3. Reales en Punto Fijo y Coma Flotante (IEEE 754)]]** *(Diap. 49–70 | Clases 10, 11, 12, 20 y 22)*
   - **Punto Fijo:** resolución uniforme $2^{-d}$, error $e_a \le 2^{-d}$, conversión de $0.35_{10}$, regla de oro `abs(A - B) < tol` (jamás `==`), y enteros escalados en microcontroladores sin FPU.
   - **Coma Flotante e IEEE 754:** problema `.` vs `,` en CSV, resolución no uniforme, signo en bit $31$ (MSB), exponente en Exceso $127$ (comparación rápida), bit `1.` implícito, simple ($32\text{ b}$) vs doble precisión ($64\text{ b}$) y **los 4 casos especiales (`±0`, desnormalizados, `±∞`, `NaN`)**.
4. **[[2.4_Codigos_Redundantes_Paridad_y_Hamming|2.4. Códigos Redundantes: Paridad y Hamming SEC]]** *(Diap. 71–91 | Clases 12, 13, 14, 20 y 22)*
   - Motivación espacial (Rovers en Marte sin campo magnético), Distancia de Hamming $M$ y teorema $M \ge C + D + 1$.
   - **Códigos con paridad ($M=2$):** modalidades, demostración de $M=2$ y circuitos generador/detector en `circuitikz` (puertas XOR).
   - **Código Hamming SEC ($M=3$, $(7,4)$):** posicionamiento en potencias de $2$, regla matricial de pizarra, síndrome $(C_4 C_2 C_1)_2$ y ejercicios oficiales resueltos (Diap. $89$–$90$). Referencia a SEC/DED (excluido de examen), BCH, Reed-Solomon y CRC.

---

## Radar Rápido de Examen — Tema 2

> [!danger] Aviso / Trampa de Examen
> 1. **Conversiones de enteros ($100\%$ garantizado):** Practicar con el generador automático del Campus Virtual (*"Si pretendéis aprender a hacerlo en el momento del examen, os vais a estrellar"*, *Clase 10*). Memorizar pesos de 1 byte ($1, 2, 4, 8, 16, 32, 64, 128, 256$) y límites ($255$ y $65\,535$).
> 2. **Trampas de alfanuméricos ("donde siempre picáis"):**
>    - ASCII de $7\text{ bits}$ ocupa **$8\text{ bits}$ ($1\text{ byte}$)** en memoria.
>    - En C/MIPS sumar siempre **$+1\text{ byte}$ por el terminador `NULL` ($\text{0x00}$)** y contar el signo `-` como un carácter más.
>    - Contar delimitadores de línea según SO: **`CR+LF` ($2\text{ B}$) en Windows** vs **`LF` ($1\text{ B}$) en Linux**.
>    - **ASCII vs UTF-8 para números/dígitos:** **$0\text{ bytes}$ de diferencia** (ambos usan $1\text{ byte}$ para los primeros $128$ códigos).
> 3. **Trampas de IEEE 754:** Dividir por cero da **`NaN`**, **NO** $+\infty$ (*Clase 12*). El signo va en el MSB (bit $31$) separado de la mantisa para mantener el mismo convenio que C2 y SM (*Clase 12*).
> 4. **Hamming SEC sí, SEC/DED no:** Saber construir $P_1, P_2, P_4$ y diagnosticar el bit erróneo con el síndrome $C_4 C_2 C_1$ (*Clases 13 y 22*).

---

## Conexiones con el Grafo del Primer Parcial

- [[Tema_1_Indice]] (Tema 1): matriz binaria de memoria, direccionamiento por bytes desde `0` y notación hexadecimal.
- [[Formato de Instrucción]] y [[Arquitectura MIPS]] (Tema 3): los campos `OPCODE` y registros se codifican en [[Binario Natural]]; los desplazamientos de salto hacia atrás se codifican en [[Complemento a 2]]; `syscall 4` en MARS exige cadenas `.asciiz`.
- [[Entrada y Salida]] y [[Timer0]] (Tema 4): dimensionamiento de variables en el PIC (`Bit` empaquetado, `Byte` sin signo con riesgo de *underflow*, `Word` de $16\text{ bits}$ para contar $5000$ ticks de $1\text{ s}$ o $4000\text{ ms}$ en los motores de la máquina de *vending*).

---

## Checklist de Dominio del Tema 2

- [ ] Calcular sin errores tamaños de buffers de texto (ASCII/UTF-8/Unicode, `CR+LF` vs `LF`, `NULL`, signo `-`) frente a binario puro.
- [ ] Convertir en menos de $2\text{ minutos}$ cualquier cadena de $8\text{ bits}$ o decimal (positivo/negativo) entre BN, SM, C2, Exceso $128$ y BCD.
- [ ] Justificar en $2$–$3$ líneas: por qué el signo en IEEE 754 está en el bit $31$, por qué el exponente usa Exceso $127$, qué aporta el `1.` implícito y cuándo aparecen `±0`, desnormalizados, `±∞` y `NaN`.
- [ ] Explicar por qué no se usa `if (A == B)` en reales y cuándo conviene punto fijo (enteros escalados).
- [ ] Codificar cualquier dato de $4\text{ bits}$ en **Hamming SEC ($7,4$)** y localizar/corregir un error de $1\text{ bit}$ usando el síndrome $(C_4 C_2 C_1)_2$.
