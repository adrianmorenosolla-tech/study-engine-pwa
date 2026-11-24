# Transcripción de clase: Fundamentos de Computadores
**Tema:** Controlador de interrupciones, repaso de ciclo de instrucción y temporización (PIC 16F877A), y avisos del primer parcial.

---

## 1. Avisos sobre el primer parcial y temario

Lo que vamos a ver hoy es control de interrupciones. Me parece que quedaría empezar el tema de DMA, pero el DMA no entra para este parcial; eso entrará para el segundo parcial. Así que exclusivamente la parte de interrupciones es con la que termina el Tema 4. Con eso ya tenéis un ratito para prepararlo.

*(Pregunta de un alumno: ¿Y preguntas de la distancia de Hamming y todo eso van a caer?)*

Tema 1, Tema 2, Tema 3 y Tema 4 menos DMA. Y en el Tema 2, lo único que explícitamente pone que no voy a preguntar son conversiones de coma flotante: ni de decimal a coma flotante, ni de coma flotante a decimal. Eso en papel se lleva demasiado tiempo, hay que usar una calculadora y ya lo he dicho varias veces que no lo iba a preguntar. Pero cualquier otra cosa del Tema 2, sí. Por lo tanto, pueden caer preguntas de conversiones de todos los colores y sabores, y pregunta de código Hamming SEC.

No voy a preguntar Hamming SECDED, porque realmente lo único que hace es añadir un bit adicional de paridad al Hamming y no me dice nada. Lo que puede ser interesante es una pregunta de Hamming SEC. Lo que significa que, con los ejercicios que tenéis y los ensayos que tenéis ahí, ya tenéis para haber practicado un buen rato, porque hay también ejercicios explícitos de Hamming SEC y luego, además, tenéis los generadores de ejercicios. O sea que no hay sorpresas.

¿Más preguntas?

---

## 2. Repaso de la temporización del PIC 16F877A: Reloj, Timer 0, Arquitectura Harvard y Pipeline

La otra cosita que quería comentar antes de meternos en la parte de controlador de interrupciones es que ayer se me quedó —como esto lo veo solo una vez al año y siempre hay que intentar cuadrar los contenidos— un poquito en el aire el tema de la relación entre la frecuencia de reloj, el porqué la frecuencia de reloj la divide por 4, y el hecho de que el Timer 0 hace un incremento en el contador (de 0 a 255 que hay aquí) por instrucción ejecutada.

Es decir, cada 4 ciclos de reloj incrementa en uno el valor del contador. Eso coincide con la ejecución de una instrucción. Es decir, cada incremento del Timer 0 implica que se ha completado la ejecución de una instrucción. Hay algunas instrucciones que tardan más de 4 ciclos, y además son las que os he dicho explícitamente que no me uséis, que son las multiplicaciones y las divisiones, porque son horriblemente lentas. Pero la inmensa mayoría del resto de las instrucciones se ejecutan cada 4 ciclos de reloj, y un incremento del Timer 0 involucra que se ha completado una instrucción.

Lo que se me quedó, como digo, un poquito en el aire sin justificar es por qué ese 4: por qué dividir por 4, por qué 4 ciclos de reloj. Y me he dado cuenta de que esto nos sirve bien para repasar el Tema 1.

Esta es la especificación que me da el fabricante, Microchip, de este microcontrolador, de la familia. De hecho, esto no es solo del PIC 16F877A; esto es del manual de la familia completa de lo que llaman la gama *mid-range*, es decir, gama media, que son PICs que van desde el 12 hasta el 16 y que tienen características de micro no barato-barato del todo, pero sí lo suficientemente potente para hacer cosas bastante serias.

Y toda esta familia utiliza una misma arquitectura interna en el procesador, en la unidad de control, y esa arquitectura de la unidad de control se caracteriza por una temporización como la que estamos viendo aquí. Y aquí es donde vamos a conectar con el Tema 1.

Me está diciendo aquí... a ver. Antes de deciros qué es lo que me está diciendo aquí, os voy a preguntar qué es lo que veis aquí. ¿Qué me está describiendo aquí? En particular con estas dos cajas que estoy marcando, y que corresponden a la misma instrucción. Las estoy marcando porque el hecho de que estén así entre corchetes hace que parezca que pertenecen a instrucciones distintas, aunque son consecutivas. Esa caja de la izquierda y la caja de la derecha pertenecen a la misma instrucción. ¿Qué es lo que me está describiendo con esas dos cajas? Tema 1.

*(Un alumno intenta responder sobre la búsqueda o carga).*

Tiene un nombre propio que ha sido el *leitmotiv* del Tema 1 y con el que prácticamente empezamos la asignatura.

*(Un alumno pregunta si es la carga, "load").*

No. Por eso digo, me he dado cuenta de que esto viene muy bien para que os asustéis un poquito acerca del repaso para el martes del Tema 1, y lo fácil que es que os pilléis los dedos con una pregunta.

¿Qué demonios es lo que estamos viendo ahí? Y tenéis una pista, porque me está poniendo *Fetch* y *Execute*. Eso ya me está dando la pista de qué es lo que estoy viendo ahí.

*(Una alumna responde en voz baja: el ciclo de instrucción).*

¡Bingo! El **ciclo de instrucción** (o ciclo de ejecución).

Pregunta, que esta podría ser de examen perfectamente, porque es de idea feliz... No, esta no sería de examen porque no es de idea feliz, es de las cosas que están claras, claras, claras, incluyendo todo lo que habéis hecho en prácticas y todo lo que ya habéis visto de documentación del PIC 16F877A, que lo tenéis clarísimo. Pero es que... no me gusta decir "preguntas trampa" porque luego me vais buscando trampas por todos lados. No me busquéis trampas en todas las preguntas. Hay alguna pregunta que tiene su miga porque la nota hay que sudarla, pero hay muchas preguntas que, cuando parece que es obvia la respuesta, es que es obvia la respuesta. Si es demasiado fácil, dadle dos vueltas no vaya a haber ahí justamente una trampa, pero en muchos casos, si la pregunta es sencilla, es que de verdad es sencilla; no hay nada oculto por detrás.

