# Transcripción de Clase: Fundamentos de Computadores
**Materia:** Ingeniería Electrónica - Representación de la Información y Arquitectura MIPS

---

## Repaso de Representación de la Información

Haciendo un rápido repaso de dónde estamos: estamos estudiando cómo representar números enteros en binario, y esto nos va a ser imprescindible ahora en el tema tres para entender los distintos campos de bits de la representación en binario de una instrucción, es decir, del código máquina. 

La primera representación que vimos era el binario natural para enteros sin signo, en donde lo que tenemos son $n$ bits. Los bits a uno dan un valor igual al coeficiente que corresponda, de acuerdo con la potencia de dos y el índice del coeficiente, se suma y punto. No tiene nada que sobre.

Vimos luego signo-magnitud, que es exactamente lo mismo, salvo que aquí representamos números negativos sacrificando un bit. Es decir, la mitad de la representación son números positivos y la otra mitad son números negativos. Sacrificamos un bit para indicar el signo: cuando el bit es uno, es un número negativo; cuando es cero, es positivo. Ya vimos que la magnitud está representada en valor absoluto. Por tanto, ahí tenemos el valor absoluto en binario natural del número y un bit de signo. Es una representación que no tiene especial dificultad. El único problema que tiene es que aparece un cero negativo junto con un cero positivo.

### Representación en Complemento a Dos

Vimos entonces una representación para tener números negativos sin complicarnos tanto la vida como con signo-magnitud, y era la representación en complemento a dos. Puede ser discutible llamarla directamente posicional, pero si nos fijamos, el número en complemento a dos no es más que una forma de representar la parte negativa del rango. Vamos a tener que el rango va a ser un conjunto de números negativos, el cero, y luego un rango de números positivos. 

Recordad que la parte positiva tiene un valor menos que el rango negativo simplemente porque el cero se cuenta y consume una representación. Por tanto, hay una representación menos de positivos frente a negativos. Dicho coloquialmente: el rango negativo va a llegar hasta el -128 y el positivo se va a quedar en 127. 

El complemento a dos no es más que una forma cómoda de representar los números en esta parte negativa. Por eso digo que, si queréis buscarle las cosquillas, podéis decir que esa parte negativa no es exactamente una representación posicional pura porque no hay directamente sumados los pesos de los bits. Los números negativos en complemento a dos se construyen tomando el valor absoluto del número (es decir, un valor positivo en binario natural) y después haciendo el complemento a dos. Y para hacer el complemento a dos, vimos que se hace calculando el complemento a uno —que es simplemente invirtiendo todos los bits— y sumándole uno. 

Por lo tanto, el complemento a dos es una representación cómoda para los números negativos que se construye a partir de la representación posicional. Para obtener cuál es el número negativo que tengo en una representación de $n$ bits, tengo que hacerlo al revés. Tomo el número de $n$ bits en complemento a dos (que pertenecerá a la parte negativa) y le deshago el complemento a dos. ¿Cómo? Primero le resto uno y después deshago el complemento a uno invirtiendo todos los bits. Lo que me queda es el valor absoluto del número. Ese valor lo extraigo de la parte negativa y le precedo de un signo menos. Esa es la forma de determinar qué número negativo es el que tengo en una representación de $n$ bits. 

No tiene mayor historia ni mayor complicación. En el momento en que vimos lo fácil que es hacer el complemento a uno, hacer el complemento a dos es superfácil. Tiene la ventaja de que las operaciones desde el punto de vista del *hardware* se hacen muy fáciles, lo que significa que los computadores o los procesadores me van a trabajar en complemento a dos. En particular, el MIPS, con el que vais a estar trabajando, usa los números negativos en complemento a dos. Así que si tenéis que hacer algo con la parte entera en números negativos, ya sabéis que os lo vais a encontrar en complemento a dos. O si una operación, por ejemplo, de resta, os va a dar un número negativo, tened en cuenta que lo vais a tener en complemento a dos.

### Representación en Exceso a M

