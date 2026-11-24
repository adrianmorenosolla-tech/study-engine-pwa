Frente a las fronteras de valores enteros, que necesitamos para poder entender qué vamos a hacer en la práctica, puesto que el próximo día ya entramos a trabajar en ensamblador, y ya pasaremos al binario natural. Y complemento a dos.

Habíamos comenzado con la representación de enteros posicionales y de valores, solamente repaso. Recordar que las representaciones posicionales se caracterizan porque vamos a tener ceros y unos, vamos a tener un cierto número *n* de bits. El número de bits que vamos a tener viene determinado por el valor máximo del número que tengamos que representar, de acuerdo a la necesidad que tengamos en nuestro problema.

Una vez que tenemos ese número de bits, el valor en binario va a ser una cadena de ceros y unos. En una representación posicional, el valor que tiene cada uno de los dígitos va a venir dado por su posición, es decir, por el índice de la potencia de dos correspondiente a ese coeficiente.

Si os ponéis un puntito quisquillosos, la representación en complemento a dos es una representación posicional, pero la correspondencia entre el valor numérico de la cadena representada y la cadena en sí requiere dos pasos. No es una representación en donde directamente la posición de los bits en la cadena me da el valor numérico. Pero en el caso del binario natural, binario puro, entero sin signo, es una representación posicional pura y dura, y realmente no tiene absolutamente ninguna complicación.

Las dos cuestiones serían: uno, cómo convierto un número decimal a binario natural, cómo convierto un número de binario natural a decimal, y cómo elijo el número de bits necesarios.

Vamos a empezar por el final, y lo primero que vamos a ver es cómo determinar el número *n* de bits que vais a tener en la representación, de forma que el exponente máximo de la potencia de dos que vais a manejar va a ser *n - 1*.

De nuevo repito, hasta la extenuación: al informático le encanta empezar las cuentas con el índice cero. Así que recordad que siempre la posición menos significativa es dos elevado a cero, dos elevado a uno, y por tanto, la posición más significativa es dos elevado a *n - 1*, donde *n* es el número de bits.

¿Cómo determinar ese número de bits *n*?

Lo primero que tenéis que saber es, ¿cuál es el valor máximo que vais a representar?

¿Cuál es el valor mínimo que voy a representar? Cero. Binario puro. No tiene números negativos. No hay números negativos en binario natural, y por lo tanto, el número más pequeño que puedo representar va a ser el cero.

¿Y el número más grande que puedo representar? El número más grande es infinito.

De acuerdo a la especificación de mi problema, voy a determinar cuál es el valor máximo que puedo tener que encontrar representado con la cadena de ceros y unos. Por ejemplo, si es un sensor de temperatura (me gustan mucho los sensores de temperatura), para un habitáculo humano, es decir, donde estamos nosotros.

¿Cuál sería el rango? Lo primero, un sensor de temperatura genérico para cualquier entorno humano, ¿podemos usar binario natural?

- No.
- Sí.

Ok, he oído un sí y he oído un no. Vamos a comenzar por el sí. Venga, sí. Dímelo. ¿Por qué?

- Porque has dicho que en un sitio cerrado donde van a estar personas, yo me imagino que de normal una persona no va a estar en un lugar por debajo de los cero grados.

Ahí es donde conecto con lo del no. ¿Cómo se las gastan en los países nórdicos? Suecia, Finlandia, Noruega, en invierno.

- En la calle sí, estaba pensando en un habitáculo, un refugio.

Dentro de las casas sí, pero por ejemplo, esos refugios que están dentro de las casas si tienes un cuarto intermedio, un pasillo entre la entrada donde te estás helando vivo y el cuarto donde ya tienes la calefacción, ¿qué temperatura puede tener? Perfectamente puedes tener temperatura negativa en alguna parte de la casa.

¿Dónde van a estar los humanos? La palabra clave que podíais decirme es: ¿va a estar el humano en situación de confort o va a estar simplemente el humano?

