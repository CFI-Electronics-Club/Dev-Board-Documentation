# ESP32 as Bluetooth Keyboard

## Description:
 - ESP32 has BLE support thus it can be used as Bluetooth Keyboard with open source library
 - Compatible with Windows,Linux,Android,MacOSX,iOS
## Concept: 
 - Making use of the Blutooth Capability of ESP32 board,we are going to send keystrokes to the device we want to.
 - Various use cases are possible:
    - Automating any set of actions
    - Taking a photo with a hand trigger 
    - and many more

## Parts Required:
 - ESP32 Board
 - Push buttons
 - Jumper wires
## Schematics:
 ![BLE_KEYBOARD](https://user-images.githubusercontent.com/55990571/113199303-964e8a00-9284-11eb-800e-b3ee8b8a1a75.png)

## Instructions:
1. Load Arduino IDE
2. Install the library from here (https://github.com/T-vK/ESP32-BLE-Keyboard/releases)
3. Add the .ZIP library from the sketch menu
4. Make the connections according to the schematic
5. Try out the below codes:

***Space bar button***
 - Upload the below code
 - Connect with PC using Bluetooth
 - Open "chrome://dino" and start playing 
```
//D13       Button pin
//3.3V      Connected to the other leg of the button

#include <BleKeyboard.h>
//Set the name of the bluetooth keyboard (that shows up in the bluetooth menu of your device)
BleKeyboard bleKeyboard("ESP_KEYBOARD");

const int buttonPin = 13;
//Set the old button state to be LOW/false; which means not pressed
boolean oldPinState = LOW;

void setup() {
  //Start the Serial communication (with the computer at 115200 bits per second)
  Serial.begin(115200);
  //Send this message to the computer
  Serial.println("Starting BLE work!");
  //Begin the BLE keyboard/start advertising the keyboard (so phones can find it)
  bleKeyboard.begin();
  //Make the button pin an INPUT_PULLDOWN pin, which means that it is normally LOW, untill it is pressed/ connected to the 3.3V
  pinMode(buttonPin, INPUT_PULLDOWN);
}

void loop() {

  if (bleKeyboard.isConnected()) {
    //if the keyboard is connected to a device
    if (digitalRead(buttonPin) == HIGH) {
      //If the button pin is pressed (since it is pulled down, it is pressed when it is high)
      if (oldPinState == LOW) {
        //if the old Pin state was LOW and the button pin is HIGH than...
        //send the spacebar
        bleKeyboard.print(" ");
      }
      oldPinState = HIGH;
    } else {
      oldPinState = LOW;
    }
  }
  delay(10);
}
```

https://user-images.githubusercontent.com/55990571/113218770-b9d1fe80-929d-11eb-8ad7-c6e9cb6ac754.mp4

![BLE_KEYBOARD_gif](https://user-images.githubusercontent.com/55990571/113268477-0a287b00-92f5-11eb-830b-f7b92f53a6fa.gif)


***Play/Pause button***
- Upload the below code
- Connect with Phone using Bluetooth
- Play your favourite music playlist,and now you can play pause the song using the the ESP_keyboard

```
#include <BleKeyboard.h>
//Se the name of the bluetooth keyboard (that shows up in the bluetooth menu of your device)
BleKeyboard bleKeyboard("ESP_KEYBOARD");

const int buttonPin = 13;
//Set the old button state to be LOW/false; which means not pressed
boolean oldPinState = LOW;
int state = 0;

void setup() {
  //Start the Serial communication (with the computer at 115200 bits per second)
  Serial.begin(115200);
  //Send this message to the computer
  Serial.println("Starting BLE work!");
  //Begin the BLE keyboard/start advertising the keyboard (so phones can find it)
  bleKeyboard.begin();
  //Make the button pin an INPUT_PULLDOWN pin, which means that it is normally LOW, untill it is pressed/ connected to the 3.3V
  pinMode(buttonPin, INPUT_PULLDOWN);
}

void loop() {

  if (bleKeyboard.isConnected()) {
if (state == 0 && digitalRead(buttonPin) == HIGH) {
    state = 1;
    bleKeyboard.write(KEY_MEDIA_PLAY_PAUSE);
  }
  if (state == 1 && digitalRead(buttonPin) == LOW) {   
    state = 0;
  }
  }
  delay(10);
}
```
