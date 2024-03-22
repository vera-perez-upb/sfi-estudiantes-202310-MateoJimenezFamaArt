## EJERCICIO 1

- ¿Por qué es importante considerar las propiedades *PortName* y *BaudRate*?
  
Es importante conciderar propiedades como PortName y BaudRate con el fin de que la comunicacion con la tarjeta se active correctamente,
si ingresamos un baud rate errado o el puerto equivocado la tarjeta no se comunicara de manera correcta con nuestro programa.

- ¿Quién debe comenzar primero la comunicación, el computador o el controlador? ¿Por qué?
El controlador debe comenzar primero la comunicación, ya que está esperando comandos del computador para responder.

- ¿Qué relación tienen las propiedades anteriores con el controlador?

  El port name es en que puerto estamos conectando nuestro microcontrolador, adicionalmente el baudrate nos inicializa
  la frequencia del monitor serial que se utilizaria, con el baud rate adecuado logramos comunicacion veloz y efectiva.

  ## EJERCICIO 2
En este programa tenemos una comunicacion rudimentaria donde uando desde el computador oprimimos la tecla A estamos obteniendo una comunicacion con un mensaje incompleto.

- ¿Funciona?
Sí, el programa funciona correctamente en términos generales, pero tiene un problema de impresion cuando intenta leer datos del puerto serial.

- ¿Qué pasaría si solo han llegado 10 de los 16 bytes del mensaje al momento de ejecutar la instrucción int numData = _serialPort.Read(buffer, 0, 20);?
En este caso la operacion read solamente leera los 10 bytes que hayan llegado y eso sera lo que se imprimira, dando asi un mensaje incompleto

-¿Cómo puede hacer tu programa para saber que ya tiene el mensaje completo?
Se podria implementar una logica la cual haga que al final de cada mensaje se ingrese un caracter especial el cual sea la "señal" para poder leer todo el mensaje.

-¿Cómo se podría garantizar que antes de hacer la operación Read tenga los 16 bytes listos para ser leídos?
Para que siempre hayan los 16 bytes se puede implementar logica la cual este revisando constantemenete si hay suficientes bytes disponibles para hacer el read.

-¿Cómo haces para saber que el mensaje enviado está completo o faltan bytes por recibir?
Esto se puede lograr anexando un caracter especial al final de cada mensaje, esto haria que sin importar la longitud del mensaje, el programa debera esperar a recibir ese caracter para poder imprimir, haciendo pues que se cumpla la necesidad de que el mensaje sea tan largo o corto como quiera puesto que la bandera que identifica sera el caracter especial.

- ¿Qué ocurre cuando se presiona la tecla A en la aplicación de Unity?
Se envía un comando al controlador para con el valor 1 para ser impreso en la consola.

- ¿Cómo se podría corregir el bloqueo de la aplicación al recibir datos del controlador?
Se podría revisar constantemente si hay datos disponibles antes de intentar leerlos.

- ¿Cómo podría el programa saber si ha recibido el mensaje completo del controlador?
Se podría establecer un carácter de fin de mensaje y esperar a recibirlo antes de procesar el mensaje completo.


## EJERCICIO 3

En este programa se mezclan las maquinas de estado con los scripts, basicamente tenemos dos maquinas de estados, una en el microcontrolador que tiene un INIT, WAIT_DATA, SEND_DATA. las cuales se encargan de inizializar, de esperar que llegue un Byte, De enviar un valor numerico al serial cada x segundos y si recive un 2 se devuelve a WAIT_INIT. Por otro lado en el script de unity se tiene una maquina de estados en la cual se manejan los estados del microcontrolador conformada igual por un INIT,WAIT_START, WAIT_EVENT. En estos tres estados se inicializad, se obtiene el key down de la tecla A y se envia un 1 al raspberry y se pasa al estado de wait events. Para ya finalmente esperar en el estado de WAIT_EVENTS para que si se preciona la B se devuelvan tanto el estado del micro al de WAIT_DATA y el estado del script a WAIT_START.


- ¿Se enfrenta el mismo problema de recepción de mensajes incompletos que en ejercicios anteriores?
Sí, se enfrenta el mismo problema ya que no se sabe cuándo el mensaje está completo.

- ¿Cómo se podría solucionar este problema?
Se podría establecer un protocolo de comunicación que garantice la longitud fija de los mensajes o tambien se podria implementar un caracter final como identificador del final del mensaje

## EJERCICIO 4

En esta iteracion del codigo se esta aplicando una logica la cual detecta si al final del mensaje hay un caracter especial el cual define que el mensaje se ha terminado y asi permite que el mensaje en cuestion que se envia llegue completo antes de que sea leido, sin embargo el codigo empleado habla ahora sobre leds y estados de esos leds, lo cual me parece particular pero al menos logro entender como se deberia implementar la logica para que los mensajes lleguen por completo a ser leidos del todo.


- ¿Cómo se podría mejorar el protocolo de comunicación para garantizar la integridad de los mensajes?
Se puede utilizar un carácter de fin de mensaje para indicar el final de cada mensaje.

- ¿Qué ventajas tiene este enfoque sobre los anteriores?
Permite una mejor detección del final de los mensajes y asegura que cada mensaje se procese completo.

## EJERCICIO 5

- No pude hacer la retro alimentacion con un compañero puesto que tuve que desatrazarme solo, sin embargo lo que abstraigo de las actividades es que teniamos un codigo para comunicarnos entre unity/micro/y pc. Con este codigo podiamos enviarnos mensajes entre nosotros pero estabamos enfrentandonos al problema de que los mensajes se leian de manera incompleta puesto que se enviaban a los bytes que fueran llegando, para resolver esto se concluyo que la mejor manera posible era agregando un caracter especial ('\n') el cual le permitiera al programa esperar a recibir ese caracter para ahi si poder leer el mensaje completo.

- Como observacion final me parece pertinente señalar que el hecho de que el codigo del ejercicio 4 sea completamente diferente a los de los ejercicios anteriores me parece algo medio malito puesto que estaba esperando poder ver como el codigo final quedaba sin tener que reiterar todo el codigo por mi cuenta para entender como funcionaria la verificacion de datos del caracter final.