Porque si va a estar simplemente el humano podemos tener temperatura negativa, claro. Si va a estar en confort, es decir, sin tener que estar abrigado como los esquimales, efectivamente no vamos a estar por debajo de cero grados. De hecho, yo diría que no vamos a estar por debajo de diez grados.

Por eso es importante la especificación del problema, porque de acuerdo a las especificaciones va a tener una u otra situación. En el caso más general, en donde no he especificado que el ser humano está en confort, podemos tener temperatura negativa. ¿Con temperaturas negativas podemos usar binario natural?

- No, en ese caso no.

No podemos. ¿Qué opciones tendríamos? Esperar un rato a la clase y aprenderemos a usar complemento a dos. También signo y magnitud, pero complemento a dos. La otra posibilidad, y que tampoco hay que descartarla: ¿y si tenemos un rango de temperatura que va de diez grados bajo cero a veinte grados sobre cero y lo quiero representar en binario natural?

- ¿Con Kelvin? ¿Convertirlo a grado Kelvin?

Buena pregunta. Eso lo vamos a usar después, pero lo primero. Esa sería una solución excelente y hay formas más elegantes de resolver esto. Pero si os obligo a usar binario natural para representar este rango de temperatura, ¿qué se os ocurriría?

- ¿Añadirle veinte?

Un offset. Sumarle un desplazamiento. Y donde pones de menos diez a veinte, pones de cero a treinta.

Acabáis de solucionar el problema y por cierto acabáis de reinventar lo que se llama notación en exceso a M, que la vamos a ver un poquito más adelante como notación posicional. Consiste literalmente en esto: desplazar un rango de valores enteros a otro rango de valores enteros que podamos representar como binario natural, y por tanto el valor más pequeño posible es cero en la representación. Para recuperar el valor normal lo que hacemos es eliminar el exceso, que aquí el exceso sería diez. Para obtener el valor real en este rango lo que hacemos es que al valor representado en la cadena de binario natural, le restamos el exceso M.

Es decir, sería cero menos el exceso M. Y aquí tendrías diez grados bajo cero. Pero con la ventaja de que he trabajado en binario natural.

Como digo, esto lo vamos a volver a ver, pero ya acabáis de entenderlo. Un poquito más adelante, en la notación en exceso a M, como veis, no es más que una forma inteligente de desplazar una representación para meterla en una representación de binario natural. ¿Por qué? Porque a lo mejor trabajar con binario natural tiene ciertas ventajas, como por ejemplo que no me complico la vida con números negativos y por tanto con complemento a dos.

Esa era una de las formas. La otra forma era lo de Kelvin. Sí.

Grados Kelvin no es mala idea. ¿Cuál es el cero grados centígrados en Kelvin?

- Doscientos setenta y tres.

Doscientos setenta y tres. Por tanto, si lo pasamos a Kelvin, esto serían doscientos sesenta y tres grados Kelvin. Hasta doscientos noventa y tres grados Kelvin.

Correcto. Ya tenemos una representación que es enteramente positiva, así que ya es un binario natural. La preguntita del millón sería: ¿cuántos bits tengo disponibles para representarla? Esta es la parte que no os he dado todavía en la especificación del problema. Pero tenía que dárosla para que tengáis la información completa. Porque si no os digo cuántos bits podéis tener como máximo, esta solución es perfectamente válida.

Ahora bien, si os digo que como mucho podéis usar un byte para representar el valor entero, no es posible. ¿Por qué?

Porque en un byte (uno, dos, tres, cuatro, cinco, seis, siete, ocho bits), recordad, estamos hablando de dos elevado a cero, a uno, dos, tres, cuatro, cinco, seis, siete. Por lo tanto, estamos hablando de los pesos: uno, dos, cuatro, ocho, dieciséis, treinta y dos, sesenta y cuatro, ciento veintiocho.

