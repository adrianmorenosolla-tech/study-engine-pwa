# Transcripción de Clase: Fundamentos de Computadores
**Tema: Representación de la Información y Conversiones Numéricas**

Como decía la semana pasada, en el tema 3 vimos la descripción de cómo están compuestos los campos de bits de las instrucciones en código máquina y la arquitectura interna del PIC. Como digo, lo vimos de manera suficiente para que podáis empezar a trabajar en las prácticas. Retomamos entonces el tema 2. 

Recordad que ya hemos visto todo lo que es representación de enteros, pero por completar un poquito vamos a hacer hoy este ejemplo que viene en el tema 2. Habíamos saltado la parte de enteros y fuimos al tema 3 para ver la representación de la información, modos de direccionamiento y la arquitectura interna del PIC, y ahora volvemos al tema 2 para completarlo.

Como digo, vamos a ver estos ejemplos porque, recordad y ya os lo he dicho, no vamos a hacer en el ordenador el examen de representación de la información, pero algún problema de este tipo de conversiones caerá en papel y lápiz en el primer examen parcial. Os lo explico para que veáis un poquito qué es lo que tenéis que hacer y, sobre todo, para que vayáis haciendo ejercicios del Campus Virtual. Tenéis herramientas de generación de problemas suficientes para que no necesitéis que yo os dé relaciones de problemas; vosotros mismos podéis ir trabajando sobre el generador del Campus Virtual tantos ejercicios como sea necesario para que podáis hacer este tipo de conversiones con destreza. Porque si pretendéis aprender a hacerlo en el momento del examen, os vais a estrellar de mala manera.

En la parte de números reales, ahora lo comentaré, no vamos a hacer en el examen problemas de transformación de reales a decimales (de IEEE 754 a decimal, o viceversa). Básicamente para eso necesitáis una calculadora y mucho tiempo. Lo de la calculadora a unas malas se podría resolver (aunque llevar calculadora es una tentación para esto y no me apetece), pero sobre todo es un problema de tiempo: hacer una conversión lleva un montón de tiempo y no tenemos tanto. Lo que sí es seguro es que caerá alguna pregunta en la parte de teoría acerca de cómo se representan los reales. Una vez que termine este bloque entraremos en la representación de reales y ya iré avisando qué cosas pueden caer como preguntas de exámenes y qué cosas no. 

Sin más dilación, esto es trivial.

## Conversión de Cadena Binaria a Representación Decimal

El ejercicio de cómo convertir de cadena binaria a representación es fácil en el momento en que recordáis cómo se interpreta el valor numérico posicional. Hay un método general de ir dividiendo iterativamente por dos, pero como vamos a dejar de trabajar con métodos complejos y siempre trabajaremos con números de 8 bits (1 byte), la forma mucho más rápida de hacer todo es recordar los pesos de los 8 bits. 

Los índices van del 0 al 7 y los pesos son: $1, 2, 4, 8, 16, 32, 64, 128$. Si calculáis el siguiente bit, que ya no es parte del byte (queda fuera), sería $256$. Os recuerdo esto porque el valor típico de todos los bits a 1 es $256 - 1 = 255$. Ya tenéis todo lo necesario para poder calcular los valores.

### Primera Tanda: Cadena `01111111`
Tenemos un cero y todo lo demás a uno. La forma difícil de hacerlo es sumar todos los pesos. La forma fácil es darse cuenta de que si a este número le sumáis 1, sale exactamente el número superior, cuyo peso conocemos (128). Así que $128 - 1 = 127$.
*   **Binario natural:** $127$.
*   **Signo y Magnitud:** El primer bit está a 0 (positivo). Ya sé que las tres representaciones principales (Signo-Magnitud, Binario natural y Complemento a 2) van a coincidir exactamente en su valor positivo, así que es $+127$.
*   **Complemento a 2:** $127$.
*   **Exceso a 128:** El valor representado en exceso a M es el valor original más M. Para deshacer el exceso, al valor original (127) hay que restarle 128. Esto daría $-1$.
*   **BCD (Decimal Codificado en Binario):** Los bits van de 4 en 4. Tenemos `0111` (7) y `1111` (15). El dígito decimal 15 no existe del 0 al 9. Por lo tanto, este patrón no es válido y no se puede representar en BCD.

