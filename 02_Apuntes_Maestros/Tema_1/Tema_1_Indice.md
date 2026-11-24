---
asignatura: fundamentos-computadores
tema: 1
parcial: 1
fuentes:
  - "tema_1_fundamentos_de_computadores.md"
  - "tema_3_fundamentos_de_computadores_parte_3_diapositivas_96_125.md (Diap. 122-124)"
  - "C. Transcripción 1.md"
  - "C. Transcripción 2.md"
  - "C. Transcripción 3.md"
  - "C. Transcripción 4.md"
  - "C. Transcripción 5.md"
  - "C. Transcripción 14.md"
  - "C. Transcripción 15.md"
  - "C. Transcripción 22.md"
estado: listo-para-estudio
---

# Tema 1: Introducción a los Computadores (MOC)

> [!info] Alcance en el Primer Parcial
> **Entra COMPLETO** (Diapositivas $1$ a $35$ de las transparencias oficiales), **más** la conexión directa que realizó el profesor en la **Clase 22** contrastando la arquitectura **Von Neumann** frente a la **Arquitectura Harvard** sobre el ciclo de instrucción segmentado (*pipeline* de $2$ etapas) del microcontrolador **PIC16F877A** (Diapositivas $122$–$124$ del Tema 3).

---

## Mapa de Sub-notas del Tema 1

Este tema se ha desglosado en **4 notas atómicas** siguiendo la columna vertebral oficial de las transparencias y conservando íntegramente todos los ejemplos de pizarra, simulaciones, dudas resueltas y avisos de examen:

1. **[[1.1_Que_es_un_Computador|1.1. ¿Qué es un Computador?]]** *(Diap. 1–10 | Clases 1 y 2)*
   - Definición formal de computador (**HW** vs **SW**, instrucciones, programa y tipos de manipulación: almacenar, transmitir y procesar).
   - Propósito específico vs propósito general (vasija de reacción química).
   - Casos históricos de pizarra: dirección de tiro naval de la 1ª GM (**NO es computador**), telar de Jacquard (**SÍ es computador**, de propósito específico), ENIAC vs máquina de programa almacenado de **John Von Neumann ($1945$)**.
   - Contexto de la asignatura y plataformas de prácticas: **MARS (MIPS)** y **PICSimLab / PIC Simulator IDE (PIC16F877A)**.
2. **[[1.2_Arquitectura_y_Ciclo_de_Instruccion|1.2. Arquitectura del Computador y Ciclo de Instrucción]]** *(Diap. 11–22 | Clases 2, 3, 5, 15 y 22)*
   - Arquitectura de programa almacenado ($\text{CPU} = \text{UC} + \text{UD} + \text{Registros}$, Memoria, E/S y `BUS`).
   - **Von Neumann vs Harvard** y el **análisis estrella de examen (Clase 22)** sobre por qué el PIC16F877A puede solapar *Fetch* y *Execute* cada $4$ ciclos de reloj y Von Neumann no.
   - Memoria **RAM (volátil)** vs **ROM / Flash (no volátil)**, direccionamiento por **bytes** desde `0`, avance de `PC` ($+1$ vs $+4$ bytes) y lectura abreviada en **hexadecimal**.
   - **Indistinguibilidad entre instrucciones y datos** en Von Neumann.
   - **Ciclo de Instrucción (*Fetch – Decode – Execute*)** con los registros `PC`, `MAR` e `IR`.
3. **[[1.3_Programacion_y_Proceso_de_Traduccion|1.3. Programación de un Procesador y Proceso de Traducción]]** *(Diap. 23–31 | Clases 4, 5 y 14)*
   - Diferencia entre **algoritmo**, **programa** y **lenguaje de programación**.
   - Representación binaria de información heterogénea en memoria.
   - Niveles de lenguaje: **alto nivel** (C, Python), **ensamblador** (MIPS/MARS) y **lenguaje máquina**.
   - Los **2 escenarios reales** donde un ingeniero baja a ensamblador (ingeniería inversa de un dron y control en tiempo real del AVE a $350\text{ km/h}$).
   - Cadena completa de traducción: **Compilador $\rightarrow$ Ensamblador $\rightarrow$ Enlazador (*Linker*) $\rightarrow$ Cargador (*Loader*)**.