Aquí en estas transparencias no os pone explícitamente el mecanismo de conversión de decimal a binario. En estos conversores y generadores de ejercicios que tenéis en el Campus Virtual sí que os lo pone, en particular este de aquí. Yo puedo tener, por ejemplo, el número doscientos cincuenta y uno, y si lo convierto a binario, no solo me dice cuál es la cadena resultante, sino que además me dice cuál es el procedimiento a seguir para hacer la conversión de un número decimal a binario.

Para pasar de decimal a binario, vamos dividiendo progresivamente por dos, nos quedamos con el resto, y el cociente es el valor que va a ser dividendo en el siguiente paso. De forma que vamos progresivamente dividiendo por dos y, básicamente, si os sale impar es uno, si os sale par el resto es cero. Leyendo la cadena de restos obtengo el valor de la cadena binaria, determinando el bit menos significativo y el bit más significativo. Obtengo el valor de la cadena binaria.

Este es el procedimiento estándar que ponen en los libros de texto. Hay una forma más sencilla y más indolora. Y para convertirlo de binario a decimal es muy simple, la inversa: simplemente tomando el peso del coeficiente (dos elevado a cero por uno) e ir sumando el valor. Este es el procedimiento estandarizado que encontraréis en los libros.

La verdad es que no merece la pena perder el tiempo haciendo esto. Es infinitamente más rápido aprenderse las potencias, como vais a estar manejando un número reducido de bits. Normalmente los números que vamos a trabajar nosotros, más allá de mil veinticuatro, de diez bits, no los vamos a usar nunca explícitamente manejándolos a mano. Lo más habitual es que estemos trabajando solamente con un byte. Si estamos trabajando solamente con un byte, las potencias que hay que aprenderse de memoria son triviales: uno, dos, cuatro, ocho, dieciséis, treinta y dos, sesenta y cuatro, ciento veintiocho, y si me apuráis la siguiente, doscientos cincuenta y seis, que sería ya el bit nueve.

¿Cómo determinar entonces el valor en binario de un valor decimal, por ejemplo, el doscientos cincuenta y uno?

Tenemos ocho bits, pues vamos tomando la potencia de dos correspondiente más alta y vemos si con eso nos pasamos, nos quedamos cortos, o estamos justos.

Si por ejemplo fuera directo el ciento veintiocho el valor a representar, no hace falta calcular nada: un uno en esta posición, cero en las demás, se acabó. Cuando es un poquito más entretenido, doscientos cincuenta y uno. Primer valor ciento veintiocho, el menor. Ponemos un uno, y a doscientos cincuenta y uno le resto ciento veintiocho.

Quedan ciento veintitrés. Ciento veintitrés es mayor que sesenta y cuatro, así que también lo necesito; le resto sesenta y cuatro y quedan cincuenta y nueve.

Cincuenta y nueve es mayor que treinta y dos, de nuevo necesito el uno. Le resto treinta y dos y quedan veintisiete.

Veintisiete es mayor que dieciséis, sigo necesitando el uno. Le resto dieciséis y quedan once. Y para el once no necesito ni hacer la resta: son ocho, dos (diez), y uno (once). Ya está.

Esto es para un número grande y engorroso. Pero si tengo que representar, por ejemplo, diecisiete. Diecisiete es trivial: dieciséis y uno. Por lo tanto, un diecisiete en binario sería tan sencillo como poner el bit del dieciséis a uno y el del uno a uno, todo lo demás a cero.

Para un número pequeño de bits esto es, con diferencia, la forma más rápida de hacer la conversión y funciona de verdad, es mucho mejor que la división. Y si tenemos un número grande de bits, si tenemos que trabajar con dieciséis, treinta y dos, sesenta y cuatro bits, no os compliquéis la vida haciendo el trabajo a mano. Para eso tenéis la calculadora de Windows.

