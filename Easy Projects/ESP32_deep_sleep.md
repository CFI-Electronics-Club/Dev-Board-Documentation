# ESP32 deep sleep tutorial
## Description
We'll see how we can put the custom board in deep sleep mode to conserve power and make projects battery friendly.       
## Concept
Inside the chip, we can find the two processing cores, the RAM and ROM memory, the WiFi module, the Bluetooth Module, a hardware acceleration module for cryptographic applications, the RTC module, and a lot of peripherals. Inside the RTC module, we can find a PMU (Phasor measurement unit) a small and very low power 32-bit co-processor, and 8Kbs of RAM memory. This small amount of memory is very useful as you are going to find out in a moment. 

![](https://content.instructables.com/ORIG/FHZ/LVVU/JG74UWE7/FHZLVVUJG74UWE7.jpg?auto=webp&frame=1&width=1024&height=1024&fit=bounds&md=df551b0e8ae4bc735b4e26d1fd4cfa83)

The WiFi modules, the Processing Cores, and the Bluetooth module require a lot of current to operate. So, if we want to conserve power we have to disable them when don’t use them. We put the ESP32 to deep sleep mode, where it disables everything except the RTC module. There is a light sleep mode and the Deep sleep mode. In Deep Sleep mode the chip offers the lowest power consumption. It just needs 0.01 mAs of current in Deep Sleep mode and that’s the purpose of the tutorial.
## Prerequisites
Arduino IDE
## Components
* Electronics Club Custom Development Board (CDB)          
* Light-Dependent Resistor (LDR)             
* Buzzer
* Resistor - 10k
* Jumpers
* Breadboard   
## Schematic
Buzzer Pin 1 --> 4 of CDB             
Buzzer Pin 2 --> GND                    
LDR Leg 1 --> 10k Resistor --> 5 of CDB              
LDR Leg 1 --> VCC           
LDR Leg 2 --> GND               
###### (Note: Leg 1 and Leg 2 are named arbitrarily)
![](Images/Theremin.png)
## Code
```
int photopin = 5; // Pin where the photo resistor is connected to
int photValue; // The analog reading from the photoresistor
int buzzerPin = 4; // Connect Buzzer to Pin 4
long buzzerFreq; // The frequency to buzz the buzzer
// You can experiment with these values:
long buzzMAX = 2500; // Maximum frequency for the buzzer
long photoMAX = 1023; // Maximum value for the photoresistor
void setup() {
    pinMode(buzzerPin, OUTPUT); // set a pin for buzzer output
}
void loop() {
    // read the values of the photoresistor
    photValue = analogRead(photopin); // Values 0-1023
    // normalize the readings of a photoresistor to that of the buzzer and photoresistor
    buzzerFreq = (photValue * buzzMAX) / photoMAX;
    buzz(buzzerPin, buzzerFreq, 10);
}
void buzz(int targetPin, long frequency, long length) {
    long delayValue = 1000000/frequency/2;
    long numCycles = frequency * length/ 1000;
    for (long i=0; i < numCycles; i++){
        digitalWrite(targetPin,HIGH);
        delayMicroseconds(delayValue);
        digitalWrite(targetPin,LOW);
        delayMicroseconds(delayValue);
    }
}
```
## Follow-up Problem Statement
Can you use the capacitive touch pins and Aluminium foil to change the frequency of the buzzer as you slide your finger over the foil??
## References
[Simple Theremin with ESP32](https://www.instructables.com/Make-a-Pocket-Size-Theremin-With-ESP32/)
