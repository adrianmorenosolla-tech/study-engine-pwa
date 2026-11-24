**[00:00]**

Hacer la elección de los representantes del Consejo de Departamento de Arquitectura de Computadores. La versión corta, si alguno de vosotros quiere presentarse, es un proceso en dos fases. En una primera fase se elige por cada asignatura y por cada grupo un representante. En una segunda fase hay una reunión en donde todos los alumnos que han sido elegidos en distintas titulaciones se reúnen para elegir los representantes que, de acuerdo con el número de alumnos que pueden estar en el Consejo de Departamento, hagan la representación durante el curso académico. Por lo tanto, no significa que el que salga ahora necesariamente va a salir luego en la reunión, sino que depende de los cupos. 

Hay un papel de convocatoria que lleva en las puertas desde hace semana y media o dos semanas. Ahí está la convocatoria. Nadie lo mira, pero está. Se mandó del departamento a los centros en hojas para que los pegaran en las puertas de las aulas donde tenemos docencia. Llevaba convocada ahí por escrito desde el plazo legal, que era una semana o semana y media, por escrito. ¿Alguien quiere ser candidato a representante del departamento? 

Una pregunta, ¿esto no es lo mismo que sea el representante de la asignatura? Sí. Lo que se suele hacer es que el representante elegido de la asignatura es el que previamente lleva los contactos con el profesor de la asignatura. Y aparte, ese representante, además, es elegido como representante en el Consejo de Departamento. De hecho, en el Consejo de Departamento, más que a la asignatura, a lo que está representando es a la titulación; como alumno de Ingeniería Industrial, no tanto al alumno de Fundamentos de Computadores. 

Otra cosa es que, como no vamos a esperar a las elecciones, que son en noviembre, a elegir el representante para que haya alguien que oficialmente haga el contacto con el profesor para todo lo que habéis estado haciendo hasta ahora, no tiene mucho sentido. Así que el representante de vosotros que ha estado viniendo a decirme si se puede poner una fecha de examen o así, que está viniendo sin problema, por mí no hay ningún problema. No me habéis puesto ninguna pega, no me habéis propuesto nada distinto, así que, como todo es razonable, me parece perfectamente razonable. 

Volviendo al tema, ¿hay alguien que se presenta como candidato? Si no, no pasa nada, supongo que no hay candidato y entrego los resultados. Las reuniones de departamento son rapidísimas y aburridas porque nunca pasa nada. Hay otros departamentos que sí hay jaleo y la sangre llega al río, pero en nuestro departamento las reuniones son muy aburridas y no hay nada destacable, así que tampoco os perdéis gran cosa. 

Otra historia es que tengáis en un momento determinado una necesidad administrativa con el departamento, aparte de los cauces oficiales vía el profesor. Siempre podéis contactar con alguno de los representantes de alumnos que estén elegidos para el Consejo de Departamento y decirle lo que necesitáis. Pero como normalmente vuestras necesidades son muy puntuales y afectan a la asignatura más que a la titulación, lo normal es hacer el contacto con el profesor y cualquier cosa que necesitéis, la comentáis. 

**[05:00]**

Todo lo del examen del día 24 cae hasta donde entremos hoy. Mañana no hay clase. De acuerdo al calendario docente, no debo dar todas las clases. Algunos de los grupos grandes me los tengo que saltar para no sobrepasar las 60 horas que legalmente debo dar. Mañana es un buen día para hacerlo, porque para qué vamos a estar por aquí. Por si acaso, en la primera sesión de la tarde también aparecen aquí los de Industriales, aprovechamos mañana, cumplo con el hecho de que no debo dar todas las clases de grupo grande y nos lo quitamos de encima. 

Continuaremos hoy por lo que nos hemos perdido al principio con las elecciones. Llegaremos justo, no sé si llegaré ya a todas las interrupciones, pero casi casi. La semana que viene terminamos esto y será lo último que entre. Todos los temas de teoría que hemos visto hasta ahora entran en el examen, incluyendo los problemas de conversión de representación de la información de números enteros y el código Hamming. No habrá en el examen escrito problemas de conversión de coma flotante a decimal. Entrará hasta lo que veamos del tema de interrupciones.