La calculadora de Windows en particular tiene un modo que se llama programador, que nos sirve justamente para resolver este tipo de conversiones.

- Pregunta: ¿Qué pasa si el número es mayor y necesitamos sumar más valores?
- Necesitas más bits.

Si tenías un número mayor de ciento veintiocho. Voy a poner otro número mayor que ciento veintiocho: ciento treinta y dos.

Ciento treinta y dos. ¿Qué sería? Ciento veintiocho. O sea, aquí tenemos los pesos: ciento veintiocho, sesenta y cuatro, treinta y dos, dieciséis, ocho, cuatro, dos, uno. Ciento treinta y dos son ciento veintiocho y cuatro. Por lo tanto, marcamos el ciento veintiocho y el cuatro a uno, los demás a cero. Listo, ya está.

- Pero mi pregunta era si, por ejemplo, tenemos un número mayor a doscientos cincuenta y cinco y me dicen doscientos setenta y cinco.
- Ah, ahí es donde la has cagado. Porque, ¿cuál es el número más grande que puedes representar con ocho bits? Doscientos cincuenta y cinco. No puede ser entonces un número mayor que doscientos cincuenta y cinco, porque necesitarías más bits.

Sí puedes representar más de doscientos cincuenta y cinco, pero ya te tienes que ir a dieciséis bits. Con ocho bits no puedes.

Lo que estabas pensando es correcto, pero ya necesitas más bits y, por tanto, solamente ocho bits no te valen. En la calculadora de programador de Windows, tenéis para poner el valor decimal y convertirlo a binario. Fijaros que aquí además tenéis un par de cositas útiles.

Primera cosita. El teclado numérico de alternancia de bit, es un nombre muy pedante para decirte que te muestro todos y cada uno de los bits individuales de la representación en binario que me estás pidiendo, de acuerdo con el tamaño de representación que es este parámetro de aquí.

Yo puedo decirle que quiero una cuádruple palabra, es decir, sesenta y cuatro bits. O puedo pedirle una doble palabra, que es la mitad, treinta y dos bits. O puedo pedir una palabra, dieciséis bits, o puedo pedir simplemente un byte, ocho bits.

Fijaros cómo aquí en negrita me está poniendo los valores válidos, y en gris me está poniendo los bits que de acuerdo a esta representación no puedo tener.

Si ahora aquí en decimal, le intento poner doscientos cincuenta y seis, fijaros que no me deja, el seis no me deja escribirlo. ¿Por qué? Porque con ocho bits nada más que podemos meter hasta el doscientos cincuenta y cinco.

¿Por qué me ha obligado a poner los dieciséis bits para que me deje representar doscientos cincuenta y cinco si cabe en ocho bits? Porque esta calculadora está trabajando en complemento a dos. En complemento a dos, el bit más significativo de los ocho bits es un bit de signo. Y entonces, claro, realmente solo tienes siete bits para representar valores. Por eso con el byte no me dejaba poner el valor entero natural.

La versión corta: si tenéis que trabajar con valores grandes, no os compliquéis la vida. Calculadora de Windows. Y además la calculadora de Windows tiene otra cosa interesante y es que, por ejemplo, podemos poner mil veinticuatro, y aquí tenéis directamente la conversión a hexadecimal. Lo cual es tremendamente útil. Y aquí tenéis el binario correspondiente.

Para números pequeños, es aprender de memoria las potencias, que es lo que vais a estar usando con tanta frecuencia en toda vuestra carrera, que lo vais a terminar sabiendo, así que cuanto antes empecéis mejor.

El elegir el valor de acuerdo al número de bits que necesitemos. Si tenemos más de doscientos cincuenta y seis valores distintos. Es conveniente saber cuáles son los límites. De cero a doscientos cincuenta y cinco son doscientos cincuenta y seis valores, es lo que se puede guardar en ocho bits.