Pregunta: ¿Eso es el ciclo de instrucción de una máquina de Von Neumann?

*(Un alumno responde que sí).*

¿Seguro? Vamos a ver si puedo enfocar la siguiente cuestión... Es que ese no es el problema. En eso tenéis razón: es el ciclo de instrucción. No es ahí donde está el problema. El problema es lo que viene después: **máquina de Von Neumann**. ¿Por qué digo que es ahí donde está el problema? Y esta es la arquitectura del PIC 16F877A.

¿Veis los detallitos? Es decir, un concepto muy sencillo siempre puede —o va a tener— detallitos, que son los que hacen la vida divertida.

¿Es una máquina de Von Neumann? Sí o no; la abstención no me vale, eso no puntúa. Y si me decís sí o no, ya sabéis la coletilla que va a venir en el examen: **justifica la respuesta**. Nada de lotería de "sí/no y si tengo suerte me han tocado los puntos". Me podéis decir la respuesta correcta, que si no justificáis la respuesta, no vale un pimiento la pregunta.

*(Un alumno dice: sí, porque tiene memoria...)*

¿Qué es lo que caracteriza a una máquina de Von Neumann? Ya has dicho la palabra: **memoria**. ¿Qué es lo que caracteriza a una máquina de Von Neumann con respecto a la memoria?

*(Un alumno responde: que guarda los datos en memoria).*

¿Solo los datos?

*(Un alumno añade: las instrucciones también).*

¡Ahí vamos! Esa es la clave de la máquina de Von Neumann: tiene una **única memoria** en donde guarda **instrucciones y datos**. Por eso la pregunta —fijaos que es darle la vuelta a otra pregunta que os he hecho un montón de veces— de: "¿Puedo distinguir si el contenido que estoy viendo en una posición de la memoria de una máquina de Von Neumann es una instrucción o un dato?". No. Si no tengo información de dónde están guardadas las instrucciones, lo que veo son cadenas de ceros y unos y no puedo distinguirlas porque en la misma memoria están instrucciones y datos.

¿Es esto una máquina de Von Neumann? No. ¿Por qué? Porque están separadas: tenemos memoria de instrucciones y memoria de datos. Y, por lo tanto, esto es lo que se llama una **arquitectura Harvard**.

No me enfado mucho porque no hayáis caído ahora mismo en el nombre de arquitectura Harvard, pero id acordándoos de que es una arquitectura Harvard. Porque una arquitectura Harvard no es una Von Neumann, ya que tiene separadas las dos memorias.

Y teniendo en cuenta que es una arquitectura Harvard y que tiene separadas la memoria de programa y la memoria de datos, ahora podéis decirme, aquí en este diagrama, por qué esto no lo podría hacer en una máquina de Von Neumann y sí lo puede hacer con la máquina Harvard. Esto que tengo aquí.

Ya hemos respondido a la primera pregunta: ¿qué es lo que estoy viendo ahí? El ciclo de instrucción de mi máquina, en este caso una máquina Harvard.

En segundo lugar: ¿por qué esto que estoy viendo aquí que está haciendo no lo podría hacer en una máquina de Von Neumann?

Fijaos que aquí lo que me está diciendo es que en cada una de las operaciones que hace durante esa caja, cuando pone *Fetch*, en realidad... ¿os acordáis del ciclo de instrucción que teníamos? Adquisición de memoria de la instrucción, decodificación y ejecución. Ahí, la caja *Fetch* en realidad está metiendo en la misma caja dos acciones: el *fetch* en sí (el traerse de memoria al procesador, a la unidad de control, la instrucción) y la decodificación. Y lo que es la ejecución en sí es la siguiente caja (*Execute*), que sería la tercera fase de nuestro ciclo de instrucción.

Y ahí lo que me está diciendo, si os fijáis, es que aquí en este ciclo —o mejor dicho, dado que nos hemos centrado en esta instrucción— aquí ha hecho la adquisición y decodificación de la instrucción, y aquí está ejecutando la instrucción de la posición a la que apuntaba el contador de programa.

Y aquí me está diciendo dos cosas: por un lado, que en adquirir la instrucción o en ejecutarla, en ambos casos tarda exactamente lo mismo: 4 ciclos de reloj de la señal de reloj que está entrando al microcontrolador. Es decir, si le habéis puesto el reloj a $4\text{ MHz}$, este es el reloj que va a $4\text{ MHz}$ y, por lo tanto, 4 ciclos de reloj de $4\text{ MHz}$ son necesarios para cada una de las dos fases: 4 ciclos para la adquisición de la instrucción y decodificación, y 4 ciclos para la ejecución.

Segunda cuestión: aquí me está diciendo que durante estos 4 ciclos de reloj está haciendo la adquisición de la instrucción inmediatamente siguiente y está ejecutando la instrucción que acabamos de adquirir y decodificar.

Pregunta: ¿Por qué eso no lo podría hacer en una máquina de Von Neumann y sí lo puede hacer en una máquina Harvard?

Esta es difícil. Esta, cuidado, no es de idea feliz; esta sabéis perfectamente responderla, pero sería la típica pregunta de "la nota hay que sudarla". La diferencia entre sacar un aprobado o un notable, o sacar el sobresaliente o la matrícula. Si estáis viniendo todos los días y aguantándome todos los días, es porque aspiráis a sacar buena nota. Así que este es el tipo de preguntas que hay que responder para sacar buena nota. ¿Por qué eso no lo puede hacer la máquina de Von Neumann y sí lo puede hacer la máquina Harvard? Y está directamente relacionado con el Tema 1 y todo lo que ya conocéis del Tema 1.