Sin más, vamos a ver qué problema es el que vamos a resolver con el uso de interrupciones. El problema es el siguiente: todos los diseños que hemos hecho hasta ahora en este tema de teoría, acerca de cómo se conecta físicamente la unidad de procesamiento (el sistema procesador: CPU y memoria) con la interfaz del periférico, es lo que ve el programador del periférico. Porque el hardware real no lo ve, lo que vemos son registros, la conexión a través de los buses. 

En este esquema básico, la única forma que tenemos de enterarnos de que el periférico tiene necesidad de que la CPU entre en la rutina de servicio del periférico (es decir, entre en el código máquina que realiza las transferencias de datos entre registros de la interfaz y memoria) es ninguna. El periférico no tiene la posibilidad de decirle nada a la CPU. Es la CPU la única que, de forma voluntaria, mediante el código que está ejecutando, consulta a ver si el periférico tiene algo que necesita transferir. 

Aquí es donde habíamos visto la consulta de estado. Esto es lo que llamamos *polling* o muestreo. La técnica de muestreo significa que la CPU periódicamente tiene que mirar si el periférico necesita atención. Ya vimos que había dos posibilidades: que me quede mirando continuamente si el periférico requiere atención, o que mire de vez en cuando. Mirar constantemente solo lo hago si sé que el periférico está a punto de necesitar atención. En ese caso es E/S programada (*programmed I/O*). Lo habitual es que la CPU de vez en cuando eche un vistazo al registro de estado a ver si el periférico necesita atención. 

Esta técnica de muestreo periódico tiene un problema gordo. Entre el momento en que el periférico necesita atención (porque ha puesto datos en el registro o necesita sacarlos) y el momento en que la CPU se da cuenta de que necesita atención mediante una de esas consultas periódicas, pasa un tiempo que es difícil de predecir. Cuidado, no es totalmente impredecible. Si hago un sistema de control en tiempo real y programo código máquina directamente, puedo contar cuántas instrucciones voy a tener en el bucle y saber exactamente cuánto tiempo pasa entre las lecturas. 

**[10:00]**

Pero va a haber un tiempo que, en un sistema normal, no tengo la posibilidad de contabilizar instrucción a instrucción cuánto va a tardar una iteración entera. O, peor todavía, el tiempo que tarde dependerá de lo que pase en el sistema. Por ejemplo, si el usuario pulsa un botón, se introducen refrescos de pantalla, lo que añade tiempos de latencia y, por lo tanto, el tiempo que va a tardar entre evento y evento cambia. 

Como este tiempo puede ser impredecible o largo, la solución es que el periférico sea capaz de avisar explícitamente a la CPU de que necesita atención. ¿Cómo? Con una línea dedicada llamada línea de interrupción, activada por la interfaz para decirle al procesador: "el periférico al que estoy conectado necesita que entres en la rutina de servicio". Esto soluciona el problema de la temporización; tan pronto la interfaz activa físicamente la línea de interrupción, hace que la CPU, a la mayor brevedad posible, entre en la rutina de servicio. 

Tenemos dos problemas que resolver: determinar cuál es el procedimiento que debe seguir la CPU para atender esa petición de interrupción, y determinar cuál es la rutina de servicio que debe ejecutar. En un sistema medianamente complejo, voy a tener en memoria distintas rutinas de servicio de distintos periféricos. Por ejemplo, tengo cuatro rutinas distintas. Yo necesito, si voy a atender al periférico Z, que la CPU ejecute ese código específico. Para que ejecute este código, tengo que conseguir que el mecanismo de atención automatizada de la CPU sepa que el contador de programa tiene que saltar a esta dirección de inicio de la rutina de servicio. 

Ahora veremos cómo determinar el origen de la petición de interrupción. Aquí tenemos el esquema de un ciclo de instrucción en una máquina Von Neumann. Utiliza el contador de programa (PC) para saber exactamente dónde está ejecutando el programa, es decir, qué instrucción debe leer en memoria para decodificarla y, a través de la unidad de control, determinar qué instrucción es y con qué operandos operar. 

Durante la ejecución, el contador de programa está apuntando a la instrucción que estamos ejecutando. Típicamente, a estas alturas ya se ha sumado el número de bytes que compone la instrucción para apuntar a la siguiente. Entretanto, tenemos una serie de registros internos y circuitería que está haciendo la ejecución (el registro de instrucción, el decodificador, la ruta de datos). 