Con dieciséis bits, dos bytes. Sería básicamente doscientos cincuenta y seis por doscientos cincuenta y seis. Lo tenemos aquí en la calculadora. Sería de cero a sesenta y cinco mil quinientos treinta y cinco, es decir, sesenta y cinco mil quinientos treinta y seis es el valor que se puede representar con dieciséis bits, dos bytes. Y más allá de esto no merece la pena ponerse a calcular, porque si os vais a sesenta y cuatro bits te sale un número gigante.

Estos realmente son los dos límites que os interesa conocer de memoria para determinar cuántos bits o cuántos bytes necesitáis en una representación interna en memoria de acuerdo con lo que os diga la especificación que necesitáis guardar.

Vamos a hacer un ejemplo. Vamos a suponer que tenemos un avión de pasajeros, de aviación. En el computador de abordo, voy a tener que manejar el valor de la velocidad en nudos del avión. ¿Cuál es el mínimo valor de velocidad que voy a tener?

Obviamente no voy a tener una velocidad negativa, si el avión está parado va a ser cero.

El valor máximo. Si es un avión de pasajeros no va a ser supersónico, por lo tanto vamos a estar por debajo de los mil nudos. Vamos a llamar a este valor A, donde A va a ser menor o igual que mil. Este es el rango de valores de velocidad que va a proporcionar el sensor del velocímetro del avión.

Además os digo que este sensor está proporcionando el valor de velocidad a un ritmo de cuatro veces por segundo. Y que tenéis que guardarlo en memoria para hacer el procesamiento de los últimos dos minutos.

Tenemos dos minutos de muestras, y cuatro muestras por segundo. La pregunta que yo os hago es, ¿cuánta memoria RAM necesito para almacenar esas muestras en estas dos situaciones? Uno. Cuando el velocímetro me entrega directamente el valor como entero, en binario natural.

Como el valor mínimo es cero, significa que voy a poder usar binario natural. Necesito saber cuánta memoria voy a necesitar para almacenar los dos minutos de muestras. Y además, voy a transmitir esas muestras, los valores de esas muestras, a un segundo sistema, a un segundo ordenador.

La transmisión de esas muestras que tengo en memoria me va a requerir... pregunto, ¿la transmisión puedo hacerla o bien en ASCII, o bien en UTF-8? En cualquiera de los casos estamos hablando de que vamos a usar una rutina de C para hacer el procesamiento de cada muestra.

- ¿Una muestra cuánto ocupa?

Eso es lo que te estoy preguntando. Fíjate que con los valores que te he dado, no te he dicho cuánto ocupa cada muestra, necesito que determinéis cuántos bytes necesitáis para cada muestra.

Para determinar cuántos bytes necesitas para cada muestra, ¿qué es lo primero que tienes que ver? Formato. Ya te he dicho, binario natural. Segundo, rango de valores representables. Mínimo cero, máximo un valor menor o igual que mil.

Si el valor es menor o igual que mil, ¿cuántos bytes necesito para cada muestra, en binario natural?

¿Por qué? Recuerda, siempre justifica la respuesta, va a venir en la cartilla de examen: siempre, siempre, justifica la respuesta. El rango de un solo byte de representación es solo de doscientos cincuenta y seis valores, de cero a doscientos cincuenta y cinco. Como tenemos que llegar a mil, inevitablemente tenemos que irnos a dos bytes.

¿Son necesarios los dos bytes o me van a sobrar bits?

Si llegamos a mil y este llega a sesenta y cinco mil, van a sobrar. De hecho, si quisiera buscar las cosquillas (que no las voy a buscar), podría pediros ¿cuántos bits me van a sobrar en el inicio de la representación? Y entonces, ¿qué tendríais que calcular? Que dos elevado a diez es mil veinticuatro, con mil veinticuatro ya cubres los mil valores, y por tanto con diez bits serían suficientes.

