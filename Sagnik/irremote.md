# Universal IR Remote Conroller
## Description
we are going to Convert an Android Phone into an IR Remote using Arduino to control various devices at home.
## Concept
Most of the IR remotes work around 38 KHz frequencies (this is the reason why I have chosen 1838T). On further involving into this topic one will recognize that there’s no fixed representation for zeros and ones in these IR data transmission methods. An Android Phone with the custom made Android App sends the signal to Dev Board circuit over Bluetooth, further the board receives the signal through TSOP-IR receiver (1838T) and analyses it. Then board commands the IR LED to blink in a particular pattern, corresponding to the button pressed on that Android Device App. This blinking pattern is captured by TV or Set-Top box’s IR receiver and it follows the instruction accordingly like changing the channel or increasing the volume.
## Prerequisites
* Arduino IDE
* IR Remote Library
## Components
* IR LED
* TSOP-IR receiver (1838T)
* Bluetooth module (HC05)
* Android Device
* Dev Board
## Schematic
...
## Code
```
#include <IRremote.h>
#include <SoftwareSerial.h>

IRsend irsend;
 
SoftwareSerial mySerial(9,10); // RX, TX

char ch;
int i=0;
 
void setup()
{
//  Serial.begin(9600);
  mySerial.begin(9600);
}

void loop() 
{
  ch=mySerial.read();
  
  // TV buttons: ON/OFF, Mute, AV/TV, Vol+, Vol-, OK, UP, Down, Left, Right, Menu, Exit, Return
  // Set-top box buttons: ON/OFF, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, V+, V-, Ch+, Ch-, Back, OK, Up, Down, Left, Right, Menu, Fav
  switch(ch)
  {
    //TV
    case 'O': 
    irsend.sendNEC(0x2FD48B7, 32);
    break;

    case 'X': 
    irsend.sendNEC(0x2FD08F7, 32);
    break;
     case 'A': 
    irsend.sendNEC(0x2FD28D7, 32);
    break;

    case '=': 
    irsend.sendNEC(0x2FD58A7, 32);
    break;

    case '-': 
    irsend.sendNEC(0x2FD7887, 32);
    break;

    case 'K': 
    irsend.sendNEC(0x2FD847B, 32);
    break;

    case 'U': 
    irsend.sendNEC(0x2FD9867, 32);
    break;

    case 'D': 
    irsend.sendNEC(0x2FDB847, 32);
    break;
     case 'L': 
    irsend.sendNEC(0x2FD42BD, 32);
    break;

    case 'R': 
    irsend.sendNEC(0x2FD02FD, 32);
    break;

    case 'M': 
    irsend.sendNEC(0x2FDDA25, 32);
    break;

    case 'E': 
    irsend.sendNEC(0x2FDC23D, 32);
    break;

    case 'B': 
    irsend.sendNEC(0x2FD26D9, 32);
    break;
  
    //Set-top box
    case 'o': 
    irsend.sendNEC(0x80BF3BC4, 32);
    break;

    case '1': 
    irsend.sendNEC(0x80BF49B6, 32);
    break;

    case '2': 
    irsend.sendNEC(0x80BFC936, 32);
    break;
       case '3': 
    irsend.sendNEC(0x80BF33CC, 32);
    break;

    case '4': 
    irsend.sendNEC(0x80BF718E, 32);
    break;

    case '5': 
    irsend.sendNEC(0x80BFF10E, 32);
    break;

    case '6': 
    irsend.sendNEC(0x80BF13EC, 32);
    break;

    case '7': 
    irsend.sendNEC(0x80BF51AE, 32);
    break;

    case '8': 
    irsend.sendNEC(0x80BFD12E, 32);
    break;

    case '9': 
    irsend.sendNEC(0x80BF23DC, 32);
    break;

    case '0': 
    irsend.sendNEC(0x80BFE11E, 32);
    break;

    case '+': 
    irsend.sendNEC(0x80BFBB44, 32);
    break;
    case '_': 
    irsend.sendNEC(0x80BF31CE, 32);
    break;

    case 'C': 
    irsend.sendNEC(0x80BF19E6, 32);
    break;

    case 'c': 
    irsend.sendNEC(0x80BFE916, 32);
    break;

    case 'b': 
    irsend.sendNEC(0x80BF43BC, 32);
    break;

    case 'k': 
    irsend.sendNEC(0x80BF738C, 32);
    break;

    case 'u': 
    irsend.sendNEC(0x80BF53AC, 32);
    break;

    case 'd': 
    irsend.sendNEC(0x80BF4BB4, 32);
    break;

    case 'l': 
    irsend.sendNEC(0x80BF9966, 32);
    break;

    case 'r': 
    irsend.sendNEC(0x80BF837C, 32);
    break;
     case 'm': 
    irsend.sendNEC(0x80BF11EE, 32);
    break;

    case 'f': 
    irsend.sendNEC(0x80BFA35C, 32);
    break;
  }
 }
```

## Follow up Problem Statement

## References
[Circuit Digest](https://circuitdigest.com/microcontroller-projects/universal-ir-remote-control-using-arduino-android)