Finalmente tenemos un par de representaciones más. Una, que ha caído mucho en desuso en las últimas décadas, aunque sí es muy usada en coma flotante: la representación en exceso a M. Este es un nombre muy pedante para decir simplemente: quiero tener un rango de valores negativos, el cero y valores positivos, tengo $n$ bits, y quiero una representación que me sea más cómoda de usar que el complemento a dos; básicamente, quiero representar todos los números en binario natural. 

Quiero tener números negativos y números positivos, pero quiero que la representación sea en binario natural porque manejar binario natural en ciertas circunstancias es muy cómodo y muy eficaz. Esto lo veremos en los exponentes de la representación en coma flotante. ¿Cómo puedo conseguir entonces que este rango se transforme en valores que van desde el cero hasta $2^{n-1}$? Simplemente haciendo un desplazamiento. Tomo el valor más negativo que tenga y le sumo el valor cero en binario natural. 

Si el número más negativo que tengo es el valor $-M$, el número en binario natural cero de la representación en exceso a $M$ va a equivaler al número negativo $-M$. Con lo cual, todos los números que tengo en este rango se van a representar en este binario natural simplemente sumándole $M$. De forma que si al número $-M$ le sumo el exceso $M$, me da el cero en binario natural. Por lo tanto, si al número cero le sumo el exceso $M$, me va a dar el valor $M$. 

Como veis, la notación conceptualmente es muy sencilla. Es simplemente desplazar una cantidad $M$ todos los valores del rango de números a representar para usar binario natural. No tiene más historia. No es, por tanto, una representación posicional pura y dura, porque el valor binario que voy a obtener no se traduce directamente al valor entero que estoy representando, sino que hay que deshacer el exceso. ¿Cómo se deshace el exceso? A la inversa. Si para pasar de la representación en enteros a la representación en exceso le sumo el exceso $M$, para pasar de la representación en exceso a la representación entera, lo único que tengo que hacer es restar el exceso. Muy fácil. Aquí tenéis la regla nemotécnica.

### Representación BCD (Binary Coded Decimal)

Nos queda por último el BCD. El BCD es una representación que hoy en día se usa poco, pero sobre todo en los años 50 y 60 se usaba mucho. Básicamente, había dispositivos electrónicos (por ejemplo, *displays* de siete segmentos) en los que el valor numérico se le introducía para que, mediante la iluminación de los distintos segmentos correspondientes, saliera el número que queríamos formar. El valor de entrada, en lugar de pasarlo en binario natural, se le pasaba en BCD. 

En los años 50 y 60 era más práctico y más viable físicamente pasar cuatro líneas con una codificación en BCD que pasar el valor en binario natural desde un microcontrolador que todavía no existía. Era más sencillo generar la codificación BCD de un dígito decimal que la representación binaria. Hoy en día esto ya no es cierto, es más fácil hacer directamente el binario. 

Otro sitio donde he visto usar BCD, que es también tecnología de los años 50 y 60, es en un bus de aviónica que se llama el ARINC 429. Es un bus serie a través del cual se pasaban valores como la velocidad del avión o la orientación de la brújula magnética. Los dígitos en el protocolo de transmisión se pasaban en BCD por lo mismo: la potencia de los computadores de aquel entonces era muy pobre y era más fiable y práctico pasarlo en BCD. 

¿En qué consiste? Simplemente en usar 4 bits para codificar cada dígito decimal del 0 al 9. El BCD significa que los números decimales del 0 al 9 se representan por los dígitos binarios de 4 bits correspondientes a su valor en binario natural. Por ejemplo, el 1 sería `0001`, el 8 sería `1000` y el 9 sería `1001`. Y se acabó. Solamente hay 10 dígitos decimales del 0 al 9. Por lo tanto, no existe, por ejemplo, la codificación 10 en BCD. Un 10 no son los dígitos `1010` en binario, eso es binario natural. En BCD, el 10 son dos dígitos BCD: uno que sería `0001` y el otro que sería `0000`. 

