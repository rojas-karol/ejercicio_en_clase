# Informe de Laboratorio - Sistemas Digitales con Arduino
**Presentado por:** Kevin Alejandro Tacha Herrera & Karol Vanessa Rojas Gil  
**Herramienta de simulación:** [Tinkercad](https://www.tinkercad.com)
---

## Proyecto 1 - Compuertas Lógicas con pulsadores

### Objetivo
Simular el comportamiento de las compuertas lógicas AND, OR y NOT usando un Arduino Uno, dos pulsadores como entradas y tres LEDs como salidas visuales.

### Materiales

| Componente | Cantidad |
|---|---|
| Arduino Uno | 1 |
| Pulsador (push button) | 2 |
| LED rojo (OR) | 1 |
| LED verde (AND) | 1 |
| LED amarillo (NOT) | 1 |
| Resistencia 220Ω | 3 |
| Resistencia 10kΩ (pull-down) | 2 |
| Protoboard | 1 |
| Cables jumper | varios |

### Tabla de pines

| Componente | Pin Arduino |
|---|---|
| Pulsador A | 2 |
| Pulsador B | 3 |
| LED rojo (OR) | 9 |
| LED verde (AND) | 10 |
| LED amarillo (NOT A) | 11 |

### Montaje paso a paso

1. Colocar el Arduino Uno y la protoboard en el área de trabajo de Tinkercad.
2. Conectar los dos pulsadores en la protoboard. Una pata de cada pulsador va al riel de **5V**, la otra al pin de entrada del Arduino (pin 2 para A, pin 3 para B).
3. Agregar una resistencia de **10kΩ** entre la pata de señal de cada pulsador y **GND** (configuración pull-down). Esto asegura que la lectura sea LOW cuando el botón no está presionado.
4. Colocar los tres LEDs en la protoboard, cada uno con su resistencia de **220Ω** en serie, conectando el cátodo (pata corta) al riel de GND.
5. Conectar el ánodo de cada LED a su pin de Arduino correspondiente:
   - LED rojo → pin 9
   - LED verde → pin 19
   - LED amarillo → pin 11
6. Cargar el código en el editor de Tinkercad y ejecutar la simulación.

### Código

```cpp
int sa = 2;
int sb = 3;
int LEDR = 9;
int LEDV = 10;
int LEDA = 11;

int a = 0;
int b = 0;

void setup()
{
  pinMode(sa, INPUT);
  pinMode(sb, INPUT);
  pinMode(LEDR, OUTPUT);
  pinMode(LEDV, OUTPUT);
  pinMode(LEDA, OUTPUT);
}

void loop()
{
  a = digitalRead(sa);
  b = digitalRead(sb);

  // OR
  if (a == 1 || b == 1){
    digitalWrite(LEDR, HIGH);
  } else {
    digitalWrite(LEDR, LOW);
  }

  // AND
  if (a == 1 && b == 1){
    digitalWrite(LEDV, HIGH);
  } else {
    digitalWrite(LEDV, LOW);
  }

  // NOT (de A)
  if (a == 0){
    digitalWrite(LEDA, HIGH);
  } else {
    digitalWrite(LEDA, LOW);
  }
}
```

### Tabla de verdad

| A | B | OR — LED Rojo | AND — LED Verde | NOT A — LED Amarillo |
|:---:|:---:|:---:|:---:|:---:|
| 0 | 0 | 0 | 0 | 1 |
| 0 | 1 | 1 | 0 | 1 |
| 1 | 0 | 1 | 0 | 0 |
| 1 | 1 | 1 | 1 | 0 |

### Enlace Tinkercad

> _https://www.tinkercad.com/things/0piow9LvSK3-compuertas-logicas-con-arduino?sharecode=W51V23k6mAeEoootVxd71PzjBluTbWwFPSpCoOsAK6o_

### Imagen del circuito

> _Inserta aquí las capturas de pantalla del montaje_

---

## Proyecto 2 - Conversor Binario a Hexadecimal

### Objetivo
Convertir un número binario de 4 bits (0000 a 1111) a su equivalente hexadecimal (0 a F), mostrando el resultado en un display de 7 segmentos controlado por Arduino.

### 🧰 Materiales

| Componente | Cantidad |
|---|---|
| Arduino Uno | 1 |
| Display 7 segmentos (ánodo común) | 1 |
| DIP Switch de 4 posiciones | 1 |
| Resistencia 220Ω | 8 |
| Resistencia 10kΩ | 4 |
| Protoboard | 1 |
| Cables jumper | varios |

### Tabla de pines

| Componente | Pin Arduino |
|---|---|
| Switch bit 0 (menos significativo) | 2 |
| Switch bit 1 | 3 |
| Switch bit 2 | 4 |
| Switch bit 3 (más significativo) | 5 |
| Segmento A | 6 |
| Segmento B | 7 |
| Segmento C | 8 |
| Segmento D | 9 |
| Segmento E | 10 |
| Segmento F | 11 |
| Segmento G | 12 |

### Montaje paso a paso

1. Colocar el Arduino Uno, el protoboard y el display 7 segmentos de ánodo común en Tinkercad.
2. Conectar los pines **COM (3 y 8)** del display al riel de **5V** del protoboard (ánodo común).
3. Conectar cada segmento del display (a → g) a través de una resistencia de **220Ω** a los pines 6–12 del Arduino.
4. Colocar el DIP Switch en la protoboard:
   - Un lado de cada switch al riel de **5V**.
   - El otro lado al pin de entrada del Arduino + resistencia de **10kΩ** a GND (pull-down).
5. Verificar que el orden de los switches corresponda a los bits del 0 (menos significativo) al 3 (más significativo).
6. Cargar el código y simular. Probar con todas las combinaciones de 0000 a 1111.

### 💻 Código

```cpp
int bit0 = 2;
int bit1 = 3;
int bit2 = 4;
int bit3 = 5;

int segA = 6;
int segB = 7;
int segC = 8;
int segD = 9;
int segE = 10;
int segF = 11;
int segG = 12;

int tabla[16][7] = {
  {0,0,0,0,0,0,1}, // 0
  {1,0,0,1,1,1,1}, // 1
  {0,0,1,0,0,1,0}, // 2
  {0,0,0,0,1,1,0}, // 3
  {1,0,0,1,1,0,0}, // 4
  {0,1,0,0,1,0,0}, // 5
  {0,1,0,0,0,0,0}, // 6
  {0,0,0,1,1,1,1}, // 7
  {0,0,0,0,0,0,0}, // 8
  {0,0,0,0,1,0,0}, // 9
  {0,0,0,1,0,0,0}, // A
  {1,1,0,0,0,0,0}, // B
  {0,1,1,0,0,0,1}, // C
  {1,0,0,0,0,1,0}, // D
  {0,1,1,0,0,0,0}, // E
  {0,1,1,1,0,0,0}  // F
};

void setup() {
  pinMode(bit0, INPUT);
  pinMode(bit1, INPUT);
  pinMode(bit2, INPUT);
  pinMode(bit3, INPUT);
  pinMode(segA, OUTPUT);
  pinMode(segB, OUTPUT);
  pinMode(segC, OUTPUT);
  pinMode(segD, OUTPUT);
  pinMode(segE, OUTPUT);
  pinMode(segF, OUTPUT);
  pinMode(segG, OUTPUT);
}

void loop() {
  int a = digitalRead(bit0);
  int b = digitalRead(bit1);
  int c = digitalRead(bit2);
  int d = digitalRead(bit3);

  int valor = d*8 + c*4 + b*2 + a;

  digitalWrite(segA, tabla[valor][0]);
  digitalWrite(segB, tabla[valor][1]);
  digitalWrite(segC, tabla[valor][2]);
  digitalWrite(segD, tabla[valor][3]);
  digitalWrite(segE, tabla[valor][4]);
  digitalWrite(segF, tabla[valor][5]);
  digitalWrite(segG, tabla[valor][6]);
}
```
### Tabla de verificación (ejemplos)

| Bit 3 | Bit 2 | Bit 1 | Bit 0 | Valor decimal | Display |
|:---:|:---:|:---:|:---:|:---:|:---:|
| 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 0 | 1 | 1 | 1 |
| 0 | 1 | 0 | 1 | 5 | 5 |
| 1 | 0 | 0 | 0 | 8 | 8 |
| 1 | 1 | 0 | 0 | 12 | C |
| 1 | 1 | 1 | 1 | 15 | F |

### 🔗 Enlace Tinkercad

> _https://www.tinkercad.com/things/b4Ew9Ej4Tqf-conversor-de-binario-a-hexadecimal?sharecode=uJp8zckOjm61lZXayt4kRIqT1eKJvU7P0CfCkzwV2dI_

### Imagen del circuito

> _Inserta aquí las capturas de pantalla del montaje_

---

## Proyecto 3 - Semáforo

### Objetivo
Simular el ciclo de un semáforo real (rojo → amarillo → verde) con temporización definida, usando un Arduino Uno y tres LEDs.

### Materiales

| Componente | Cantidad |
|---|---|
| Arduino Uno | 1 |
| LED rojo | 1 |
| LED amarillo | 1 |
| LED verde | 1 |
| Resistencia 220Ω | 3 |
| Protoboard | 1 |
| Cables jumper | varios |

### Tabla de pines

| LED | Pin Arduino | Tiempo encendido |
|---|---|---|
| Rojo | 13 | 6 segundos |
| Amarillo | 12 | 2 segundos |
| Verde | 11 | 4 segundos |

### Montaje paso a paso

1. Colocar el Arduino Uno y el protoboard en Tinkercad.
2. Insertar los tres LEDs en la protoboard. El cátodo (pata corta) de cada LED va al riel de **GND**.
3. Conectar una resistencia de **220Ω** en serie con el ánodo de cada LED.
4. Conectar el otro extremo de cada resistencia al pin de Arduino correspondiente:
   - LED rojo → pin 13
   - LED amarillo → pin 12
   - LED verde → pin 11
5. Cargar el código y ejecutar la simulación. El semáforo debe ciclar automáticamente.

### Código

```cpp
int rojo = 13;
int amarillo = 12;
int verde = 11;

void setup()
{
  pinMode(rojo, OUTPUT);
  pinMode(amarillo, OUTPUT);
  pinMode(verde, OUTPUT);
}

void loop()
{
  digitalWrite(verde, LOW);
  digitalWrite(amarillo, LOW);
  digitalWrite(rojo, HIGH);
  delay(6000);

  digitalWrite(rojo, LOW);
  digitalWrite(verde, LOW);
  digitalWrite(amarillo, HIGH);
  delay(2000);

  digitalWrite(rojo, LOW);
  digitalWrite(amarillo, LOW);
  digitalWrite(verde, HIGH);
  delay(4000);
}
```
### Enlace Tinkercad

> _https://www.tinkercad.com/things/7X3q8NgJl8Y-semaforo?sharecode=wFN-IaZZKuAWqXzABXyL6YCO7LQ5EuAegkz241tXKRY_

### Imagen del circuito

> _Inserta aquí las capturas de pantalla del montaje_
