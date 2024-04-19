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

LSB-CRC16 (CheckSum) -> Hace los respectivos chequeos para verificar si hay algun error en la comunicacion, esto lo hace realizando un CRC {cyclic redundancy check} en todo el mensaje. Este check esta dividido en el LSB y el MSB que son Least Significant Byte y Most Significant Byte respectivamente. 

- **¿Para qué sirve cada parte del mensaje?**

Len -> Sirve para poner de parametro de que tan largo es el mensaje a leer y apoya al lector a determinar los limites del mensaje en cuestion

Adr -> Sirve para identificar quien recibira el bloque entero de mensajeria y tambien determina si solo lo recibe o si deberia responder algo

Cmd -> Sirve para definir la accion en cuestion que el lector debera de realizar

Data[] -> Sirve para contener la informacion necesaria por el lector para ejecutar las acciones en cuestion que requieran ciertos valores 

LSB-CRC16 (CheckSum) -> Principal objetivo de esta actividad es identificar la naturaleza sobre la comunicacion binaria y entender que funciones de comunicacion serial se emplean y la excepcion de el caracter de fin de mensaje "\n".

Se sabe que empleamos las siguientes funciones para comunicarse y establecer usabilidades seriales:

- Serial.available() -> Get the number of bytes (characters) available for reading from the serial port. This is data that’s already arrived and stored in the serial receive buffer (which holds 64 bytes).
  
- Serial.read() -> Reads incoming serial data.
  
- Serial.readBytes(buffer, length) -> `Serial.readBytes()` reads characters from the serial port into a buffer. The function terminates if the determined length has been read. Serial.readBytes() returns the number of characters placed in the buffer. A 0 means no valid data was found.
  
- Serial.write() -> Writes binary data to the serial port. This data is sent as a byte or series of bytes; to send the characters representing the digits of a number use the print() function instead.

  Todas estas funciones permiten comunicacion serial al leer, mirar disponibles, leer los bytes en el buffer con tanta longitud y escribir de manera serial algo, es importante notar que la funcion Serial.readBytesUntil() ya no nos conviene puesto que esta es mejor utilizarla en un protocolo de comunciacion ASCII ya que es mas apto para manejar \n, para comunicacion binaria es mejor emplear otras funciones de lectura y escritura como las que se mostraron mas arriba.

  ## Ejercicio 3

  El principal objetivo de esta actividad es identificar que es el endian?

  En la comunicacion binaria se tiene que en casos se deben enviar variables cuyos datos son mayores a un byte, esto conlleva a que al diseñar debemos tener en cuenta el orden en que los bytes iran entrando a nuestro buffer (Por ejemplo cuando usamos un punto flotante este se representa en 4 bytes).  Asi pues a la hora de escribir se debe trbajar un una de dos opciones de lecturas:

   -  Transmitir primero el byte de menor peso (little endian)
   -  Transmitir primero el byte de mayor peso (big endian)

   Esto es importante identificarlo a la hora de diseñar un programa puesto que nos implica que desde un principio se debe tener claro que posibilidad se eligira para protocolos binarios

  ## Ejercicio 4

  El principal objetivo de esta actividad es identificar varios elementos de un protocolo de comunicacion binario?

  Se tienen estos dos codigos

Guardando el valor a ser leido solamente en una variable flotante
```
  void setup() {
    Serial.begin(115200);
}

void loop() {
    // 45 60 55 d5
    // https://www.h-schmidt.net/FloatConverter/IEEE754.html
    static 
    float num = 3589.3645;
    if(Serial.available()){
        if(Serial.read() == 's'){
            Serial.write ( (uint8_t *) &num,4);
        }
    }
}
```

Guardando el valor a ser leido en un array de 4 espacios

```
void setup() {
    Serial.begin(115200);
}

void loop() {
// 45 60 55 d5
// https://www.h-schmidt.net/FloatConverter/IEEE754.htmlstatic
float num = 3589.3645;
static uint8_t arr[4] = {0};

if(Serial.available()){
if(Serial.read() == 's'){
            memcpy(arr,(uint8_t *)&num,4);
            Serial.write(arr,4);
        }
    }
}
```

  - ¿En qué *endian* estamos transmitiendo el número?

  En ambos casos estamos transmitiendo el little endian puesto que en la secuencia del mensaje se esta enviando : (45,60,55,d5) y la forma en que se esta escribiendo este mensaje es en d5, 55, 60,45 aca se evidencia que el little endian o sea el ultimo byte del mensaje es el que se esta leyendo primero, por ende estamos en ambos codigos utilziando comunicacion con el little endian.
    
- Y si queremos transmitir en el *endian* contrario, ¿Cómo se modifica el código?

  Para transmitir el big endian tendriamos que albergar todo en un array para poder recorrer cada parte del mensaje y luego implementar un ciclo for que vaya recorriendo el array en el orden deseado para que se muestre desde el big endian o el primer byte hasta el ultimo

```
  void setup() {
    Serial.begin(115200);
}

void loop() {
// 45 60 55 d5
// https://www.h-schmidt.net/FloatConverter/IEEE754.htmlstatic
float num = 3589.3645;
static uint8_t arr[4] = {0};

if(Serial.available()){
if(Serial.read() == 's'){
            memcpy(arr,(uint8_t *)&num,4);
for(int8_t i = 3; i >= 0; i--){
              Serial.write(arr[i]);
            }
        }
    }
}
```

Finalmente aca se ve como al implementar el float en un array y lueog interpretarlo inversamente desde un ciclo for podemos interpretar desde codigo el lenguaje como si fuera una comunicacion con big endian.

  
