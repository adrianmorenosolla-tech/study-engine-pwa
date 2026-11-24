Continuamos con el estudio de la adaptación de los periféricos. Ayer estuvimos viendo el hardware. Estuvimos viendo placas como las que vais a estar manejando en prácticas, la placa de verdad. En la placa aislada estuvimos viendo un periférico concretamente: el ventilador, y estuvimos viendo el hardware de adaptación.

  

Habíamos visto entonces lo que es el hardware. Tenemos el ventilador, que es el periférico, y hemos visto que necesitamos, en general y en la mayoría de los casos, un hardware de adaptación de tensiones y corrientes para poder controlarlo desde el sistema procesador. Directamente no podemos mover el periférico desde las líneas del microprocesador; ni por compatibilidad de tensiones, ni por compatibilidad de intensidades, ni por formatos distintos. No podemos mover el periférico desde las líneas del microcontrolador o microprocesador y necesitamos ese hardware de adaptación.

  

Vimos también cómo el hardware de adaptación incluía toda la circuitería necesaria para conectar la línea de salida (en este caso del microcontrolador) al circuito de adaptación (la base del transistor) para poder darle la corriente al ventilador.

  

Todo esto habíamos visto que en la práctica, o en la teoría, se traduce simplemente en que en nuestro diseño tenemos una parte que es transparente para el programador, que es el hardware de adaptación. Típicamente el fabricante ya se ha buscado la vida para darnos ese hardware de adaptación, y lo único que quiere el programador es tener una forma sencilla y cómoda de controlarlo desde el programa que está corriendo en el sistema procesador, es decir, en la CPU de mi sistema, de mi ordenador o de mi microcontrolador. Ese programa se va a encargar de gestionar el intercambio de señales con esta parte que le va a ordenar al periférico hacer cosas, o va a comprobar en qué estado está el periférico para saber si es posible hacerlas.

  

A ese software importante lo vamos a llamar el _driver_ del dispositivo (o _driver_ del periférico). A través de la interfaz se va a encargar de mover o intercambiar datos con el periférico. ¿Qué es lo que queremos? Que ese _driver_ del dispositivo tenga algo sencillito de manejar que le permita, de una forma abstracta, controlar el hardware de adaptación y el periférico. Ese elemento sencillo son los registros.

  

La interfaz del periférico, por tanto, va a estar compuesta del hardware de adaptación y un conjunto de registros, que es lo que va a ser visible desde el sistema procesador y a través del cual vamos a poder controlar el periférico.

  

Antes de entrar en más detalles de esos registros y de cómo llegar a ellos, aclaremos un par de cosas de nomenclatura:

  

1. El _driver_ del periférico.
    
      
    
2. Las direcciones van a tener nombres y apellidos. Concretamente, el sentido que se toma como referencia para determinar en qué dirección están pasando los datos es CPU-céntrico. Es decir, tomamos como referencia del sistema el procesador, dondequiera que esté la CPU que está ejecutando el software que, a través de esos registros, controla el periférico.
    
      
    

Si el acceso a los registros del periférico es de entrada, los datos están pasando desde el periférico al procesador. Por lo tanto, una entrada (lectura o _read_) es una operación en donde los datos van del periférico hacia el procesador. Mientras que una escritura (_write_) es una operación de salida en la que los datos van desde la CPU hacia el periférico. Esta es la referencia de sentido que tomaremos para decir si una transacción que estamos haciendo desde el _driver_ hacia el periférico es de lectura o de escritura: la referencia siempre va a ser el sistema procesador.

  

Una vez que tenemos eso claro, esto es lo que va a ver el programador del _driver_ del dispositivo: una serie de registros donde cada registro va a ser una caja en donde podemos almacenar un cierto número de bits. Típicamente coincide con la palabra, o con un número de bytes igual a la palabra que tenga el procesador (suelen ser 1, 2, 4 u 8 bytes lo que tenemos en el registro).

  

Los registros van a ser los mecanismos a través de los cuales, mediante la escritura o lectura de bits o campos de bits, vamos a poder saber cómo está el periférico, qué está haciendo, ordenarle acciones, o leer y escribir los datos que se estén generando como consecuencia de la transacción con el periférico. De forma que vamos a tener uno o varios registros de estado, uno o varios registros de control y, potencialmente, registros de entrada y de salida de datos. Esta es la forma más general de los registros de una interfaz.

  

Para entender mejor cuál es el papel de estos registros de estado y de control, volvemos a nuestro ejemplo del ventilador. Recordad que teníamos una línea que poníamos a 1. Esa línea viene de una de las que conectamos al microcontrolador. Concretamente, en la documentación que tenéis en el Campus Virtual se describe que el ventilador está conectado a la tercera línea (recordad que empieza en cero: 0, 1, 2, la línea 2) del puerto C del microcontrolador.

  

Aquí os presento este documento que en el Campus Virtual podéis encontrar en las prácticas. En la documentación de la placa PICgenios V3, vais a encontrar este documento. Es lo primero que tendríais que leer antes de siquiera empezar a programar con la placa. Os describe con detalle qué periféricos tenéis y, sobre todo, para cada periférico individual (por ejemplo, la botonera o los LEDs) cuáles son las líneas a las que está conectado y sus características eléctricas.

  