*(Un alumno apunta a que está haciendo dos instrucciones al mismo tiempo).*

De hecho, lo que me está diciendo es que está haciendo el ciclo de instrucción de dos instrucciones al mismo tiempo, en fases distintas: para una de las instrucciones está haciendo la adquisición y decodificación, y para la otra instrucción está haciendo la ejecución.

Pero la ejecución, ¿qué puede involucrar? Si, por ejemplo, estáis ejecutando una instrucción que hace una operación aritmética y escribe el resultado de la operación aritmética en un registro de memoria —recordad que el PIC no usa un banco de registros separado, sino que usa las mismas posiciones de memoria como si fueran registros—, por lo tanto, la ejecución puede involucrar escritura a memoria o lectura de memoria. ¿Por qué eso no se podría hacer en la máquina de Von Neumann y sí se puede hacer con la arquitectura Harvard?

Estoy mostrando cómo incluso conceptos sencillos como los del Tema 1, en el momento en que bajas al mundo real y le das un poquito de vuelta, siguen siendo sencillos, pero tienes que haberlos entendido bien para poder interpretar esta documentación de la vida real.

*(Un alumno responde: porque estaría leyendo y escribiendo a la vez en la misma memoria, y eso no se puede hacer).*

¡Correcto! Esa es la respuesta correcta. Vas camino de la matrícula.

Justamente la razón por la que no se puede hacer en una máquina de Von Neumann es porque tiene una sola memoria donde residen instrucciones y datos. En la fase de adquisición, ¿qué estoy haciendo? Leyendo de memoria, de una posición de memoria que está apuntada por el contador de programa, el código de operación —la instrucción en sí—. Y si estoy ejecutando, y suponemos que esté leyendo o escribiendo en memoria, ¿qué estaría haciendo? Accediendo a otra posición de memoria en la parte de datos, que va a estar apuntada por otro registro que ya vimos que se encarga de poner la dirección a la que hay que acceder en la parte de datos. Pero como es una sola memoria, tendría que estar haciendo dos accesos simultáneos a memoria, cosa que no puede hacer la máquina de Von Neumann, al menos en este tipo de memorias sencillas. No puede estar leyendo la instrucción y al mismo tiempo leyendo o escribiendo datos de memoria.

¿Qué pasa con la arquitectura Harvard? Tenemos dos memorias separadas. Así que no hay ningún problema para que de una memoria esté haciendo la adquisición de la instrucción mientras con la otra memoria está haciendo las operaciones que involucren datos. Por eso es por lo que puede perfectamente superponer la ejecución de una instrucción con la adquisición de la inmediatamente siguiente, porque no colisionan en el uso de la misma memoria.

Y lo que me está diciendo la documentación es que gracias a este efecto —esto, por cierto, es lo que se llama en términos técnicos un **pipeline** de ejecución; en términos todavía más técnicos lo tenéis en el Tema 3 en los apuntes, aunque no vemos eso en la asignatura porque se nos sale de madre, pero os lo he dejado en los apuntes por si alguien tiene curiosidad en algún momento: hablan de **segmentación**—. Esto es esencialmente segmentación: tienes dos instrucciones que se están ejecutando al mismo tiempo, en paralelo, decaladas, con un decalaje en la fase de ejecución.

Lo que me está diciendo esto es que, gracias a este *pipeline* de adquisición y ejecución de instrucciones, puedo en los mismos 4 ciclos de reloj estar adquiriendo una instrucción y ejecutando la anterior. Lo que en la práctica se traduce en que **cada 4 ciclos de reloj completo la ejecución de una instrucción**.

En realidad, no es que la instrucción tarde 4 ciclos; la instrucción tarda 8 en total, porque hay que contar los 4 de la adquisición. Pero como se superpone con la ejecución de la anterior, cada instrucción esencialmente termina su ejecución en 4 ciclos, o dicho de otra forma: cada 4 ciclos de reloj se ejecuta una instrucción.

Por eso es por lo que —y ahora volvemos aquí al temporizador, a mi Timer 0— recordad que os he dicho que el reloj con que avanza el Timer 0 va a ser la frecuencia de la señal de reloj que esté siendo usada por el microcontrolador dividida por 4 ($f/4$). Ahora ya entendemos por qué es dividido por 4: porque básicamente la ejecución de cada instrucción requiere 4 ciclos de reloj y, por eso además, decimos que el Timer 0 se incrementa en 1 por cada instrucción que se ejecuta. Porque como es la frecuencia de reloj dividida por 4, 4 ciclos de reloj incrementan en uno el Timer 0 y, por tanto, coincide con la frontera de ejecución de cada instrucción. Como digo, salvo algunas instrucciones que son muy lentas y tardan más de 4 ciclos de reloj, pero son poco habituales.

---

## 3. Ejemplo práctico de diseño: Medición de tiempo de ejecución con Timer 0 e interrupciones

Y ahora una pregunta que está más relacionada con prácticas, pero puede servir también.

¿Os acordáis que hemos dicho más de una vez lo de control en tiempo real? Y hemos hablado de que puede ser que tengas que programar en código máquina (ensamblador) directamente porque vas tan justo de tiempo en la respuesta del sistema —es decir, los eventos se producen tan rápidamente— que el código generado por un compilador normal es demasiado lento, porque hace un montón de cosas que son buenas, pero que tardan demasiadas instrucciones, y tú tienes un segmento crítico en donde tienes que reducir al mínimo posible el tiempo de ejecución. Esa es una posibilidad.

La otra posibilidad que tiene relación con lo mismo: ¿os acordáis de la consulta periódica? Decíamos: "¿Puedes saber el tiempo que tarda cada consulta?". Sí, si sabes exactamente cuántas instrucciones pasan entre consulta y consulta.