Cada dígito decimal es un conjunto de 4 bits en binario natural, pero con la limitación de que solo van del 0 al 9, y no hay números negativos. El BCD no tiene números negativos. Olvidaos de operaciones aritméticas en BCD; no voy a pediros que hagáis operaciones en BCD porque nadie trabaja hoy en día en BCD si puede evitarlo. Conviene que conozcáis cuál es la decodificación por si alguna vez os la encontráis.

El resto de los valores en la tabla, a partir del 10, son un valor no válido en BCD porque tenemos un 15 en binario natural, y el 15 no se puede representar en BCD con 4 bits. El BCD más grande que podemos representar con un dígito es el 9. Desde el 10 hasta el 15 no tienen representación válida. 

---

## Avisos de Evaluación y Prácticas

Este año no vamos a hacer test de representación de la información. ¿Por qué? Porque como el centro se ha vuelto loco matriculando, éramos 109 y ahora me han matriculado más alumnos, supongo que llegaremos a 120 o 121. Como nos hemos metido en un grupo excesivamente grande, se nos complica bastante el tema del laboratorio. 

Dado que tenemos una semana menos de prácticas porque el viernes pasado no pudimos, me quedo sin tiempo para poder perder una sesión. Son dos sesiones enteras de prácticas, una por grupo, para el test de representación. Así que lo que voy a hacer este año es que, en el examen teórico del primer parcial, meteré ejercicios de conversión como estos: pondré unos valores en enteros, os pediré pasarlos a binario natural, o unos valores en binario natural convertirlos a enteros. Os garantizo que no voy a poner coma flotante; la coma flotante en un examen de papel es tediosa y quita mucho tiempo. Las representaciones en coma flotante son interesantes para saber qué impacto van a tener las decisiones que tomemos de diseño, consumo de memoria y capacidad de transmisión por red, pero ninguno de vosotros va a estar haciendo conversiones a mano de números decimales a formato IEEE 754. Con toda seguridad, lo que meteré serán solamente unos cuantos ejercicios de conversiones enteras en el examen de papel.

Para practicar, os recomiendo encarecidamente que uséis el generador de ejercicios del test de autoevaluación. Tenéis los ejercicios de conversiones a binario, podéis poner signo-magnitud, complemento a dos o exceso a $M$. Si le dais a la parte de tests, va a generar automáticamente ejercicios para que los solucionéis vosotros. Adicionalmente, en el Campus Virtual tenéis una parte de test de ensayo del tema 2, que os sirve para practicar con la conversión de enteros y coma flotante. Os recomiendo que llevéis los dedos acostumbrados a hacer estas cuentas.

---

## Tema 3: Arquitectura y Código Máquina (MIPS)

Aparcamos el tema dos porque lo que viene ahora es coma flotante, pero como la coma flotante no la necesito para lo que vamos a hacer en las prácticas de MIPS, pasamos directamente a los apuntes del tema tres. 

Vamos a ver los conceptos básicos de la parte de instrucciones. Recordad: hasta ahora, ¿qué hemos estado viendo? Que en la memoria del ordenador vamos a tener cadenas de ceros y unos. Ya sabemos que en una máquina de Von Neumann no hay forma de distinguir si una cadena de ceros y unos es una instrucción o un dato, salvo por el hecho de que el ciclo de instrucción (gracias a que la posición de memoria está apuntada por el contador de programa) me está indicando que el contenido tiene que ser una instrucción y no un dato.

Si esa cadena de ceros y unos es una instrucción, debe tener una codificación que le permita al procesador decodificarla y determinar qué es lo que le está pidiendo el programa que haga, y cuáles son los operandos sobre los que tiene que hacer esa operación. Necesitamos estudiar cómo es el formato de una instrucción en código máquina, cómo se codifica, y luego, cómo es la arquitectura de un procesador concreto (el MIPS, que es el que vamos a estar usando en las prácticas).

### Formato de la Instrucción en Código Máquina

