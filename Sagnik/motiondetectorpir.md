# Motion and Gesture Detection using PIR sensor
## Description
We will interface the dev board with PIR module and blink a LED and beep a Buzzer whenever a movement is detected.
## Prerequisites
* Arduino IDE
## Concept
The PIR sensor stands for Passive Infrared sensor. It is a low cost sensor which can detect the presence of Human beings or animals. There are two important materials present in the sensor one is the pyroelectric crystal which can detect the heat signatures from a living organism (humans/animals)   and the other is a Fresnel lenses which can widen the range of the sensor.
## Components
* Dev Board
* PIR Sensor Module
* LED
* Buzzer
* Resistors
* Breadboard
## Schematic
...
## Code
```
void setup() {
  pinMode(2, INPUT); //Pin 2 as INPUT
  pinMode(3, OUTPUT); //PIN 3 as OUTPUT
}

void loop() {
  if (digitalRead(2) == HIGH)
  {
  digitalWrite(3, HIGH);   // turn the LED/Buzz ON
  delay(100);                       // wait for 100 msecond 
  digitalWrite(3, LOW);   // turn the LED/Buzz OFF
  delay(100);                       // wait for 100 msecond 
  }
}
```
## Follow up Problem Statement
Try building a simple home automation system using the Dev Board and PIR sensor.
## References
[Circuit Digest](https://circuitdigest.com/microcontroller-projects/arduino-motion-detector-using-pir-sensor)

