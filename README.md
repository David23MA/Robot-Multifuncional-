Robot Multifuncional
Repositorio del Proyecto: https://github.com/David23MA/Robot-Multifuncional-.git

Descripción
Este proyecto presenta un Robot Multifuncional desarrollado sobre la plataforma Arduino, diseñado para demostrar la integración de sistemas de control, sensado y movimiento. El robot está equipado con una doble funcionalidad clave: teleoperación inalámbrica y navegación autónoma con evasión de obstáculos.

El objetivo principal es crear una plataforma versátil y robusta que sirva como base para la automatización en entornos dinámicos, contribuyendo a soluciones de accesibilidad y seguridad.
Problema que resuelve
Demostrar principios de mecatrónica básica mediante un sistema interactivo que integra control electrónico y mecánica, resolviendo la necesidad de crear una interfaz física entre el usuario y un entorno controlado.

Propósito
Demostrar la integración eficiente del sistema del robot multifuncional, control y movimiento. Su misión es crear una plataforma tecnológica que sirva como base para soluciones de accesibilidad, seguridad industrial y robótica de servicio.

Contexto y Alcance
El robot multifuncional se concibió como un proyecto de integración de sistemas (hardware y software) dentro de un contexto académico. Su alcance se centra en: Movimiento multidireccional (4 motores), Detección precisa de proximidad (Sensor Ultrasónico); y Control inalámbrico (Módulo Bluetooth).

Arquitectura general
El sistema se basa en un ciclo constante de: Recepción de Comandos → Detección de Entorno → Toma de Decisiones → Ejecución de Movimiento.
1. Recepción de Comandos: El Módulo Bluetooth recibe comandos (datos seriales) de una aplicación móvil.
2. Procesamiento (Arduino UNO): El microcontrolador interpreta estos datos para establecer la dirección deseada (ej. "avanzar").
3. Detección de Obstáculos: El Sensor Ultrasónico, montado sobre el Servomotor, realiza un barrido de su entorno.
4. Toma de Decisiones (Algoritmo): Si la distancia detectada por el sensor es menor a un umbral predefinido, el Arduino ignora la orden del Bluetooth y ejecuta una rutina de evasión automática (detenerse, girar).
5. Ejecución de Movimiento: Las señales de control se envían a la Placa de Control de Motores (Driver), que suministra la potencia necesaria a los cuatro motores para realizar el movimiento indicado.


Tecnologías usadas
Hardware
Arduino UNO board (placa).
Arduino (en general).
Chasis para carro.
4 Motores y 4 Ruedas.
Servomotor.
Sensor Ultrasónico.
Módulo Bluetooth.
Módulo para pilas(portabaterías).
Cable USB.
6 Cables Jumper.

Software
Arduino IDE
Librerias
  Servo.h
  AFMotot.h

Estructura de carpetas del proyecto
PIA Robot Multifuncional/
├── codigo/
│   └── robot/
│       └── robot.ino
├── docs/
│   ├── diagrama de conexiones.png
│   └── documentacion.docx
├── hardware/
│   └── materiales.txt
└── (Archivos raíz como README.md, LICENSE.md, etc.)

Cómo usar el proyecto (Instalación y ejecución)
Prerrequisitos
Tener instalado Arduino IDE
Placa Arduino Uno
Componentes listados en la sección de materiales
Instalación paso a paso
Paso 1: Entrar a la página oficial de arduino, y presionar el botón descargar Paso 
2: Seleccionar ubicación de donde se guardará y presionar el botón “Guardar” Paso 
3: Abrir el instalador de Arduino y presionar “Acepto” Paso 
4: Presionar siguiente en la ventana de “Elegir opciones de instalación” Paso 
5: Seleccionar la carpeta de destino y presionar “Instalar” Paso 
6: Verificar el programa Paso 
7: Subir el programa Paso 
8: Probar el proyecto

Código y decisiones técnicas
int distance;
int Left;
int Right;
int L = 0;
int R = 0;
int L1 = 0;
int R1 = 0;
Servo servo;
AF_DCMotor M1(1);
AF_DCMotor M2(2);
AF_DCMotor M3(3);
AF_DCMotor M4(4);
void setup() {
Serial.begin(9600);
pinMode(Trig, OUTPUT);
pinMode(Echo, INPUT);
servo.attach(motor);
M1.setSpeed(Speed);
M2.setSpeed(Speed);
M3.setSpeed(Speed);
M4.setSpeed(Speed);
}
void loop() {
//Obstacle();
//Bluetoothcontrol();
}
void Bluetoothcontrol() {
if (Serial.available() > 0) {
value = Serial.read();
Serial.println(value);
}
if (value == 'F') {
forward();
} else if (value == 'B') {
backward();
} else if (value == 'L') {
left();
} else if (value == 'R') {
right();
} else if (value == 'S') {
Stop();
}
}
void Obstacle() {
distance = ultrasonic();
if (distance <= 12) {
Stop();
backward();
delay(100);
Stop();
L = leftsee();
servo.write(spoint);
delay(800);
R = rightsee();
servo.write(spoint);
if (L < R) {
right();
delay(500);
Stop();
delay(200);
} else if (L > R) {
left();
delay(500);
Stop();
delay(200);
}
} else {
forward();
}
}
void voicecontrol() {
if (Serial.available() > 0) {
value = Serial.read();
Serial.println(value);
if (value == '^') {
forward();
} else if (value == '-') {
backward();
} else if (value == '<') {
L = leftsee();
servo.write(spoint);
if (L >= 10 ) {
left();
delay(500);
Stop();
} else if (L < 10) {
Stop();
}
} else if (value == '>') {
R = rightsee();
servo.write(spoint);
if (R >= 10 ) {
right();
delay(500);
Stop();
} else if (R < 10) {
Stop();
}
} else if (value == '*') {
Stop();
}
}
}
// Ultrasonic sensor distance reading function
int ultrasonic() {
digitalWrite(Trig, LOW);
delayMicroseconds(4);
digitalWrite(Trig, HIGH);
delayMicroseconds(10);
digitalWrite(Trig, LOW);
long t = pulseIn(Echo, HIGH);
long cm = t / 29 / 2; //time convert distance
return cm;
}
void forward() {
M1.run(FORWARD);
M2.run(FORWARD);
M3.run(FORWARD);
M4.run(FORWARD);
}
void backward() {
M1.run(BACKWARD);
M2.run(BACKWARD);
M3.run(BACKWARD);
M4.run(BACKWARD);
}
void right() {
M1.run(BACKWARD);
M2.run(BACKWARD);
M3.run(FORWARD);
M4.run(FORWARD);
}
void left() {
M1.run(FORWARD);
M2.run(FORWARD);
M3.run(BACKWARD);
M4.run(BACKWARD);
}
void Stop() {
M1.run(RELEASE);
M2.run(RELEASE);
M3.run(RELEASE);
M4.run(RELEASE);
}
int rightsee() {
servo.write(20);
delay(800);
Left = ultrasonic();
return Left;
}
int leftsee() {
servo.write(180);
delay(800);
Right = ultrasonic();
return Right;
}