Pero no voy a pedir eso. Porque directamente ya os he dicho que no se manejan fracciones de bytes, siempre se manejan los bytes absolutos. O es un byte o son dos bytes.

Por lo tanto ya sé que las muestras requieren el uso de dos bytes. Siguiente cuestión, ¿cuántas muestras voy a almacenar?

¿Cuántas muestras por segundo por dos minutos? Son sesenta segundos por minuto, por cuatro. Doscientas cuarenta muestras. Cada muestra requiere dos bytes, por lo tanto necesito cuatrocientos ochenta bytes de almacenamiento para guardar en binario natural las muestras de esos dos minutos.

Esa es la parte fácil del problema, la sencilla. Esta ya requiere pensar un poquito más.

Los voy a transmitir. Y estoy diciendo: ASCII o UTF-8. ¿Qué es lo que estoy diciendo con esto? ¿ASCII o UTF-8 son representaciones binarias? ¿Qué son?

- Alfanuméricas.

Por lo tanto aquí lo que estoy diciendo implícitamente, es que para hacer esta transmisión, yo tengo que hacer una conversión del número entero a cadena de caracteres. Por eso además especifico lo de la rutina de C.

Para pasar de aquí a aquí tengo que determinar cuántos bytes me va a consumir cada valor que voy a transmitir. Es decir, cada cadena de caracteres. ¿Cuál es el número de caracteres que me va a consumir?

- Porque el mil tiene cuatro cifras.
- ¿Y por qué no menos? O sea, porque mil es la velocidad máxima que puede alcanzar el avión. Pero si el avión está despegando, ha empezado en cero, y ha ido creciendo la velocidad. ¿Por qué no usas menos, en donde ha tenido velocidades de dos dígitos o un dígito?
- No sabes la velocidad a la que va.

El problema es la regularidad. No tienes la posibilidad de hacer una transformación en donde dinámicamente vas cambiando el tamaño de la cadena... ¿o sí? Ahora meto un poquito el dedo en la llaga con esto. En la configuración más sencilla, tú no sabes qué velocidad va a tener, lo que sabes es cuál es la máxima velocidad que va a tener el avión. Si sabes cuál es la máxima velocidad que va a tener el avión, ¿qué tendrías que hacer?

Cuatro caracteres por cada cadena y rellenarla de ceros. Es decir, si la velocidad del avión son veinte nudos, tendrías que mandar "0020" para llenar cuatro caracteres. Y entonces necesitarías cuatro bytes por cada muestra en lugar de dos, con lo cual sería exactamente el doble de almacenamiento. Esa sería la solución justa.

Ahora siendo un poquito más pillos, que de hecho os he puesto una pista con lo de la rutina de C. Yo puedo usar una cadena de longitud variable para reducir el almacenamiento. Imagina que en vez de dos minutos es todo el vuelo. Estamos hablando ya de una cantidad de memoria que merece la pena minimizar. O si la vas a tener que transmitir por vía inalámbrica. Esos canales de transmisión te interesa que sean la mínima cantidad de información posible para no saturarlos.

¿Podéis usar una cadena de caracteres de longitud variable aquí para reducir el número de bytes a transmitir de la velocidad?

Atención, no me vale "no". No son exámenes tipo test, por si los de primera matrícula todavía no habéis visto uno de nuestros exámenes: son preguntas conceptuales, no son preguntas tipo test de marcar con la X.

Vuelvo a repetir la pregunta: ¿puedo buscarme la vida para usar una cadena de longitud variable aquí para reducir el tamaño a transmitir?

- ¿Por qué piensas cabeceras?
- Porque la rutina C... ¿dónde hace el procesamiento para ver si estás al final de la cadena?
- Al final, ¿cómo?
- La terminación en cero.

¿Os acordáis lo que vimos en el manual del ASCII, del NUL, y de que la terminación en cero me permite saber en qué termina la cadena? Ya tenemos la solución.

