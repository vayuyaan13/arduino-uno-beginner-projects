# Arduino Uno R3 Beginner Projects

A practical collection of beginner-friendly Arduino Uno R3 projects and experiments. Each project focuses on one core skill: reading an input, controlling an output, working with a sensor, or combining simple parts into a useful circuit.

## What you will learn

- How an Arduino Uno R3 runs a sketch
- Digital input and output
- Analog input and PWM output
- Serial Monitor debugging
- Working with LEDs, buttons, buzzers, and sensors
- Basic motor control
- How to test a circuit one part at a time
- How to turn a working experiment into a small project

## Before you start

You will usually need an Arduino Uno R3, USB cable, breadboard, jumper wires, LEDs, resistors, and a few input components. Some projects need additional sensors or a motor driver.

Never connect an LED without an appropriate current-limiting resistor, and do not connect motors directly to Arduino I/O pins.

## Project 1: LED blink

The simplest starting point is controlling an LED from a digital output.

```cpp
const int LED_PIN = 13;

void setup() {
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_PIN, HIGH);
  delay(500);
  digitalWrite(LED_PIN, LOW);
  delay(500);
}
```

This teaches the basic `setup()` and `loop()` structure and how a digital output changes state.

## Project 2: Push button and LED

A button introduces digital input. The example below uses the Uno's internal pull-up resistor, so the input reads LOW when the button is pressed.

```cpp
const int BUTTON_PIN = 2;
const int LED_PIN = 13;

void setup() {
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  bool pressed = digitalRead(BUTTON_PIN) == LOW;
  digitalWrite(LED_PIN, pressed ? HIGH : LOW);
}
```

Connect the button between pin 2 and GND. No external pull-up resistor is required for this arrangement.

## Project 3: Read a potentiometer

A potentiometer is a simple way to learn analog input. Connect its center pin to A0 and its outer pins to 5 V and GND.

```cpp
const int POT_PIN = A0;

void setup() {
  Serial.begin(9600);
}

void loop() {
  int value = analogRead(POT_PIN);
  Serial.println(value);
  delay(100);
}
```

Open the Serial Monitor at 9600 baud and turn the potentiometer. The Uno's analog input returns a value from 0 to 1023.

## Project 4: Control LED brightness with PWM

The Uno can use PWM on pins marked with `~`. The following example maps a potentiometer reading to LED brightness.

```cpp
const int POT_PIN = A0;
const int LED_PIN = 9;

void setup() {
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  int sensorValue = analogRead(POT_PIN);
  int brightness = map(sensorValue, 0, 1023, 0, 255);
  analogWrite(LED_PIN, brightness);
}
```

PWM is useful for simple speed or brightness control, but it is not the same as supplying a variable DC voltage.

## Project 5: Buzzer alert

A buzzer can provide audible feedback when an event occurs.

```cpp
const int BUZZER_PIN = 8;

void setup() {
  pinMode(BUZZER_PIN, OUTPUT);
}

void loop() {
  tone(BUZZER_PIN, 1000);
  delay(300);
  noTone(BUZZER_PIN);
  delay(700);
}
```

Use a buzzer suitable for the voltage and current available from your circuit. For larger loads, use an appropriate driver circuit rather than an Arduino pin directly.

## Project 6: Ultrasonic distance reading

An HC-SR04 ultrasonic sensor can measure distance by sending a pulse and timing its echo. A typical connection uses two digital pins.

```cpp
const int TRIG_PIN = 9;
const int ECHO_PIN = 10;

void setup() {
  Serial.begin(9600);
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
}

void loop() {
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  unsigned long duration = pulseIn(ECHO_PIN, HIGH, 30000);

  if (duration == 0) {
    Serial.println("No echo");
  } else {
    float distanceCm = duration * 0.0343 / 2.0;
    Serial.print("Distance: ");
    Serial.print(distanceCm, 1);
    Serial.println(" cm");
  }

  delay(100);
}
```

Mount the sensor so that the transducers have a clear view of the target. Readings can change with the target's shape, angle, and surface.

## Project 7: Servo motor sweep

A small hobby servo can be controlled with the Arduino Servo library.

```cpp
#include <Servo.h>

Servo myServo;

void setup() {
  myServo.attach(9);
}

void loop() {
  for (int angle = 0; angle <= 180; angle++) {
    myServo.write(angle);
    delay(10);
  }

  for (int angle = 180; angle >= 0; angle--) {
    myServo.write(angle);
    delay(10);
  }
}
```

For servos that draw significant current, use an appropriate external supply and connect the grounds together.

## Project 8: Control a DC motor safely

A DC motor should be driven through a motor driver such as an L293D rather than directly from an Arduino output pin. A motor driver lets the Uno control direction and, with a suitable enable connection, speed.

A simple motor project teaches an important robotics principle: the microcontroller supplies control signals while the driver handles the motor load.

## A simple learning path

If you are completely new to Arduino, work through the projects in this order:

1. LED blink
2. Push button and LED
3. Potentiometer and Serial Monitor
4. PWM LED brightness
5. Buzzer
6. Ultrasonic sensor
7. Servo
8. DC motor with a driver

Do not move to a more complex project until you can explain what each pin is doing in the current one.

## Troubleshooting checklist

When a project does not work, check the basics first:

1. Confirm the correct board and port are selected in Arduino IDE.
2. Upload a known-good Blink sketch to verify the board.
3. Check GND connections before checking software.
4. Compare every wire with the circuit diagram.
5. Confirm sensor modules are receiving the expected supply voltage.
6. Open Serial Monitor when the project has serial output.
7. Test one component at a time.
8. Reduce the circuit to the smallest working version before adding features.

## Next step

For a broader beginner reference covering the Arduino Uno R3 board, its pins, setup, programming basics, and practical uses, see the related guide on Vayuyaan:

**[Arduino Uno R3: Complete Guide for Beginners](https://vayuyaan.com/blog/arduino-uno-r3-complete-guide-for-beginners/)**

Use that guide as background while working through the examples here.

## License

This project is released under the MIT License.