¿De qué forma se puede saber cuántas instrucciones —teniendo en cuenta que se incrementa el Timer 0 en 1 por cada instrucción— tarda un segmento de código $S$ en donde tenemos un cierto número $N$ de instrucciones? ¿Cómo podemos saber cuántos milisegundos o microsegundos tarda en ejecutarse este segmento de código $S$ que tiene $N$ instrucciones? Quiero saber con precisión cuánto tarda en ejecutarse ese segmento de código.

¿Cómo puedo usar Timer 0 —sin habilitar interrupciones, solamente dejando correr Timer 0, que por defecto ya lo he dicho, siempre está activo— para saber exactamente cuánto tiempo, en microsegundos o en milisegundos, ha pasado desde el momento en que ejecuté la primera instrucción aquí hasta el momento en que ejecuté la última instrucción aquí?

*(Un alumno menciona que cada incremento del Timer 0 es una instrucción).*

Cada incremento del Timer 0 efectivamente es una instrucción. Suponte que no sabes exactamente cuántas instrucciones son porque no te has parado a contarlas, porque es un segmento largo, pero lo que sí sabes es cuál es el punto de entrada y cuál es el punto de salida, es decir, cuál es la primera instrucción y cuál es la última instrucción. ¿Cómo se os ocurriría usar el Timer 0 para poder determinar cuántas instrucciones tiene el segmento y cuánto tiempo tarda?

*(Un alumno responde: poner el Timer 0 a 0 al principio).*

Si aquí, en el punto de entrada, cargo `Timer 0 = 0`, y luego aquí en el punto de salida, inmediatamente después, lo que hago es que tomo una variable a la que llamo, por ejemplo, `long` (o `cuenta`), y la cargo con el valor de Timer 0, ¿qué voy a tener en esa variable? El número de incrementos que ha habido en el Timer 0 y, por lo tanto, el número de instrucciones que ha habido.

¿Y cómo puedo traducir eso a tiempo? Porque fijaos que el Timer 0 lo que me va a dar es el número de instrucciones. ¿Cómo lo puedo traducir a tiempo?

Esto, por cierto, es algo que se usa mucho cuando estás desarrollando hardware para determinar temporizaciones críticas. De hecho, un procesador como estos —un Core i5— lleva un contador interno de muy alta resolución y de muy alta precisión que te permite medidas de nanosegundos, y te permite medir tiempos como estos insertando desde código un sello de tiempo al inicio, o más bien leyendo el contador al inicio del intervalo, leyendo el valor al final y restando; con eso ya sabes cuántos *ticks* de temporizador ha habido en ese intervalo.

Como digo, esto me da el número de instrucciones. ¿Cómo lo traduzco a tiempo?

*(Un alumno comenta sobre dividir la frecuencia entre 4).*

Vamos a suponer que la frecuencia del reloj la llamamos $f$. Esta es la frecuencia a la que estoy haciendo correr el procesador. $f / 4$ es la frecuencia a la que se está incrementando el Timer 0. Si a esto le doy la vuelta, esto me da el periodo $T = \frac{4}{f}$ (el inverso de la frecuencia es el periodo). El periodo es el tiempo que hay entre un *tick* (entre un incremento) y el incremento inmediatamente siguiente; es decir, el tiempo que tarda en completarse la ejecución de una instrucción.

Por lo tanto, si multiplico este periodo $T$ por el valor que me ha salido aquí —la `cuenta` que me ha salido del Timer 0—, ya tengo que $\text{cuenta} \times T$ es el tiempo total. Y con esto ya tengo la garantía de que sé con precisión cuánto tiempo estoy tardando en ese segmento de código, sin necesidad de haber contado una por una las instrucciones. Lo único que necesito es hacer una ejecución de ese segmento de código, mirar los valores del Timer 0 de un punto a otro, y determinar cuál es la cuenta y cuál es el tiempo.

¿Cuál es la única limitación que tiene este método? ¿Cuál es la máxima longitud en instrucciones que puede tener ese segmento de código para que el valor que estoy obteniendo sea correcto?

*(Un alumno responde: de 0 a 255, porque desborda).*

La limitación es que si tengo más de 255 instrucciones, el contador me vuelve a cero. ¿Cómo distingo, por ejemplo, el cero inicial de un cero que ya ha ocurrido porque ha habido un desbordamiento del contador? No podría distinguirlo. En particular, si comienza a contar en 0, pasa por 255, vuelve a 0 y después de ese 0 llega hasta 20, yo leería 20, pero no son 20; son $255 + 20 = 275$ instrucciones.

Esta sería una limitación si no fuera porque, como bien ha dicho él, podemos ser pillos y aprovechar que tenemos ahí un bit de acarreo (desbordamiento). Y si tenemos un bit de acarreo, ¿cómo puedo entonces medir la longitud de este segmento aunque tenga no ya más de 255, sino más de 512 o de 1024 instrucciones? Es decir, hacer este truco para mirar la longitud de un segmento de longitud arbitraria.

Porque la idea de usar el bit de acarreo está muy bien, pero estoy diciendo longitud arbitraria, así que el desbordamiento no tiene por qué ocurrir una sola vez; puede haber ocurrido varias veces. ¿Qué tendría que hacer entonces para poder obtener con precisión la lectura del número de instrucciones y, con unas pequeñas correcciones, obtener con precisión el tiempo?

*(Un alumno sugiere: con otra variable que cuente cuántas veces se ha reiniciado).*

La idea es buena: tienes una variable que cuenta cuántas veces se ha generado el bit de acarreo. El problema es cómo das valor a esa variable del bit de acarreo. Porque si lo haces por código, tienes que ejecutar código que esté mirando el bit de acarreo y luego escriba el valor; pero si empiezas a meter código que hace consulta periódica o bloqueo de programa para consultar el bit de acarreo, la hemos liado, porque te has cargado el seguimiento del código $S$.

