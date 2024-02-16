# Introduccion

#### Ejercicio 1

Para trackear movimiento de manera digital hay multiples opciones, la mas confiable es mediante trackers y lighthouses que basicamente funcionan como una maya de puntos en 3 dimensiones las cuales luego se distrocionan gracias a unos trackers que modifican esa maya de puntos
y reconstruyen la imagen de la persona en computadora. Otra forma de realizar ese tracking es con algo conocido como IMU, estos son basicamente microcontroladores que mediante accelerometros, giroscopios y kinematica inversa logran reconstruir la poscion de la persona
ofreciendo asi un estimado bastante fiel a la vida real de los movimientos.

#### Ejercicio 2

Los microcontroladores no solo se limitan a los raspberry, tambien hay otros como arduino, esp32 dev boards y muchos otros que traen diferentes capcacidades e inclusive traen diferentes perifericos intregrados como secciones especializadas en vision artificial o tarjetas de wifi integradas


#### Ejercicio 3

Los microcontroladores emplean lenguaje maquina de c++, inicialmente se programa para microcontroladores en base a que pin se utilziara, sea un pin digital o pin analogo. En el caso del raspberry pico solamente hay pines analogos, esto quiere decir que solo reciben 1 y 0.

```
void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
  static uint32_t previousTime = 0;
  static bool ledState = true;

  uint32_t currentTime = millis();

  if( (currentTime - previousTime) > 100){
    previousTime = currentTime;
    ledState = !ledState;
    digitalWrite(LED_BUILTIN, ledState);
  }
}
```

Este codigo permite que se incie un pin que sea un out put en el led que trae el pico integrado

Luego en el loop (El constante) se define una variable de el tiempo previo en 0 y se define un booleano sobre el estado del led (Prendido Apagado)

Luego la variable se sincroniza con la funcion millis que basicamente es un contador interno del raspberry

luego inicia un if en el cual si el tiempo actual - el tiempo previo que va sincronizado con la funcion millis luego compara que hayan concurrido 100 mili segundos, de ser asi entonces el tiempo previo se igualara al tiempo actual (Guardando valores previos)

El estado del led se invertira o sea que si estaba apagado se prende o si estaba prendido se apagaba.
Por ultimo hacia una escritura digital en el pin del LED_BUILTIN que es el led integrado y luego modificaba su estado a que estuviera prendido o apagado segun la variable del bool ledState.

#### Ejercicio 4

Aca se nos pide modificar el codigo en una parte especifica. En la linea 11 se pide modificar el 100 por un 1000 esto quiere decir que estamos cambiando el intervalo en que prende y apaga el led de 100 mili segundos a 1000 milisegundos significando que ahora el led hara blinks de 1 segundo de duracion

#### Ejercicio 5 