Estas características eléctricas me dicen que los pulsadores ponen a tierra (es decir, ponen un cero) la línea correspondiente del puerto B del microcontrolador cuando los pulsamos. Y nos indica que esas líneas tienen una resistencia de _pull-up_. En electrónica, una resistencia de _pull-up_ se pone para garantizar que una línea que por defecto no tiene un dispositivo que le dé valores, tenga un valor definido a 1. Fijaros que aquí tengo VCC (la alimentación de 5 voltios). ¿Cómo puedo garantizar que, si el microcontrolador en la salida RB1 no le está dando un valor cero mediante la pulsación del botón, esta línea esté a uno? Poniendo la línea conectada a la alimentación a través de esta resistencia.

  

¿Qué pinta esta resistencia aquí? Si no existiera esta resistencia de _pull-up_ y conecto directamente la alimentación a la línea, en el momento que ponga un cero al pulsar el botón, tendré un cortocircuito. La corriente se dispara y voy a quemar la fuente de alimentación, el microcontrolador o algún componente. ¿Cómo conseguir que no eche humo? Pongo una resistencia de por medio que limita la corriente. Esa resistencia va a ser muy grande para que la corriente al pulsar el botón sea pequeña, pero lo suficiente para proteger contra cortocircuitos y, al mismo tiempo, permitir que cuando nadie le da valor a la línea, tengamos los 5 voltios (un 1 lógico). Ese es el papel de la resistencia de _pull-up_. Una resistencia de _pull-down_ hace exactamente lo mismo, pero puesta a cero (a tierra) en lugar de a VCC.

  

Este documento os muestra detalles que os permiten saber cómo programar los botones. Lo primero que tenéis que preguntaros al leer la línea RB1 es: cuando pulse el botón, ¿qué forma de onda voy a tener? Esta es la pregunta que inmediatamente se hace el programador del _driver_, porque tiene implicaciones no triviales. La implicación más gorda es el flanco. No es lo mismo buscar un flanco de subida que uno de bajada. El flanco de subida lo vamos a encontrar cuando la línea pase de cero a uno; lo encontraremos cuando empiece la pulsación o cuando se suelte, dependiendo del diseño. En un teclado siempre hay dos eventos: pulsar tecla y soltar tecla, y los dos son distintos. Desde el hardware se identifican de forma distinta y, dependiendo de lo que queráis hacer, decidís cuál usar. Para eso está la documentación: os dirá que la línea RB1 por defecto está a 1, y cuando pulsas baja a 0.

  

Esto también tiene implicaciones no triviales al manejar interrupciones. Las interrupciones de la línea RB0 (que es la que dispara la interrupción externa) se disparan por flancos (de subida o de bajada). Si en el pulsador primero está el flanco de bajada y luego el de subida, la interrupción se disparará al inicio de la pulsación o al soltar el botón, según cómo la configuréis.

  

También es fundamental leer la documentación para el _display_ de siete segmentos multiplexado. Si tenéis que mostrar un número de 4 dígitos, como el 1024, ¿cuál es el problema? Las líneas de datos de 0 a 7 son un bus compartido. Es un puerto de ocho bits que sirve como salida. El problema es que los cuatro _displays_ leen las mismas líneas al mismo tiempo. Si los cuatro están encendidos a la vez, mostrarán exactamente el mismo número.

  

Fijaros que este transistor actúa como un interruptor digital. Al activar esta línea, cerramos el circuito a tierra y se enciende el _display_. Cuando pongo un 1 lógico en esta línea de control (líneas DIS1 a DIS4 del puerto A), enciendo el _display_ correspondiente. Por tanto, puedo controlar el encendido individual de cada uno. Para mostrar cuatro números distintos, encendemos un _display_, escribimos su valor, lo apagamos, pasamos al siguiente, lo encendemos y escribimos. Esto requiere hacer barridos periódicos a una velocidad lo suficientemente alta para engañar a la retina y ver los cuatro números a la vez. Si leéis la documentación, entenderéis perfectamente cómo funciona; si no la leéis, os pondréis a programar a lo loco y tardaréis el doble.

  

En cuanto a los botones, ¿cómo sabéis cuál se ha pulsado? Cada botón está conectado de forma única a una línea distinta del puerto B. El programador tiene que leer el valor de ese botón en el registro PORTB. Leyendo este registro de ocho bits y enmascarando, obtenéis el estado del pulsador correspondiente. Este es un ejemplo de registro de estado: un registro de solo lectura que proporciona información sobre el periférico.

  

¿Y el registro de control? Es un registro de escritura donde configuro el periférico u ordeno acciones. Para garantizar que el puerto B actúe como salida, usamos el registro TRISB. Escribiendo un cero en el bit correspondiente del TRISB, configuramos la línea como salida. Escribiendo un uno, la configuramos como entrada. El registro TRISB es el que configura, y el registro PORTB es el registro de estado donde leemos o escribimos la información.

  

Todo este conjunto define el modelo de programación del periférico. Mientras este modelo de registros se mantenga, el _driver_ seguirá funcionando correctamente aunque se realicen cambios en el hardware subyacente. Para acceder a estos registros mapeados en memoria, la CPU utilizará operaciones estándar de lectura y escritura. Dependiendo de la arquitectura subyacente, esto puede verse implementado a nivel de instrucciones de máquina, tal como sucede en código ensamblador MIPS o RISC-V, o empleando mecanismos de acceso directo a memoria (DMA) si el periférico o el microcontrolador PIC 16F877A así lo requiere para transferencias complejas.