Así que no puede ser basado en software mediante consulta periódica o bloqueo de programa. ¿Qué solución me queda? Y esto es Tema 4. Estos son problemas de diseño de ingeniería literal. Si no puedo hacerlo por software porque la introducción de software me altera lo que es el sistema bajo test (el sistema que estoy intentando monitorizar me lo altera la ejecución de código porque se está ejecutando en el mismo procesador que está ejecutando ese segmento de código), ¿qué tengo que usar por fuerza?

*(Un alumno responde: hardware).*

Hardware. ¿Y cómo se llama el mecanismo hardware que me permite avisar de que ha habido un cambio y necesito hacer algo para registrar ese cambio? No busquéis cosas complicadas, que es facilísimo. Estoy generando un bit de acarreo y lo que quiero es enterarme de que ha ocurrido el acarreo, pero no puedo estar usando código para monitorizar el bit de acarreo. Tengo que usar hardware. ¿Cómo se llama ese mecanismo?

¡Las **interrupciones**!

Vais entendiendo ahora, cuando hablamos de **rutina de servicio**, a qué nos referimos con rutina de servicio. Aquí tengo una rutina de servicio que es código que va a incrementar en uno el valor de la variable que me dice cuántos desbordamientos ha habido. Esa es la rutina de servicio. ¿Qué hago? Habilito la interrupción, de forma que ahora el bit de desbordamiento me genera una interrupción y esa interrupción me lleva al código que incrementa la variable en donde cuento el número de desbordamientos que ha habido.

¿Qué es lo único con lo que tengo que tener cuidado para que no se me altere la cuenta por haber usado la interrupción? Porque todo tiene su precio. El uso de las interrupciones me permite no tener que estar haciendo bloqueo de programa ni consulta periódica, por lo tanto me elimina la perturbación que me suponía la presencia de ese código, pero me introduce otra perturbación en la cuenta que tengo que resolver. ¿Cuál sería?

Fijaos que esto es un precioso problema de diseño y, al mismo tiempo, un precioso recordatorio del Tema 4, de cómo funcionan las interrupciones.

La interrupción hemos dicho que hace que el procesador vaya a ejecutar la rutina de servicio. Pero la rutina de servicio, ¿qué son? Instrucciones. Y si estoy contando el número de instrucciones que ejecuto, Timer 0 sigue incrementándose. Por lo tanto, el ejecutar la rutina de servicio va a hacer que ejecute una serie de instrucciones que me van a modificar la cuenta, y yo no quiero que me modifiquen esa cuenta.

Necesitaría, por tanto, si voy a tener más de 255 instrucciones y voy a usar interrupciones, de alguna forma descontar esa rutina de servicio del número total de instrucciones que estoy contando con el Timer 0. ¿Cómo lo haríamos?

De hecho, habéis visto en todo lo que hemos visto en clase todo lo necesario para resolverlo. Otra cosa es que un problema de diseño bonito requiera tener todavía la fluidez mental suficiente para poder resolverlo, pero saber, sabéis cómo hacerlo.

Vamos a resolver la parte más sencilla, y es cómo evitar que la ejecución de la rutina de servicio —vuestro `On Interrupt`— incremente Timer 0, o mejor dicho, cómo conseguir que Timer 0, cuando salga de la interrupción, siga contando por donde estaba.

*(Un alumno responde: guardando el valor).*

Guardándolo. Como la entrada a la rutina de servicio se va a producir por hardware inevitablemente, nada más entrar en la rutina de servicio guardas el valor de Timer 0. Y cuando sales de la rutina de servicio, como ese valor lo has guardado en una variable de memoria, cargas de nuevo el valor de Timer 0 tal como estaba.

De hecho, ahora que lo pienso, prácticamente sabemos cuál es el valor de Timer 0, porque si se ha producido un desbordamiento es porque ha pasado a 0. Y si ha pasado a 0, ha puesto a 1 el bit de interrupción. Y el procesador va a entrar en la rutina de servicio tan pronto vea el bit a 1, que va a ser al final de la siguiente instrucción. Por lo tanto, como mucho va a haber un incremento de 1 en el Timer 0. Ya habría que ver si le da tiempo al Timer 0 a ir de 0 a 1 cuando entra en la rutina de servicio o si directamente es 0. Pero es prácticamente garantizado que cuando se genera el desbordamiento, Timer 0 está en 0.

¿Qué es lo que sí tenéis que corregir en la cuenta? Porque estoy diciendo "Timer 0 va a entrar en 0", mentira. Timer 0 es 0 en el momento en que empieza a atender el microcontrolador la interrupción. Pero ¿qué es lo que va a ocurrir al inicio de la interrupción que me va a incrementar el valor de Timer 0 antes de que empiece la ejecución de mi código donde puedo leer el Timer 0?

En la descripción de cómo se atiende por un microprocesador una interrupción, hemos visto que cuando la línea de interrupción está activa y el procesador empieza a hacerle caso porque termina la ejecución de la instrucción en curso, ocurre algo al principio —en el diagrama aquel de flechitas que veíamos— que me va a incrementar el valor del Timer 0. ¿Qué es?

*(Un alumno dice algo sobre deshabilitar interrupciones).*

Eso es hardware, eso no incrementa Timer 0 porque no ejecuta software. De hecho, vimos en el ensamblador que me genera el programa de prueba que compilé ahí en la máquina virtual, código que se ejecutaba cuando entraba por el vector de interrupción 4 (el único vector de interrupción que tiene, que es la dirección 4) y vimos código que se ejecutaba al salir. ¿Qué es lo que estaba haciendo ese código?

*(Un alumno responde: el cambio de contexto).*

Exacto, el **cambio de contexto**. El salvar el contexto requería, si recuerdo bien, 4 instrucciones. Así que, antes de entrar en la rutina de servicio —es decir, antes de la primera instrucción donde yo puedo leer el valor de Timer 0— ha ejecutado 4 instrucciones. Por lo tanto, suponiendo que Timer 0 entra con 0 (suponiendo que no le da tiempo a incrementarse a 1 cuando entra en la rutina de servicio), ¿qué es lo que nos vamos a encontrar cuando leamos Timer 0 en la primera instrucción? Un 4.