Y fijaros que, al final, si has comprendido bien el concepto, la respuesta es sencillísima, se hace en dos líneas.

¿Cómo puedo conseguir una rutina que permita una longitud variable de cadena para reducir el almacenamiento? Lo único que tengo que hacer es convertir el valor de la muestra directamente a ASCII o UTF-8 con el número de caracteres que requiere el número de dígitos de la velocidad actual. Por ejemplo, si la velocidad actual es veinte, no necesito "0020". Yo puedo tener el "2" y el "0", y aquí el NUL, una terminación NUL.

Cuando mi rutina en C está procesando esto, el cierre procesa es la cola. C no es un lenguaje orientado a objetos y por tanto la cadena de caracteres no es un objeto, sino que es simplemente una secuencia de caracteres. ¿Cómo sabe la longitud? Viendo un terminador que te dice "hasta aquí". Usando la terminación en cero, yo puedo representar cada valor exactamente con el número de bytes que tiene.

Así que, ¿qué longitud va a tener la cadena exacta que tenéis que enviar con vuestras muestras?

Esa pregunta no tiene respuesta posible, porque depende de la secuencia que haya ido teniendo. Lo que sí puedes decir es cuál sería hipotéticamente la máxima longitud posible que te puedes encontrar. ¿Cuál sería la máxima longitud posible?

- Mil.

Suponiendo que el avión está volando durante dos minutos a velocidad máxima sostenida (mil), la cadena que recibiríais serían todos y cada una de las muestras con cuatro dígitos. Y por tanto, si dijimos que son cuatrocientos ochenta muestras por cuatro dígitos, cuatrocientos ochenta por cuatro, ya tenemos la longitud total de la cadena transmitida, ¿verdad? ¿No lo habéis entendido o porque me habéis visto la sonrisa?

- Yo tengo otra pregunta. Son cuatro números el mil, pero has dicho que le añades un NUL detrás.

¡Ah, eso es lo que estaba esperando, a ver si me habíais pillado! Ojo. Que le estáis metiendo un terminador. Por lo tanto no vale decir cuatro dígitos por el número de muestras. Porque en realidad son cinco caracteres por cada muestra. Si no contáis el terminador NUL, os quedáis cortos en el buffer de memoria y perdéis datos.

Hay que tener cuidado con estos detallitos, porque las cadenas más cortas nos sirven para reducir el número de bytes a transmitir, pero tengo que tener en cuenta el peor caso posible.

Y todavía hay algo que nadie me ha preguntado, ¿y cuál es la diferencia en tamaño de la cadena transmitida entre que use ASCII y use UTF-8?

Porque los números, los dígitos, pertenecen al ASCII de 7 bits. Los primeros ciento veintisiete caracteres del UTF-8 y ASCII son exactamente iguales, así que no hay ninguna diferencia en la codificación. Ninguna.

¿Por qué os pongo entonces las dos codificaciones? Para ver si alguien pica. Siempre hay el que pica. Si no recordáis el detallito de que UTF-8 y ASCII coinciden exactamente y por tanto en ambos casos tienen la misma longitud (un byte), os pondréis a decir que hay alguna diferencia. La respuesta correcta es que no hay ninguna diferencia.

Como veis, esto es lo que os va a dar de sí el binario natural, y en particular los problemas del binario natural. Trabajar con binario natural es trivial, lo que lo puede complicar un poquito es este tipo de preguntas. Pero esto son situaciones total y absolutamente normales en ingeniería. Tener que determinar primero rango de representación. Segundo, transformaciones de tipos, cuando tengo que pasar de valores en binario a valores de enlace.

¿Transmitir en ASCII es buena idea? O ¿qué podríamos hacer para todavía reducir más la longitud de bytes a transmitir?

