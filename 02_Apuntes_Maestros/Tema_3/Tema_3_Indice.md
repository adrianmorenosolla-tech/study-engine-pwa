---
asignatura: fundamentos-computadores
tema: 3
parcial: 1
fuentes:
  - "tema_3_fundamentos_de_computadores.md (Diap. 1-55)"
  - "tema_3_fundamentos_de_computadores_parte_2_diapositivas_56_95.md (Diap. 56-95 — EXCLUIDO)"
  - "tema_3_fundamentos_de_computadores_parte_3_diapositivas_96_125.md (Diap. 96-121 EXCLUIDO; Diap. 122-124 SÍ ENTRA)"
  - "C. Transcripción 2.md"
  - "C. Transcripción 3.md"
  - "C. Transcripción 4.md"
  - "C. Transcripción 6.md"
  - "C. Transcripción 8.md"
  - "C. Transcripción 9.md"
  - "C. Transcripción 13.md"
  - "C. Transcripción 21.md"
  - "C. Transcripción 22.md"
estado: listo-para-estudio
---

# Tema 3: Instrucciones de un Procesador y Programación en Ensamblador (MOC)

> [!info] Alcance Exacto en el Primer Parcial (¡Atención a la excepción de la Parte 3!)
> El profesor reordenó el temario durante el curso para llegar a tiempo a las prácticas de MARS y del PIC16F877A: dio la Parte 1 del Tema 3 en las **Clases 8 y 9** (intercalándola dentro del Tema 2) y recuperó las **Diapositivas 122–124** del final de la Parte 3 en las **Clases 21 y 22**.
>
> | Estado | Contenidos del Tema 3 |
> | :--- | :--- |
> | **✅ SÍ ENTRA EN EL 1er PARCIAL** | • **Parte 1 completa (Diap. $1$–$55$):** Lenguaje máquina y proceso de traducción, tipos de instrucciones, **formato de instrucción e ISA**, diseño de formatos de instrucción (cálculo de bits de `OPCODE`, registros y modos), **taxonomía completa de modos de direccionamiento**, saltos relativos a `PC` en C2, y **arquitectura/ensamblador MIPS** (RISC vs CISC, $32$ registros, *Load-Store*, $5$ modos MIPS, formatos R/I/J y repertorio de instrucciones).<br>• **Parte 3 — EXCEPCIÓN CRÍTICA (Diap. $122$–$124$, *Clases 21 y 22*):** **Arquitectura Harvard del PIC16F87XA**, división del reloj $f_{\text{osc}}/4$ ($Q_1$–$Q_4$), **cauce segmentado (*pipeline*) de $2$ etapas** (*Fetch*/*Execute* solapados), penalización por saltos (`Flush`/`NOP`), **por qué Harvard permite el solapamiento y Von Neumann no**, y medición de tiempos de código con `Timer0`. |
> | **❌ EXCLUIDO / OMITIDO DE LA ASIGNATURA** | • **Apdo. 3 — Tiempo de ciclo en procesador monociclo r-MIPS** (Diap. $56$–$79$, *Parte 2*).<br>• **Apdos. 4, 5 y 6 — Segmentación MIPS de $5$ etapas, dependencias, riesgos (estructurales, control y datos), inserción de `NOPs`, salto retardado y cálculo de tiempo de ejecución segmentado** (Diap. $80$–$95$ de *Parte 2* y Diap. $96$–$121$ de *Parte 3*).<br>• *Citas del profesor:* *"No vamos a perder tiempo hablando de la temporización"* (*Clase 13*) y *"lo tenéis en el Tema 3 en los apuntes, aunque no vemos eso en la asignatura porque se nos sale de madre, pero os lo he dejado en los apuntes por si alguien tiene curiosidad"* (*Clase 22*). |
> | **🧪 SOLO PRÁCTICAS DE MARS (NO examen teórico)** | • **Lecturas desalineadas** en fronteras impares de $3\text{ bytes}$ (*Clase 9*: *"No os preocupéis para el examen... Preocuparos para las prácticas de MIPS"*).<br>• ***Endianness* (Little-Endian en MARS sobre x86)** (*Clases 7 y 9*).<br>• **Modos indirectos y autoindexado** (Diap. $31$–$36$): el profesor explica su definición pero señala que no existen en MIPS y no le preocupan (*Clase 9*). |

---

## Mapa de Sub-notas del Tema 3

El contenido evaluable del Tema 3 ($55$ diapositivas de la Parte 1 + $3$ diapositivas clave de la Parte 3 + Clases $8, 9, 13, 21$ y $22$, más conexiones con MARS en Clases $2, 3, 4$ y $6$) se estructura en **4 sub-notas atómicas sin pérdida de contenido**:

1. **[[3.1_Instrucciones_y_Formato_de_Instruccion|3.1. Instrucciones de un Procesador y Formato de Instrucción]]** *(Diap. 1–10 y 37–43 | Clases 2, 3, 4, 8 y 13)*
   - Lenguaje máquina, proceso de traducción y visión de la memoria binaria (recordatorio de indistinguibilidad instrucción/dato en Von Neumann).
   - Clasificación del conjunto de instrucciones (Transferencia, Transformación, Control).
   - Formato de instrucción e **ISA**, cálculo del tamaño de `OPCODE` y registros en [[Binario Natural]] (analogía de los $9$ alumnos), **problema oficial de diseño de formato de $32\text{ bits}$ resuelto paso a paso** (Diap. $37$–$43$) y desplazamientos de salto negativos en [[Complemento a 2]].
2. **[[3.2_Modos_de_Direccionamiento|3.2. Modos de Direccionamiento]]** *(Diap. 11–36 | Clases 8, 9 y 13)*
   - Taxonomía completa (Inmediato, Directo Absoluto a Registro/Memoria, Directo Relativo con Registro Implícito/Explícito, Autoindexado e Indirecto por Registro/Memoria).
   - Todos los ejemplos oficiales sobre `ADD A, B, C` (`0100 1001 0010 0101`) y bucle de recorrido de memoria (`INC R0`).
   - Contraste real **MIPS vs PIC16F877A** (banco de registros separado vs memoria unificada en bancos con registro `W` y BASIC compilado).
   - **Pregunta clave de examen:** identificación de modos combinados dentro de una misma instrucción MIPS (`beq` y `lw`/`sw`).
3. **[[3.3_Arquitectura_y_Ensamblador_MIPS|3.3. Arquitectura y Lenguaje Ensamblador MIPS (MARS)]]** *(Diap. 44–55 | Clases 2, 3, 4, 6, 8, 9 y 13)*
   - Filosofía **RISC vs CISC**, banco de $32$ registros (`$0`–`$31`, convenio de llamada) y **arquitectura *Load-Store*** con ruta de datos básica.
   - Los $5$ modos de direccionamiento MIPS, los $3$ formatos de $32\text{ bits}$ (**R, I y J**) y el repertorio oficial completo de instrucciones (Diap. $53$–$54$).
   - Guías prácticas de **MARS**: `Compact, Data at Address 0`, coherencia `.word`/`lw` vs `.byte`/`lb`, **pseudoinstrucciones** (`li` de $32\text{ bits} \rightarrow$ `lui` + `ori`), alineamiento en potencias de $2$ y *Little-Endian* sobre x86.
4. **[[3.4_Arquitectura_Harvard_y_Pipeline_PIC16F87XA|3.4. Arquitectura Harvard, Reloj f/4 y Pipeline del PIC16F87XA]]** *(Parte 3, Diap. 122–124 | Clases 9, 14, 21 y 22)*
   - Diagrama de bloques oficial del **PIC16F87XA** (Flash de programa de $14\text{ bits}$ separada de RAM de datos de $8\text{ bits}$).
   - Reloj en cuadratura ($Q_1$–$Q_4$), frecuencia de instrucción $f_{\text{osc}}/4$ y cronograma oficial (`Figure 4-3`).
   - **Cauce segmentado (*pipeline*) de $2$ etapas** (`Example 4-1` con `CALL` $\rightarrow$ `Flush`/`NOP`), **pregunta de matrícula de honor** (por qué Harvard permite solapar *Fetch* y *Execute* y Von Neumann no) y **diseño de medición de tiempos con `Timer0`** (desbordamiento $> 255$ instrucciones e interrupción descontando el cambio de contexto).

---

## Radar Rápido de Examen — Tema 3

> [!danger] Aviso / Trampa de Examen
> 1. **Cálculo de campos en un formato de instrucción (*Clase 8*, Diap. 37–43):** Para $N$ instrucciones o $R$ registros numerados desde `0`, el número de bits en [[Binario Natural]] es $\lceil \log_2 N \rceil$ (ej. $64 \rightarrow 6\text{ bits}$, $32 \rightarrow 5\text{ bits}$, $9 \rightarrow 4\text{ bits}$). ¡Ojo al número de bits del modo de direccionamiento!: si un operando tiene modo fijo (siempre registro) y el otro alterna entre $2$ modos, el campo $MD_{op2}$ solo necesita **$1\text{ bit}$**, no $2$.
> 2. **Desplazamiento negativo en saltos relativos a `PC` (*Clase 8*):** Retroceder $3$ instrucciones en un bucle se codifica como $-3$ en **[[Complemento a 2]]** para sumarlo directamente al `PC`.
> 3. **Modos de direccionamiento mixtos en la misma instrucción MIPS (*Clase 9*):**
>    - En `beq $s1, $s2, desp`: `$s1` y `$s2` usan **directo absoluto a registro**; el destino del salto usa **directo relativo con registro implícito (`PC`)** (el usuario no puede acceder al `PC` directamente para que no corrompa el flujo).
>    - En `lw $s1, 20($s2)`: `$s1` usa **directo absoluto a registro**; `20($s2)` usa **directo relativo con registro explícito** ($DE = R[\text{rs}] + \text{desp}$).
> 4. **Pregunta de sobresaliente / matrícula de honor (*Clase 22*, Diap. 122–124):** Ante el cronograma *Fetch*/*Execute* del **PIC16F877A**, identificar: (a) que muestra el **ciclo de instrucción segmentado (*pipeline* de $2$ etapas)**; (b) que **NO es Von Neumann sino arquitectura Harvard**; y (c) que en Von Neumann no se pueden solapar ambas fases porque *Fetch* (lectura de instrucción) y *Execute* (lectura/escritura de dato) **colisionarían accediendo simultáneamente a la única memoria**.

---

## Conexiones con el Grafo del Primer Parcial

- [[Tema_1_Indice]] (Tema 1): [[Arquitectura Von Neumann]] vs [[Arquitectura Harvard]], [[Ciclo de Instrucción]] (*Fetch–Decode–Execute*), registro `PC`, direccionamiento por bytes desde `0` y cuándo programar en ensamblador (ingeniería inversa y tiempo real).
- [[Tema_2_Indice]] (Tema 2): todos los campos del formato de instrucción reutilizan enteros del Tema 2 ([[Binario Natural]] para `OPCODE`/registros y [[Complemento a 2]] para inmediatos y saltos negativos); cadenas `.asciiz` en `syscall 4` de MARS; *Little-Endian*.
- [[Entrada y Salida]], [[Interrupciones]] y [[Timer0]] (Tema 4): la arquitectura Harvard y el ciclo $T_{\text{CY}} = 4/f_{\text{osc}}$ del PIC16F87XA justifican por qué `Timer0` incrementa a $f_{\text{osc}}/4$ ($1\ \mu\text{s}$ a $4\text{ MHz}$), cómo se miden tiempos de segmentos críticos con interrupciones y el modelo de memoria unificada (`PORTB`, `TRISB`, `INTCON`) en el PIC.

---

## Checklist de Dominio del Tema 3

- [ ] Diseñar un formato de instrucción de $n$ bits calculando exactamente la anchura de `OPCODE`, operandos de registro, selector de modo de direccionamiento y campos de dirección/desplazamiento.
- [ ] Dada una instrucción binaria ficticia (tipo `ADD A, B, C`) y unas tablas de memoria y registros, calcular el operando obtenido bajo cualquiera de los modos de direccionamiento (Inmediato, Directo a Registro, Directo a Memoria, Relativo Implícito, Relativo Explícito e Indirectos).
- [ ] Identificar los modos de direccionamiento de cada operando en instrucciones reales de MIPS (`add`, `addi`, `lw`, `sw`, `beq`, `j`).
- [ ] Explicar las características de MIPS (RISC, $32$ registros, *Load-Store*, formatos R/I/J, pseudoinstrucción `li` $\rightarrow$ `lui` + `ori`, coherencia `.word`/`lw` vs `.byte`/`lb`).
- [ ] Justificar con rigor el cronograma del PIC16F87XA: por qué $f_{\text{osc}}/4$, cómo el *pipeline* de $2$ etapas logra $1$ instrucción cada $4$ ciclos (salvo saltos con `Flush`/`NOP`), por qué exige arquitectura **Harvard** frente a **Von Neumann** y cómo medir el tiempo de un bloque de código con `Timer0`.