¿Os gustaría tener que diseñar un procesador en donde, en mitad de la ejecución de una instrucción, se tuviera que guardar todo este estado interno para saltar a otro lado y atender la interrupción? Debería sonar feo, porque tiene que guardar un montón de información de estado, con muchas posibilidades de que algo falle. 

**[15:00]**

Efectivamente, no es una buena idea intentar guardar el estado del procesador en ese momento. ¿Cuándo es el momento más sencillo para guardar el estado del procesador? Justo al inicio del ciclo de instrucción, cuando lo único que tienen que guardar es el contador de programa, porque todavía ni siquiera han leído la nueva instrucción. Es el contador de programa que apunta a la próxima instrucción que vas a ejecutar. Punto. No hay más que guardar. Ese es todo el contexto del programa. 

Todo lo demás, todos los registros internos, es prescindible. Lo crítico es el contador de programa. Así que el momento más adecuado para parar la ejecución del programa actual e irte a ejecutar la rutina de servicio es cuando vas a empezar una nueva instrucción. Lo único que hay que hacer es salvar el valor del contador de programa en la memoria, típicamente en la pila, y cargar en el contador de programa la dirección de inicio de la rutina de servicio del periférico. 

Listo, ya está. Con eso has cambiado el contexto. La figura nos muestra que no cambiamos el estado del procesador en el momento en que se activa la línea de interrupción, porque en ese momento el procesador está ejecutando una instrucción y su estado es muy complejo. Espera a terminar de ejecutar la instrucción en curso. Una vez que termina, el contexto es sencillito: el contador de programa apuntando a la próxima instrucción, y lo salvamos en memoria. 

Luego determinas qué periférico necesita atención, sacas la dirección de inicio de su rutina y la cargas en el contador de programa. Empieza a ejecutar ese código. Cuando termina la última instrucción de la rutina de servicio, vuelves exactamente al punto de donde habías partido. Recuperas el contexto (el valor del PC que guardaste) y continúas la ejecución del programa normal como si no hubiera pasado nada. 

Esta es la razón por la que las interrupciones hay que tratarlas con respeto. Os pongo el ejemplo de un Trabajo de Fin de Grado (TFG) de un compañero para controlar un péndulo invertido usando Arduino. Para estabilizar el péndulo, el algoritmo de control debía ser muy rápido y requerir muchas muestras por segundo. Usaba interrupciones generadas por un temporizador. 

¿Cuál es el problema de las interrupciones con la necesidad de salvar y recuperar el contexto? Que si el número de interrupciones por segundo es excesivamente grande, os tiráis más tiempo escribiendo y leyendo de memoria por los cambios de contexto que procesando. Literalmente, inutilizáis el procesador. 

Si tenéis decenas de miles de interrupciones por segundo, los cambios de contexto os matan. Pensad seriamente en usar *polling*. El *polling* requiere consultar el estado mediante el procesador, pero tiene la ventaja de que no gastáis el tiempo de salvar y recuperar el contexto. En el TFG, el alumno consiguió resultados correctos cuando dejó de usar interrupciones y empezó a usar *polling*, porque con las interrupciones el control llegaba tarde, se desestabilizaba y el péndulo se caía. 

**[20:00]**

Este mecanismo de interrupciones funciona, pero tiene un problema: la interrupción interrumpida. En la fase donde el procesador empieza a ejecutar la primera instrucción de la rutina de servicio, ¿qué está viendo? Está viendo que la línea de petición de interrupción sigue activada. El procesador interpreta que el periférico sigue pidiendo atención. 

¿Qué pasará? Al terminar la primera instrucción de la rutina de servicio, volverá a salvar el contexto (que ahora apunta a la segunda instrucción de la rutina) y saltará de nuevo al inicio de la rutina de servicio. Cuando atienda al periférico y la línea por fin se desactive, al volver, recuperará el PC apuntando a la segunda instrucción y seguirá ejecutando. 

¿Qué pasa si vuelves a entrar en la rutina de servicio cuando el periférico ya fue atendido? Tienes un problema grave. Si el diseñador del periférico o el programador del *driver* no tuvo en cuenta estas interrupciones espurias, el *driver* pierde el control. Puede leer un registro de datos cuando no hay datos. Se desestabiliza el hardware del sistema y ocurre el famoso pantallazo azul de la muerte (BSOD) en Windows, o un *Kernel Panic* en Linux, que tumba el ordenador para evitar corromper los datos del disco duro. 

