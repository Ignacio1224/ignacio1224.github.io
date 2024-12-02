---
title: Automatismo para Elevadores Monofásicos
date: 2024-12-02
draft: false
tags:
  - programacion
  - sistemas
  - tecnologia
  - microcontroladores
  - impresion3D
  - arduino
  - electronica
  - lenguajeC
---
## Situación

Los talleres mecánicos suelen contar con elevadores, los cuales se clasifican en monofásicos o trifásicos. Sin embargo, estos equipos suelen ser lentos: para elevar un automóvil desde el suelo hasta una altura en la que una persona pueda caminar cómodamente por debajo, generalmente tardan alrededor de 90 segundos. Además, durante este proceso, el usuario debe mantener presionado un botón, lo que puede resultar incómodo y costoso en un entorno donde el tiempo es un factor crítico.

Para mejorar esta situación, se decidió implementar un sistema automatizado que permita seleccionar entre tres niveles predefinidos. Con solo presionar un botón, el elevador se desplazará automáticamente sin necesidad de intervención por parte del usuario.

El sistema está basado en un microcontrolador ATMEGA 328P (Arduino Nano), y cuenta con una serie de componentes como resistencias pull-up de 10 KΩ, tres sensores magnéticos y tres botones.

El diseño de este sistema se ha orientado a ser simple, de bajo costo y fácil de mantener. Por ello, se optó por utilizar materiales comunes y accesibles en el mercado, evitando componentes especializados. Los botones, de gran tamaño, se eligieron por su practicidad y por el aporte estético que brindan al conjunto. Los sensores magnéticos, económicos y ampliamente disponibles en el mercado de alarmas, también fueron seleccionados por su fiabilidad. Finalmente, el microcontrolador fue elegido por su bajo costo y su facilidad de programación, ofreciendo suficientes entradas y salidas digitales para las necesidades del proyecto.

### Requerimientos
- 3 Botones normalmente abiertos para las alturas fijadas
- 1 Botón normalmente cerrado como interruptor general
	![[BotonIndustrial.png]]

- 3 Sensores los cuales definen las alturas
	![[SensorMagnetico.png]]

- Arduino nano
	![[ArduinoNano.png]]

- Resistencias de 10 Kilo Ohm
	![[Resistencias10K.png]]

- Fuente de 12 V 400 ma
	![[Fuente12V400ma.png]]

- Relé
	![[RelayArduino.png]]

- Caja para guardar la electrónica 
- Estaño
- Cables

### Construcción

En primer lugar, se realizaron pruebas en un protoboard para evaluar la viabilidad de los componentes. Afortunadamente, las pruebas fueron exitosas desde el principio. Inicialmente, se consideraron llaves mecánicas como sensores, pero se desechó esta opción debido a que su uso implicaría un desgaste significativo con el tiempo.

![[ProtoboardElevador.jpg]]

El código para el Arduino fue escrito en lenguaje C utilizando el IDE de Arduino. En él, se definen los pines de entrada y salida, así como la lógica para determinar cuándo debe activarse o desactivarse el relé que controla el disyuntor del elevador. Además, se implementó una programación defensiva, de modo que si uno de los sensores falla, el sensor superior detiene automáticamente la acción del relé para evitar posibles fallos en el sistema.

### Código

```c
#define  OUTPUT_PIN       (2)   // Relay

#define LOW_LEVEL         (3)   // Low level button
#define MEDIUM_LEVEL      (4)   // Medium level button
#define HIGH_LEVEL        (5)   // High level button

#define LOW_LEVEL_END     (6)   // Low level sensor
#define MEDIUM_LEVEL_END  (7)   // Medium level sensor
#define HIGH_LEVEL_END    (8)   // High level sensor

// Level to reach
uint8_t levelToReach = 0;

// Elevating flag, true while elevating, false when done
bool working = false;

// Flag to write OUTPUT_PIN pin one time
bool switched = true;

/**
 * Returns the level to reach as the pin number asigned
 */
uint8_t getLevelToReach();

/*
 * If any sensor upper or in the same level is activated, 
 * the level is reached for protection.
*/
bool isLevelReached();

/**
 * Setup all the stuff
 */
void setup() {
  
  pinMode(OUTPUT_PIN, OUTPUT);
  
  pinMode(LOW_LEVEL, INPUT);
  pinMode(MEDIUM_LEVEL, INPUT);
  pinMode(HIGH_LEVEL, INPUT);

  pinMode(LOW_LEVEL_END, INPUT);
  pinMode(MEDIUM_LEVEL_END, INPUT);
  pinMode(HIGH_LEVEL_END, INPUT);

  levelToReach = 0;
  working = false;
  switched = true;

}

/**
 * Main loop
 */
void loop() {

  if (working) {

    uint8_t newLevel = getLevelToReach();

    if (newLevel != 0) {
      levelToReach = newLevel;
    }
    
    if (!switched) {
      digitalWrite(OUTPUT_PIN, HIGH);
      switched = true;
    }

    if (isLevelReached()) {
      digitalWrite(OUTPUT_PIN, LOW);
      working = false;
    }
    
  } else {
    
    levelToReach = getLevelToReach();
    working = levelToReach != LOW;
    switched = false;
  
  }

}

uint8_t getLevelToReach() {
  
  if (digitalRead(LOW_LEVEL) == HIGH)  return LOW_LEVEL;

  if (digitalRead(MEDIUM_LEVEL) == HIGH) return MEDIUM_LEVEL;

  if (digitalRead(HIGH_LEVEL) == HIGH) return HIGH_LEVEL;

  return 0;
 
}

bool isLevelReached() {
  
  if (levelToReach <= HIGH_LEVEL && digitalRead(HIGH_LEVEL_END) == HIGH) return true;

  if (levelToReach <= MEDIUM_LEVEL && digitalRead(MEDIUM_LEVEL_END) == HIGH) return true;

  if (levelToReach == LOW_LEVEL && digitalRead(LOW_LEVEL_END) == HIGH) return true;

  return false;
 
}
```

### Esquemático

![[EsquematicoAutomatismoElevador.png]]

![[EsquematicoArduinoElevador.png]]

### Diseño de la Caja

El diseño de la caja se realizó utilizando el software Fusion 360 de Autodesk, respetando las dimensiones de los componentes que se incluirían en su interior. La caja fue diseñada para alojar de manera adecuada los cuatro botones (tres en la parte frontal y uno en la superior), así como un espacio para los cables, el Arduino, la fuente de alimentación y el relé.

![[CajaElevador1.jpg]]

![[CajaElevador2.jpg]]

![[CajaElevador3.jpg]]