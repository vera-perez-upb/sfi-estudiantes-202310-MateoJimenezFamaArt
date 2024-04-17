# Trayecto de Actividades 3

## Ejercicio 1

Principal objetivo de esta actividad es identificar **¿Cómo se ve un protocolo binario?**

Las preguntas guia que utilizaremos para este ejercicio son:

- **¿Cómo se ve un protocolo binario?**

Se ve como una cadena de 1s y 0s los cuales empaquetan informacion y mantienen cierta informacion y comandos los cuales luego se traducen de lenguaje maquina a un cierto input deseado

- **¿Puedes describir las partes de un mensaje?**

Las partes de un mensaje son:

Len -> Longitud del bloque de datos. Va de 4 a 96 bytes para bloques de datos de comandos y de 5 a 101 se usan bytes de respuesta a esa data

Adr -> Direccion de a donde se hace el broadcast de la informacion

Cmd -> Representa el comando que se debe ejecutar por el lector

Data[] -> Contiene las variables que se requieren para ejecutar el comando en cuestion

LSB-CRC16 -> Hace los respectivos chequeos para verificar si hay algun error en la comunicacion, esto lo hace realizando un CRC {cyclic redundancy check} en todo el mensaje. Este check esta dividido en el LSB y el MSB que son Least Significant Byte y Most Significant Byte respectivamente. 

- **¿Para qué sirve cada parte del mensaje?**

Len -> Sirve para poner de parametro de que tan largo es el mensaje a leer y apoya al lector a determinar los limites del mensaje en cuestion

Adr -> Sirve para identificar quien recibira el bloque entero de mensajeria y tambien determina si solo lo recibe o si deberia responder algo

Cmd -> Sirve para definir la accion en cuestion que el lector debera de realizar

Data[] -> Sirve para contener la informacion necesaria por el lector para ejecutar las acciones en cuestion que requieran ciertos valores 

LSB-CRC16 -> Sirve para determinar la integridad de los datos enviados y nos muestra si el mensaje esta siendo recibido correctamente