### Segunda Tanda: Cadena `10000000`
*   **Binario natural:** Ya lo conocemos de sobra, es $128$.
*   **Signo y Magnitud:** El primer bit es un 1 (número negativo). El resto del valor absoluto es 0. Sería $-0$. Recordad que la representación en Signo-Magnitud tenía el problema de los dos ceros (positivo y negativo).
*   **Complemento a 2:** Es negativo porque el bit más significativo es 1. Hay que deshacer el complemento a 2. Restamos 1 y queda `01111111` (complemento a 1). Invirtiendo los bits obtenemos de nuevo `10000000`, que en valor positivo es 128. Como le ponemos el signo, es $-128$.
*   **Exceso a 128:** Fijaos que si tomamos el valor en binario natural (128) y le restamos el exceso (128), obtenemos $0$. No he tenido ni que pensar, basta con restar.
*   **BCD:** Tenemos cuatro dígitos y otros cuatro: `1000` es 8, y `0000` es 0. El valor es $80$. (En BCD no hay números negativos).

### Tercera Tanda: Cadena `00000001`
Es un número positivo. Las tres primeras representaciones coinciden de nuevo.
*   **Binario natural:** $1$.
*   **Signo y Magnitud:** $1$.
*   **Complemento a 2:** $1$.
*   **Exceso a 128:** Hacemos $1 - 128 = -127$.
*   **BCD:** Tenemos `0000` (0) y `0001` (1). Valor: $01$.

### Cuarta Tanda: Cadena `10000001`
De nuevo va a ser un número negativo por tener el bit más significativo a 1.
*   **Binario natural:** $128 + 1 = 129$.
*   **Signo y Magnitud:** Signo negativo (1), valor absoluto 1. Esto es $-1$.
*   **Complemento a 2:** Deshacemos el complemento. Invertimos todos los bits (`01111110`) y le sumamos 1, obteniendo `01111111`. Esto es 127. Aplicando el signo, es $-127$.
*   **Exceso a 128:** Hacemos $129 - 128 = 1$.
*   **BCD:** Tenemos `1000` (8) y `0001` (1). Al ser dígitos válidos, el valor es $81$.

Como veis, sabiendo los pesos de los ocho primeros bits, hacer todas las cuentas es sumamente rápido. 

## Conversión de Decimal a Representaciones Binarias

Vamos a ver ahora cómo hacer la conversión inversa, partiendo de un número decimal hacia las distintas representaciones.

### Valor Decimal: 25
*   **Binario natural:** Buscamos los pesos. El 32 se pasa, empezamos por el 16. $25 - 16 = 9$. El 8 cabe. $9 - 8 = 1$. Nos queda el peso 1. La cadena resultante es `00011001`.
*   **Signo y Magnitud:** Al ser positivo, es la misma cadena: `00011001`.
*   **Complemento a 2:** De nuevo, positivo, coincide: `00011001`.
*   **Exceso a 128:** Sumamos $25 + 128 = 153$. Representar 153 consiste simplemente en encender el bit del 128 sobre la cadena anterior: `10011001`. 
*   **BCD:** Codificamos el 2 (`0010`) y el 5 (`0101`). Resultado: `00100101`.

### Valor Decimal: -10
*   **Binario natural:** Directamente decimos que no. No puede representar números negativos.
*   **Signo y Magnitud:** Ponemos un 1 en el bit de signo, y representamos el 10 ($8 + 2$). Resultado: `10001010`.
*   **Complemento a 2:** Partimos del +10 (`00001010`). Invertimos todos los bits (`11110101`) y sumamos 1. Resultado: `11110110`.
*   **Exceso a 128:** Hacemos $-10 + 128 = 118$. Buscamos representar 118: $64 + 32 = 96$. $96 + 16 = 112$. Faltan 6 ($4 + 2$). La cadena es `01110110`.
*   **BCD:** No es representable, porque en BCD no existen los números negativos.