Y cuando salgamos de la rutina de servicio, ¿qué valor tendrá Timer 0? 8 (4 de entrada más 4 de salida) más el número de instrucciones que haya ejecutado. ¿Me importa realmente cuál sea el valor de Timer 0 en ese momento? Pues la verdad es que no. ¿Qué es lo único que tengo que hacer para restaurar la cuenta al valor correcto?

Como sé que entra en 0 (o en 1; habría que mirar con precisión si le da tiempo a ponerse a 1, y para saber si le ha dado tiempo basta con mirar si al leer Timer 0 la primera vez os encontráis un 4 o un 5: si es 4 es que no le ha dado tiempo, si es 5 es que le ha dado tiempo a incrementarse en uno), en cualquiera de los casos sabéis con qué valor ha entrado: o bien 0 o bien 1. Lo que tenéis que hacer entonces es ponerle de nuevo el 0 (o el 1) y descontarle el valor correspondiente al cambio de contexto.

Esa sería la forma —un poquito alambicada, pero es la forma— de poder aprovechar las interrupciones y el *flag* de acarreo para hacer la cuenta de un segmento de longitud arbitraria. Aquí entre nosotros, no se hace con segmentos de longitud arbitraria porque no tiene sentido: si tú estás queriendo ver un segmento crítico, ese segmento crítico tiene que ser muy cortito y muy eficiente, así que normalmente va a tener poquitas instrucciones y será raro que tenga más de 255 instrucciones.

Y hay otro detallito ya de hardware que hay que tener en cuenta, como que por ejemplo cuando escribís en Timer 0, en realidad tarda ciertos ciclos de reloj en terminar la escritura en el registro y usar el valor que habéis escrito, o sea que el ajuste tendría que ser un poquito más sofisticado. Pero para el propósito que queríamos nosotros, que era repasar el proceso de interrupciones —qué es lo que hace la interrupción, para qué me ha servido (para no tener que ejecutar código haciendo consulta periódica que me trastorna la cuenta) y lo que involucra desde el punto de vista del cambio de contexto—, es un buen ejemplo para repasar toda la parte de interrupciones.

¿Preguntas, dudas, cuestiones?

---

## 4. Controlador de interrupciones

Esto es la punta que le hemos podido sacar a la forma de gestionar las interrupciones que tiene el PIC, que es el mecanismo de **muestreo (polling)**, en donde tenemos una única línea de petición de interrupciones que involucra, por tanto, que el código de la rutina de servicio vaya mirando el registro de estado de cada uno de los periféricos que pueden pedir la interrupción. El orden en que mira los registros de estado es el orden de priorización de los periféricos y, por lo tanto, el periférico que está el último de la cadena siempre es el que va a tener una atención más lenta a su petición de interrupción, porque hay que muestrear todos los anteriores aunque no hayan pedido interrupción.

Si tenemos múltiples periféricos que pueden pedir interrupciones, ¿hay otra forma de gestionarlos? Sí, mediante lo que se llama un **controlador de interrupciones**.

Es decir, meter hardware adicional —esto es básicamente un periférico glorificado— en donde tenemos una línea de petición de interrupciones individual por cada periférico (por cada interfaz de periférico) que pueda pedir interrupciones. Y el controlador de interrupciones va a hacer dos cosas:

1. **Centralizar y priorizar la generación de la señal de interrupción**, de forma que solamente uno de ellos generará la petición de interrupción hacia la CPU, y el controlador determinará el orden de prioridad por hardware para ver a quién le hace caso en la situación en que más de un periférico haya pedido interrupción simultáneamente.
2. **Generar un vector de interrupción** que, a través del bus de datos, entregará a la CPU para decirle cuál ha sido el periférico que ha pedido la interrupción —o mejor dicho, a cuál de los periféricos que han pedido la interrupción es al que le ha dado la prioridad y ha generado la interrupción en su nombre—.

Si queremos usar una terminología cercana a las páginas web, ahí lo que tenemos es un *proxy*. El controlador de interrupciones es un *proxy* que está recibiendo las peticiones de interrupción y, de acuerdo con un algoritmo hardware, determinando que la petición que viene de uno de ellos es la que va a ser trasladada a la CPU. Y el vector de interrupción sirve para identificar en nombre de cuál de los periféricos el controlador de interrupciones ha generado la petición de interrupción.

Como digo, el controlador de interrupciones realmente es un periférico glorificado. Como es un periférico glorificado, ¿qué es lo que tienen todos los periféricos? Una interfaz, con unos registros de control y de estado, y un modelo de programación que determina cómo programar el controlador de interrupciones y cómo funciona.

Y el controlador de interrupciones va a tener dos partes:
1. La parte hardware que resuelve el problema de cómo priorizar las peticiones de interrupción y cómo generar el vector de interrupción.
2. Una segunda parte, esta de entrada, que son lo que se llaman las **máscaras de interrupción individuales**.

### 4.1. Máscaras de interrupción individuales y comparación con el PIC 16F877A

Aquí hago el comentario de que el microcontrolador PIC 16F877A es un sistema un tanto peculiar porque está mezclando mecanismos. Vamos un momentito a la lógica de generación de interrupciones del PIC.

Como veis, en el microcontrolador PIC el esquema que está usando para generar las interrupciones —esta es la línea de petición de interrupción que va al procesador— es el que ya conocemos, el de *polling*: interrupciones encadenadas, todas conectadas mediante puertas OR a la misma línea de petición de interrupción, donde todos los posibles orígenes de interrupción terminan en una única línea de petición de interrupción. Por lo tanto, es el esquema de muestreo (*polling*), que es el esquema más sencillo y más cutre de petición de interrupciones.