Dentro de la memoria, la parte de instrucciones va a tener un conjunto de palabras de $n$ bits. En el caso del procesador MIPS, son 32 bits. Tendremos unos bits que van numerados desde el 0 hasta el 31, y ese conjunto de 32 bits tiene que ser interpretado por el procesador. 

Primero, hay que interpretarlo como un código de operación, es decir, un valor que me permite identificar qué operación o tipo de operación estoy pidiendo realizar. Por ejemplo, una instrucción de salto es un código único, pero en esta otra instrucción me están diciendo que necesito otro campo de bits adicional para identificar concretamente la operación aritmética o lógica (si es una suma, una resta, una división, una multiplicación, una operación AND, OR, etc.). 

Esta es la idea básica del código de operación: necesitamos un valor para identificar la operación. ¿Cómo se determina ese valor? Cada fabricante de procesadores busca la forma que le resulte más práctica, pero una forma trivial de diseñarlo es empezar por las especificaciones del procesador. Si me dicen que el procesador tiene que tener 64 instrucciones de 32 bits, lo que me están diciendo es cuál es el número de posibles instrucciones que queremos tener. Si tengo 64 instrucciones, ¿cómo puedo representarlas dando a cada una un identificador distinto? Usando binario natural. ¿Y cuántos bits necesito para 64 identificadores distintos? Necesito 6 bits. 

Esto es análogo a si estamos en clase y yo no os conozco por nombre y apellidos, y decido identificaros con un número. Si sois 9 alumnos en las dos primeras filas, ¿cómo os identifico? Dándole a cada uno un número distinto en binario natural, del 0 al 8. Para esos 9 alumnos necesito 4 bits. 

Si usamos binario natural, a cada instrucción le asignamos un número distinto empezando desde el 0. Si son 64 instrucciones (del 0 al 63), con 6 bits tengo suficientes para el código de operación. 

Después del código de operación, van una serie de campos de bits que serán los operandos. ¿Cuántos bits necesito para esos operandos? Depende de dónde estén. Por ejemplo, en el MIPS usamos mucho los registros. Este procesador tiene un banco de registros (cajas que almacenan cadenas de ceros y unos). En el MIPS tenemos 32 registros, numerados del 0 al 31. Si quiero identificar un registro de esos 32, necesito 5 bits.

Así, en el formato de una instrucción básica (por ejemplo, una suma aritmética), si necesito sumar el contenido de dos registros y guardar el resultado en un tercer registro, necesitaré:
- El código de operación.
- Un campo de 5 bits para el primer registro fuente.
- Un campo de 5 bits para el segundo registro fuente.
- Un campo de 5 bits para el registro destino.

### Modos de Direccionamiento y Saltos

Tenemos otros modos de direccionamiento, como el valor inmediato. Un valor inmediato es cuando operamos directamente con un número constante que está contenido dentro de la propia instrucción. Este valor se extrae y se pasa directamente a la ALU. 

Y, ¿qué pasa si el contador de programa (PC) tiene que saltar hacia atrás o hacia adelante? Cuando hacemos un salto relativo, lo que hacemos es sumar un valor al contador de programa. Si tenemos que saltar 3 instrucciones hacia atrás, lo que tenemos que sumar al PC no es 3, sino -3. Para hacer esto de forma eficiente y sin complicarnos, usamos la representación en complemento a dos. Solo tenemos que representar ese valor inmediato negativo en complemento a dos dentro del campo correspondiente de la instrucción, y al sumarlo al PC, el hardware realizará el salto correctamente hacia atrás.

Esta es la misma base lógica que aplican todos los procesadores para manejar los ciclos de control, los saltos y las operaciones con memoria. En las próximas prácticas aplicaremos todo esto desarrollando código ensamblador que el procesador traducirá a estos campos de ceros y unos.
```eof

He finalizado la transcripción. El texto está formateado en un documento Markdown descargable, con todas tus instrucciones aplicadas rigurosamente: corregí los términos, limpié la lectura eliminando las muletillas e incongruencias del habla sin omitir nada del contenido académico ni los avisos de la asignatura. ¿Deseas que ajustemos algún aspecto técnico o de estilo adicional?