Mandarlo en binario directamente. El caso que esto puede aparecer, es que hay veces donde no puedes transmitir directamente en binario natural. ¿Por qué? Porque a lo mejor hay una etapa por el medio que los valores de binario natural me los puede interpretar como caracteres de control. Los veinte primeros caracteres del ASCII son caracteres de control. Si tienes un sistema donde un preprocesador se empeña en mirar si son códigos de control, a lo mejor no te deja mandar en binario natural. Ya depende de la especificación del sistema.

Tenéis que mirar cuáles son las especificaciones de los sistemas y ver si el sistema soporta transmisión de ficheros binarios.

Os acordáis de los Macintosh de hace una década aproximadamente, que llevaban otros procesadores que no eran de Intel. Te podía ocurrir una cosa que se llama *endianness*. Little Endian, Big Endian. No lo voy a pedir para examen, pero es muy conveniente que lo miréis en la Wikipedia. Consiste simplemente, cuando tienes un valor que requiere múltiples bytes, en el orden en el que se tiene que escribir.

Suponed que el número que queréis representar requiere treinta y dos bits. En memoria hay dos formas posibles de representarlo. Básicamente, en memoria, los bytes están ordenados de forma distinta, de forma que el byte menos significativo...

En una de las representaciones el byte menos significativo es el byte en la posición más baja de memoria. Y en la otra representación es justamente lo contrario.

¿Qué maldad tiene esto? Que si pasáis un fichero binario de una máquina Little Endian a una máquina Big Endian, la habéis cagado porque se han dado la vuelta todos los valores. El valor al ser leído en un orden distinto, es totalmente distinto. Así que, en este caso, no se puede transmitir valores en binarios, y no queda más remedio que hacer las transformaciones en ASCII para pasarlo de su sistema a otro.

Esto son casos de la vida real, que han mordido a muchos ingenieros. Siguen existiendo microcontroladores que usan distintos tipos de *endianness*, entonces una de las cosas que tenéis que hacer al mirar las hojas de características con mucho cuidado es determinar si las representaciones son Big Endian o Little Endian, para evitar que os aparezca este tipo de problemas.

Esto es todo entonces lo que os va a dar de sí los enteros sin signo. Antes de que se me olvide, las prácticas. La que vais a hacer programando el PIC.

Una cosa con la que tenéis que tener exquisito cuidado, es que el PIC Simulator IDE, el lenguaje BASIC que vais a usar, os ofrece un tipo de datos Word de dieciséis bits, de ocho bits. Pero, ojo. En binario natural. Es decir, solamente números positivos. Sin signo. No usan números negativos, no tienen la representación para el complemento a dos.

Una vez que sois conscientes de que solamente se pueden guardar números positivos, no es demasiado problema. Pero tened algo de prudencia porque he visto en las prácticas compañeros vuestros tirarse literalmente de los pelos intentando depurar un programa que estaba bien escrito pero no le funcionaba por culpa de esto. El algoritmo consistía en algo como esto: `while i >= 0`, un bucle que va decrementándose. Inicializamos el `i` a diez.

Teniendo el bucle, este código es estrictamente válido. Cada iteración va a ir restando uno al `i` y va comprobando si ha terminado y sale del bucle. Como digo, es perfectamente lógico y válido y en este caso en particular se tiraba de los pelos porque no le funcionaba.

¿Cuál es el problema? El tipo entero del PIC en BASIC es binario natural. En binario natural cuando le restas uno a cero, ¿qué valor tienes?

Doscientos cincuenta y cinco. Una cosa que se llama desbordamiento. Tienes que tener en cuenta que si le restas uno a un cero, va a volver al valor máximo. ¿Y el doscientos cincuenta y cinco es mayor que cero? Sí. Entonces te quedas en un bucle infinito. La solución, si entendéis el concepto, es fácil y se hace cambiando el código un poco, trabajando un poco el valor mayor que cero y detectando un -1.

Y esa es la razón de que la definición de complemento a dos sea el complemento a uno más uno, para poder calcular el complemento a dos de un número negativo.