Con 8 o 10 bits (hasta 1024) los cálculos se llevan bastante bien a mano. Por encima de eso, usad la calculadora; pero, como ya he dicho, en el examen no me interesa evaluar si sabéis operar con 32 bits a mano gastando tiempo innecesario.

## Representación de Números Reales: Punto Fijo y Coma Flotante

Una vez visto esto, pasamos a los números con decimales. Hay dos tipos de representaciones: Punto Fijo y Coma Flotante.

### Punto Fijo
Como su nombre indica, fijamos la posición del punto decimal. Decidimos de antemano a piñón cuántos bits dedicamos a la parte entera y cuántos bits a la parte decimal o fraccionaria. Es como las calculadoras de contabilidad en las que configuras dos dígitos para los céntimos y no los puedes mover. 

En computación, si tenemos un total de $N$ bits, dedicamos un cierto número a la parte entera y el resto a la fraccionaria. No es un formato flexible, pero es útil si el rango de variación de los datos es constante. Las operaciones matemáticas en punto fijo internamente funcionan de forma muy similar a trabajar con números enteros escalados, lo que requiere muchísimo menos coste computacional para el microprocesador en comparación con la coma flotante.

La forma estandarizada de convertir la parte decimal o fraccionaria a binario es ir multiplicando reiteradamente por 2. Por ejemplo, queremos convertir el valor decimal $0.35$:
1. $0.35 \times 2 = 0.70$. Como la parte entera es 0, el primer dígito fraccionario es $0$.
2. Tomamos el resultado y multiplicamos: $0.70 \times 2 = 1.40$. Tomamos el 1 para el segundo dígito binario, y nos quedamos con el resto fraccionario ($0.40$).
3. $0.40 \times 2 = 0.80$. La parte entera es 0. Tercer dígito: $0$.

También podéis hacerlo usando los pesos fraccionarios: $2^{-1} (0.5)$, $2^{-2} (0.25)$, $2^{-3} (0.125)$, etc. Vas restando de tu valor los pesos que caben, exactamente igual que hacíamos en la parte entera pero con exponentes negativos.

### Advertencia Crítica de Programación con Números Reales

Aquí hay una moraleja muy importante desde el punto de vista de la programación. Es una advertencia que a menudo ni siquiera en libros de programación se detalla, y es súper común encontrarse con esto en el mundo real. 

Cuando trabajáis con números fraccionarios (reales) en lenguajes de programación como C o JavaScript, **es muy mala idea hacer una comprobación condicional estricta de igualdad**. Es decir, jamás hagáis algo como `if (A == B)` con variables de coma flotante. 

¿Por qué va a fallar esa comparación? 
Fijaos en lo que pasa al convertir decimales a binario (como el $0.35$ anterior): muchas fracciones decimales producen una cadena binaria periódica infinita. Dado que la representación en el ordenador (por ejemplo, el estándar IEEE 754) tiene un límite de bits en la mantisa (23 bits para precisión simple), llega un punto en el que el ordenador tiene que truncar o redondear ese número. Por tanto, el número almacenado rara vez va a cuadrar de forma exacta a nivel de bits con el número teórico que vosotros habéis tecleado, sino que se almacenará con un pequeñísimo error de precisión (ej. $0.34999999...$). Si se van encadenando operaciones aritméticas, ese error de redondeo se va arrastrando y aumentando.

Si hacéis una comparación estricta de igualdad `==`, el hardware verificará todos y cada uno de los bits. Como diferirán en los últimos decimales por los redondeos, la condición os devolverá falso aunque lógicamente los números debieran ser el mismo. Vuestro programa tendrá un comportamiento totalmente errático y os volveréis locos intentando depurarlo.

**La solución** es usar la noción de tolerancia o resolución (a veces llamada *epsilon*). Lo que debéis hacer siempre es comprobar si el valor absoluto de la resta entre los dos números es menor que el umbral de tolerancia aceptado por vuestro programa.
`if (abs(A - B) < 0.001)`
Con esta condición, si la diferencia cae dentro de ese pequeño margen de error, consideramos los números como lógicamente iguales, solucionando por completo el problema del arrastre de imprecisión en coma flotante.