Pero aquí vemos estas puertas AND: vemos que para cada uno de los orígenes distintos de petición de interrupciones tenemos una puerta AND y un bit de habilitación (*enable*). La `E` me está indicando que es un bit de habilitación. Por ejemplo, `TMR0IE` (o `T0IE`) es el bit que habilita la petición de interrupción del temporizador Timer 0. ¿Cómo lo habilita?
- Si ponemos a `1` esa línea, esta puerta AND básicamente no hace nada: el valor que tenga aquí el *flag* `TMR0IF` (`T0IF`) es el mismo valor que vamos a tener aquí en la salida.
- Si, por el contrario, la máscara de interrupción (el bit de habilitación) está en `0`, da lo mismo lo que tenga en el *flag* de petición de interrupción —es decir, da lo mismo lo que pase con la petición de interrupción del Timer 0—, que aquí a piñón voy a tener un `0`.

Por lo tanto, esta línea de habilitación está funcionando como **máscara de interrupción individual** de la interrupción del Timer 0.

Y esa es la razón de que en el registro `INTCON` —ya lo había comentado— tenemos una parte que es registro de control donde tengo la **máscara de interrupción global** (`GIE`), que lo que hace es que habilita o deshabilita el subsistema completo de interrupciones. ¿Cómo lo habilita o deshabilita? Con otra puerta AND que está haciendo el enmascaramiento del valor de la línea `INT` en este punto, de forma que si la máscara de interrupciones globales está en `0`, da lo mismo lo que pase con el sistema de interrupciones por aquí, que no va a generar interrupción ninguna; eso siempre permanecerá en `0`. Por lo tanto, `GIE` es la máscara global de interrupciones y hay que ponerlo a `1` si queremos que haya interrupciones.

Pero luego aquí tenemos la máscara de interrupción individual para, en particular, varios orígenes de interrupción. Esta es la máscara de interrupción individual para el Timer 0, de forma que tenemos que explícitamente poner un `1` ahí para habilitar la interrupción del Timer 0, pero el haber habilitado la interrupción del Timer 0 no habilita ninguno de los otros orígenes de interrupción. Eso permite hacer un control individualizado de cada uno de estos orígenes, de forma que vosotros, por ejemplo, vais a usar estas dos máscaras: la petición de interrupción externa de la línea `INT` y la petición de interrupción interna generada por el temporizador Timer 0.

En este sentido, el PIC utiliza un mecanismo que realmente lo encontramos en controladores de interrupción; solo que, en lugar de llevar un controlador de interrupción entero porque este microcontrolador es muy sencillo, lo que tiene el PIC es únicamente esta parte de lo que sería la implementación de un controlador de interrupciones más sofisticado: el enmascaramiento individual de los orígenes de interrupción.

Y si os fijáis en este dibujo del controlador de interrupciones, el mecanismo coincide exactamente con el mismo que tenemos en el PIC: tenemos el registro de control donde tenemos los bits individuales de máscara que debemos poner a `1` para que la puerta AND permita que la petición de interrupción que viene del periférico correspondiente pase al resto del controlador de interrupciones.

Así que esta parte del controlador de interrupciones está también implementada en el PIC. ¿Qué es lo que no implementa el PIC? Nada de lo que hay en esta otra parte, porque ya sabemos que la priorización hay que hacerla por software y la generación de la información de quién es el origen de la interrupción hay que hacerla por software en el caso del PIC, porque es *polling*.

### 4.2. Priorización por hardware: el codificador con prioridad

En cambio, en un controlador de interrupciones, el hardware es el que se encarga de hacer la priorización. ¿Cómo? Si estuviéramos en el segundo semestre ya habríais visto **codificadores**, que son circuitos combinacionales que básicamente tienen aquí un número de bits de salida $n$ —en este caso $n = 2$— y tienen un número de entradas que es, en binario natural, $2^n$. Como en este caso $n = 2$, necesitamos por tanto 4 entradas distintas ($2^2 = 4$).

Lo que hace el codificador es que si hay un `1` en una de esas líneas —vamos a suponer la línea de índice 2—, el código de $n$ bits de salida (en este caso los 2 bits de salida) se pone al código binario natural que me da el valor del índice de esta línea. Esta línea tiene el índice 2, por lo tanto aquí necesito —fijaos que este es el bit 1, el más significativo— un `10` en la salida, que sería el 2, y eso es lo que me está dando el codificador.

¿Qué es un **codificador con prioridad**? Es lo mismo, un circuito combinacional que cuando tengo una entrada activa me genera el código binario del índice de esa entrada, pero cuando tengo más de una entrada activa utiliza hardware para determinar una priorización de cuál de los índices es el que va a generar por la salida. En particular, típicamente el `0` es la entrada más prioritaria y, conforme va subiendo el índice, tenemos menos prioridad.

Si aquí tenemos la línea 1 y la línea 2 activas, el codificador con prioridad ve que la línea más prioritaria es la 1 y, por lo tanto, aquí generaría un `01`, que es el índice 1, pese a que también el índice 2 está activo. La priorización por hardware hace que directamente, sin necesidad de muestreo ninguno, me saque el valor de la línea más prioritaria.

- **Ventaja de esta codificación con prioridad:** no necesita muestreo, no necesita software; es más rápido porque toda la atención la hace por hardware.
- **Inconveniente:** ¿quién es más prioritario, $P_2$ o $P_3$? $P_2$. ¿Y si quiero que $P_3$ sea más prioritario que $P_2$, qué tendré que hacer? Cambiar el orden físico. En *polling* era muy sencillo porque cambiabas el orden de muestreo y era por software. Aquí necesitas físicamente sacar el periférico de la placa de expansión y conectarlo a otro *slot* de la placa de expansión para cambiar el orden de priorización de la interrupción.

Por lo tanto, es más complejo desde el punto de vista hardware, pero más rápido.