Para evitar esto, la solución es sencilla y se hace por hardware. La unidad de control deshabilita las interrupciones globales cuando empieza a procesar una interrupción (al salvar el contexto) y las vuelve a habilitar cuando recupera el contexto. Así evitamos el problema, ya que, aunque la línea siga activa, no volveremos a ser interrumpidos hasta terminar la atención actual. 

Todos los procesadores lo hacen, incluyendo el PIC16F877A. En el PIC, el registro de control de interrupciones (INTCON) tiene un bit que es la máscara global de interrupciones (GIE - *Global Interrupt Enable*). El programador lo pone a 1 para habilitarlas. Cuando la unidad de control entra a procesar una petición, automáticamente pone a 0 este bit, deshabilitando interrupciones, y cuando sale de la rutina de servicio, el procesador vuelve a ponerlo a 1. Si usáis el PIC Simulator IDE, podéis ver cómo el bit GIE cambia automáticamente sin que programéis nada. 

El siguiente problema a resolver es determinar qué periférico ha pedido interrupción y, por tanto, la dirección de inicio de su rutina. Se hace mediante lo que se llama un vector de interrupción, que es un número entero. En los micros pequeños y durante mucho tiempo en los ordenadores antiguos, el vector era un valor de 8 bits (de 0 a 255) que se usa como índice. 

Tenemos en memoria una tabla de vectores de interrupción (tabla de direcciones). El sistema operativo carga en esta tabla las direcciones de inicio de las rutinas. Si el periférico genera el vector 2, el procesador va a la posición 2 de la tabla y carga en el contador de programa la dirección almacenada ahí. 

**[25:00]**

Hoy en día, si miráis los recursos de un dispositivo en Windows bajo un controlador moderno como PCI Express, el vector de interrupción ya es de 32 bits (ocho dígitos hexadecimales). Los procesadores modernos tienen múltiples tablas de vectores de interrupción dependiendo del nivel de privilegio (usuario, supervisor, etc.). 

¿Cómo se generan estos vectores de interrupción? Los puede entregar el hardware (un controlador de interrupciones) o los genera el propio procesador internamente. El PIC16F877A que vais a usar en las prácticas tiene un sistema de interrupciones muy sencillito: hay un único vector de interrupción en la dirección de memoria `0x0004`. 

Cuando se produce una petición de interrupción, el procesador automáticamente salta a la dirección `0x0004` y empieza a ejecutar el código que haya allí. Por eso, en el código ensamblador (el fichero LST/ASM) que usamos en las prácticas veréis la directiva `ORG 0x0004` (*Origin* 4). Esa directiva le dice al ensamblador que las instrucciones siguientes se coloquen en esa dirección específica de memoria. 

En la dirección 4 os encontraréis cuatro instrucciones que sirven para salvar el contexto y después un salto (un `GOTO` a una etiqueta como `on_interrupt`) donde se encuentra el código de vuestra rutina real. Esas primeras instrucciones manuales en ensamblador lo que hacen es guardar registros como `W` o `STATUS` (por ejemplo, con `movwf STATUS_TEMP`), ya que el hardware del PIC solo guarda el contador de programa. 

Al terminar vuestra rutina, antes de volver, tendréis instrucciones para recuperar el contexto (devolver los valores originales a `STATUS` y `W`) y finalizar con una instrucción especial llamada `RETFIE` (*Return from Interrupt Enable*). Esta instrucción recupera el contador de programa de la pila y, automáticamente, vuelve a poner a 1 el bit global de interrupciones (GIE). 

Aquí es donde comprendéis por qué un número masivo de interrupciones mata el rendimiento: todo este tiempo que se gasta saltando, deshabilitando, salvando registros en memoria manualmente, recuperándolos y haciendo el retorno, son ciclos de reloj que el procesador no está usando para procesar información útil. 

Si tenéis pocas interrupciones, este mecanismo es ideal. Si el volumen es altísimo, el *polling* empieza a ser la opción viable. ¿Alguna duda, preguntas, cuestiones? Nos vemos el viernes.