![image](https://github.com/vera-perez-upb/sfi-estudiantes-202310-MateoJimenezFamaArt/assets/117747851/46e1ee1d-1f1e-4bd6-935c-93ea866cbae3)

Segun lo leido tambien se puede compilar y subir cosas a un pico con consola de linux, ahora mas tarde revisare desde mi otra VMB. Esto se consigue del siguiente [Link](https://datasheets.raspberrypi.com/pico/getting-started-with-pico.pdf?_gl=1*5pnz8b*_ga*MTY0NzgzNjEwLjE3MDc1MDU3MzU.*_ga_22FD70LWDS*MTcwNzUwNTczOC4xLjEuMTcwNzUwNTc0OC4wLjAuMA..)



#### Ejercicio 6

Segun el codigo

```
`void task1()
{
    *// Definición de estados y variable de estado*` 

   `**enum** **class** **Task1States**    {
        INIT,
        WAIT_TIMEOUT
    };
    **static** Task1States task1State = Task1States::INIT;

    *// Definición de variables static (conservan*`    

*`// su valor entre llamadas a task1)*`    

**`static** uint32_t lastTime = 0;

    *// Constantes*`

    `**constexpr** uint32_t INTERVAL = 1000;

    *// MÁQUINA de ESTADOS*    **switch** (task1State)
    {
    **case** Task1States::INIT:
    {
        *// Acciones:*`  

      `Serial.begin(115200);

        *// Garantiza los valores iniciales*`        

*`// para el siguiente estado.*`       

 `lastTime = millis();
        task1State = Task1States::WAIT_TIMEOUT;

        Serial.print("Task1States::WAIT_TIMEOUT**\n**");

        **break**;
    }
    **case** Task1States::WAIT_TIMEOUT:
    {
        uint32_t currentTime = millis();

        *// Evento*`       

 `**if** ((currentTime - lastTime) >= INTERVAL)
        {
            *// Acciones:*`            

`lastTime = currentTime;
            Serial.print(currentTime);
            Serial.print('\n');
        }
        **break**;
    }
    **default**:
    {
        Serial.println("Error");
    }
    }
}

void setup()
{
    task1();
}

void loop()
{
    task1();
}`
```
¿Cómo se ejecuta este programa?

Se define una tarea en esta caso TASK1
Luego se definen los estados de este TASK1, El primero de estos es TaskState1 el cual lleva dos variables INIT y WAIT_TIMEOUTm Finalmente el TaskState1 se termina de definir como:  **static** Task1States task1State = Task1States::INIT; 
Esto significa que se genere un Task1States cuyo nombre es task1State y su valor sea estatico significando que no cambiara segun los estados que se apliquen

Pudiste ver este mensaje: `Serial.print("Task1States::WAIT_TIMEOUT\n");`. ¿Por qué crees que ocurre esto?
Este mensaje ocurre por que al final del task de "iniciar la carrera" como parte de las acciones del task te esta avisando que en efecto ahora el task1state va a ser el de WAIT_TIMEOUT por lo cual significa que ya comenzo a correr.

¿Cuántas veces se ejecuta el código en el case Task1States::INIT?
Se ejecuta una sola vez pues el task state :: init funciona como una especie de pistola de carreras, que al dispararse una vez pasa la maquina de esados al estado de waittimeout el cual basicmanete le esta diciendo: Todo listo?! 3,2,1, a correr!!! y pum. Salto al Timeout el cual comienza el reloj.


#### Ejercicio 7

En este ejercicio hacemos un analisis a profundidad del programa como tal. En base a esto mi hipotesis era correcta, el programa en cuestion no contaba con dos estados, si no que cuenta con un estado y un seudo estado. El estado como tal es el de WAITTIMEOUT el cual basicamente inicia a contar segun los milisegundos que vayan transcurriendo y el INIT es el que setea las condiciones iniciales, pero realmente es un pseudo estado ya que solo sirve de punto de partida.
Adicionalmente tambien podemos ver como nos explican las acciones que realiza el programa, esto funciona de la siguiente manera. En el segundo que se cumple el estado objetivo llamese como se llame, se ejecutara un bloque de codigo el cual nos dira ciertas instrucciones las cuales se ejecutaran siempre y cuando la condicion se este cumpliendo. Ya finalmente se analiza un evento dentro del estado de task WAITTIMEOUT el cual nos esta diciendo que se llamaran los milisegundos para que estos sean asignados a un valor fijo, luego de esto se ahce una operacion logica la cual, si se cumple entonces imprimira en el monitor serial de manera continua numeros en incrementos de milisegundos, de lo contrario batara un error.

#### Ejercicio 8

*Realiza un programa que envíe un mensaje al pasar un segundo, dos segundos y tres segundos. Luego de esto debe volver a comenzar.*

- ¿Cuáles son los estados del programa?
El programa tiene 3 tareas la cual independientemente poseen 1 pseudo estado y 1 estado cada una
  
- ¿Cuáles son los eventos?
Los eventos como tal que tiene cada tarea y cada estado de WAITTIMEOUT son de tomar los valores de las variables y comparar el reloj inicial con el reloj de cada tarea para poder imprimir cada momento. En lo que varia cada tarea es en el intervalo de espera para imprimir.
  
- ¿Cuáles son las acciones?
Las acciones son los respectivos prints de cada tarea :).

```
void task1(){
        enum class Task1State{
                INIT,
                WAIT_FOR_TIMEOUT
        };

        static Task1State t1s = Task1State::INIT;
        static uint32_t lastTime;
        static constexpr uint32_t INTERVAL = 1000;

        switch(t1s){
                case Task1State::INIT:{
                        Serial.begin(115200);
                        lastTime = millis();
                        t1s = Task1State::WAIT_FOR_TIMEOUT;
                        break;
                }
                case Task1State::WAIT_FOR_TIMEOUT:{
                        uint32_t currentTime = millis();
                        if ((currentTime - lastTime) >= INTERVAL){
                                lastTime = currentTime;
                                Serial.print("1");
                        }
                        break;
                        }
                default:{
                        break;
                        }
                }
}

void task2(){
        enum class Task2State{
                INIT,
                WAIT_FOR_TIMEOUT
        };

        static Task2State t2s = Task2State::INIT;
        static uint32_t lastTime;
        static constexpr uint32_t INTERVAL = 2000;

        switch(t2s){
                case Task2State::INIT:{
                        Serial.begin(115200);
                        lastTime = millis();
                        t2s = Task2State::WAIT_FOR_TIMEOUT;
                        break;
                }
                case Task2State::WAIT_FOR_TIMEOUT:{
                        uint32_t currentTime = millis();
                        if ((currentTime - lastTime) >= INTERVAL){
                                lastTime = currentTime;
                                Serial.print("2");
                                Serial.print("\n");
                        }
                        break;
                        }
                default:{
                        break;
                        }
                }
}

void task3(){
        enum class Task3State{
                INIT,
                WAIT_FOR_TIMEOUT
        };

        static Task3State t3s = Task3State::INIT;
        static uint32_t lastTime;
        static constexpr uint32_t INTERVAL = 3000;

        switch(t3s){
                case Task3State::INIT:{
                        Serial.begin(115200);
                        lastTime = millis();
                        t3s = Task3State::WAIT_FOR_TIMEOUT;
                        break;
                }
                case Task3State::WAIT_FOR_TIMEOUT:{
                        uint32_t currentTime = millis();
                        if ((currentTime - lastTime) >= INTERVAL){
                                lastTime = currentTime;
                                Serial.print("2 + 1");
                                Serial.print("\n");
                        }
                        break;
                        }
                default:{
                        break;
                        }
                }
}

void setup(){
  task1();
  task2();
  task3();
}

void loop(){
  task1();
  task2();
  task3();
}
```

#### Ejercicio 9

Quedo mezclado con el ejercicio 8 :)

#### Ejercicio 10