4. **[[1.4_Ejecucion_de_un_Programa_y_Casos_Practicos|1.4. Ejecución de un Programa y Casos Prácticos de Pizarra]]** *(Diap. 32–35 | Clases 3 y 4)*
   - Ejecución secuencial vs saltos incondicionales y condicionales (modificación del `PC`).
   - Riesgo de seguridad cuando `PC` apunta a datos (*segmentation fault* y *hackeos*) y diseño de seguridad de `HALT = 0000`.
   - **Caso práctico paso a paso en simulador Von Neumann (Clase 4):** programación en código máquina binario puro del **contador de $3$ a $0$**, análisis del bug real de frontera (`>= 0` vs `> 0`) y rediseño lógico correcto.

---

## Radar Rápido de Examen — Tema 1

> [!danger] Aviso / Trampa de Examen
> 1. **Regla de oro del profesor:** Toda pregunta exige **justificación razonada**. Responder "Sí" o "No" sin justificar puntúa **$0$** aunque aciertes (*Clases 7 y 22*).
> 2. **"La pregunta del millón" (cae casi seguro):** ¿Cómo distinguir una instrucción de un dato en la memoria de una máquina de Von Neumann? $\rightarrow$ Mirando solo la memoria es **imposible**; los distingue la **fase del ciclo de instrucción** (`Fetch` vía `PC` = instrucción; `Execute` vía operando = dato) (*Clases 3, 5, 8, 12 y 22*).
> 3. **Pregunta de nota alta / matrícula (Clase 22):** Identificar que el cronograma *Fetch* / *Execute* solapado del PIC16F877A corresponde a una **arquitectura Harvard** (y NO a Von Neumann) y justificar que Von Neumann no puede hacerlo porque colisionarían dos accesos simultáneos a la única memoria.
> 4. **Conteo desde cero:** En informática las posiciones de memoria y bits empiezan siempre en **$0$** ($256$ bytes van de $0$ a $255$; instrucciones MIPS de $32\text{ bits} = 4\text{ bytes}$ van en $0, 4, 8, 12\dots$) (*Clases 2 y 3*).

---

## Conexiones con el Grafo del Primer Parcial

- [[Representación de la Información]] (Tema 2): cómo se codifican en cadenas de `0` y `1` tanto los datos (ASCII/UTF-8, enteros, reales) como los campos de las instrucciones.
- [[Binario Natural]] y [[Complemento a 2]] (Tema 2): base de las direcciones de memoria, los valores en el acumulador y los desplazamientos de salto.
- [[Formato de Instrucción]] y [[Arquitectura MIPS]] (Tema 3): desglose interno de los $32\text{ bits}$ de cada instrucción máquina y modelo *Load-Store*.
- [[Entrada y Salida]], [[Colisión de Bus]] y [[Chip Select]] (Tema 4): conexión física de los periféricos al bus compartido de la máquina Von Neumann.
- [[Interrupciones]] y [[Timer0]] (Tema 4): cómo la CPU detiene el ciclo de instrucción justo al finalizar la instrucción actual para salvar el `PC` y por qué el `Timer0` del PIC avanza a $f_{\text{osc}}/4$.

---

## Checklist de Dominio del Tema 1

- [ ] Explicar con ejemplos por qué la dirección de tiro naval **no** es un computador y el telar de Jacquard **sí** lo es (pero de propósito específico).
- [ ] Dibujar de memoria el esquema $\text{CPU} = \text{UC} + \text{UD} + \text{Registros}$ con Memoria, E/S y todas las señales de `control`, `estado`, `datos`, `instrucciones` y `direcciones`.
- [ ] Justificar en 3 líneas cómo distingue la CPU una instrucción de un dato en Von Neumann y qué ocurre si `PC` salta a la zona de datos.
- [ ] Comparar **Von Neumann vs Harvard** y justificar el solapamiento *Fetch* / *Execute* del PIC16F877A.
- [ ] Explicar el papel de la **ROM** en el arranque (`PC = 0`) frente a la volatilidad de la **RAM**.
- [ ] Trazar las 3 fases (*Fetch*, *Decode*, *Execute*) con `PC`, `MAR` e `IR` y distinguir algoritmo, programa, compilador, ensamblador, *linker* y *loader*.