### 4.3. Generación del vector de interrupción, excepciones y registro base

Aquí recibe este valor en binario que es el vector de interrupción —o mejor dicho, es el número base que me permite generar el vector de interrupción—. ¿Por qué digo eso del número base que me permite generar el vector de interrupción?

¿Qué números podría generar con estos dos bits en este ejemplo? `00`, `01`, `10`, `11`; o lo que es lo mismo, los valores que podría obtener como vectores serían 0, 1, 2 y 3. Pero estos valores son muy malos como valores base de vectores de interrupción. ¿Por qué?

Recordad que el sistema operativo ofrece una **tabla de vectores de interrupción**. Si es un vector de 8 bits, tenemos una tabla en donde el primer índice es 0, el segundo índice es 1, y el último índice es 255. Cada una de estas entradas apunta a una dirección de rutina de servicio. Ese es el esquema que ya conocemos.

Una cosa que suele suceder en todos los microprocesadores —cuando tenemos una tabla de vectores de interrupción con más de un vector— es que los valores iniciales de la tabla de vectores de interrupción (las interrupciones 0, 1, 2, 3...) se utilizan por el propio hardware del procesador para lo que se llama **generación de excepciones**.

Por ejemplo, si se os ocurre hacer una división por cero —cogéis el número 9 y lo dividís por 0—, esto me va a generar un error porque una instrucción de división no puede dividir por cero. ¿Qué es lo que hace el microprocesador? Genera automáticamente una interrupción llamada **excepción**, en donde hay una rutina de servicio aquí que llama a una rutina del código del sistema operativo que es la que se encarga de sacar un mensaje por pantalla que diga: *"Estúpido, estás haciendo una división por cero, esto no es posible; paro el programa porque es un error grave"*. Esa rutina de servicio se encarga de ese trabajo sucio y eso lo hace automáticamente el procesador porque el hardware de la unidad de control está programado para que, si aparece esta situación excepcional, llame al vector de interrupción (en este caso, digamos, el 1) y llame a esa rutina de servicio.

Eso significa que estos valores del 0 al 3 son muy malos para usar porque ya están usados. ¿Qué me interesaría? Pues, por ejemplo, poder poner el valor a partir del 60: 60, 61, 62 y 63. Yo quiero tener el valor 60 como punto de partida de mis rutinas de servicio de interrupciones para usuario.

¿Cómo se puede conseguir esto en el esquema que tenemos aquí? Usando un **registro base**. Es un registro de control que forma parte de la interfaz del programador del controlador de interrupciones. Esto básicamente guarda el valor base —el valor origen— de los vectores de interrupción que quiero generar, y aquí en este registro base es donde escribiría `60`. Y entonces, usando un sumador, el valor en binario que viene por aquí (que es del 0 al 3) se sumaría al 60 que tengo como vector base, y aquí el valor de vector de interrupción que me sale es del 60 al 63. Y eso es lo que va hacia el procesador como vector de interrupción.

Así que esto es lo que hace un controlador de interrupciones, que como veis es un periférico glorificado. Primero, ya sabemos lo que hace la interfaz y lo que hacen esos registros (de control y estado), y ya sabemos también cómo programarlo: usaremos **PIO** (E/S programada) al inicio del arranque del sistema operativo —o durante la configuración del controlador de interrupciones por parte de nuestro código— para:
1. Escribir en el registro de control de la máscara de interrupciones individuales el enmascaramiento que habilite o deshabilite los periféricos que necesitemos.
2. Escribir la dirección base del vector de interrupción.
3. Escribir la señal de máscara de interrupción global, que permite la salida de la petición de interrupción.

Esto requiere la presencia de un hardware adicional que típicamente estará hoy día dentro del propio procesador y se suele llamar **APIC**. Donde veáis las siglas APIC, lo que están poniendo es *Advanced Programmable Interrupt Controller*: controlador de interrupciones avanzado y programable. Ya son controladores que tienen una funcionalidad no solo como esta que estoy describiendo, sino todavía más sofisticada. Ese APIC típicamente hoy día está integrado dentro del propio procesador y lo que ofrece son las líneas de petición de interrupción hacia el exterior para que los periféricos puedan pedir interrupciones.

Y no tiene mucho más. Las interrupciones con esto terminan y ya lo siguiente sería DMA, que ya he dicho que no entra para el examen.

---

## 5. Dudas finales y formato del examen parcial

¿Dudas, preguntas, cuestiones?

¿Vais viendo qué pinta puede tener el examen? Fijaos cómo no es memorizar. Hay que haberse estudiado las transparencias y el contenido, pero lo que os van a requerir las preguntas es haberlo entendido. Y habéis visto también cómo ya habéis pasado por tener que resolver preguntas de ese estilo, porque las hemos hecho un montón de veces aquí en clase. No hay que memorizar nada —repito, hay que haberse estudiado el tema, pero no es memorística, no es aprenderse de memoria un rollo de las transparencias y soltarlo—, sino que con haber entendido las ideas y tenerlas bien claras, no deberíais tener problemas con el aprobado.

No me vengáis con sueño, tomaos un café antes, esas cosas. En principio no deberíais tener problema para incluso sacar nota en el examen.

*(Pregunta de una alumna: ¿Más o menos cuántos ejercicios son?)*

10 preguntas. En los parciales, salvo que se me vaya la pinza y ponga una pregunta compleja que tenga dos subpreguntas —que entonces vale 2 puntos en vez de 1—, normalmente lo que pongo son 10 preguntas. Sobre todo por no concentrar demasiados puntos en una pregunta. Porque si hay muchos puntos concentrados en una pregunta y tienes la mala suerte de que te pilla mal, pierdes todos esos puntos y, claro, no es lo mismo perder 1 punto de 10 que perder 3 puntos de 10 y quedarte con solo 7 puntos para poder aprobar.