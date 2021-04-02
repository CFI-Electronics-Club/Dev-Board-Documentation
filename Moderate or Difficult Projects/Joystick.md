# Joystick interfacing
## Description
In this project, we are going to interface HW-504 analog controller module to the laptop and use it in place of the good old WASD gaming controls.
## Concept
Concepts involve the working of the joystick module and the pushbutton trigger. Apart from that, it is just coding and using a special library to do the job.
## Components required
* Electronics club custom development board
* HW-504 joystick module 
* Jumper wires
## Libraries required
* BleKeyboard.h

BleKeyboard.h is the header file which is used to provide keystrokes to the laptop via bluetooth. However, this library is not directly installable through the library mananger. Instead the .zip file must be downloaded and then must be manually added. Follow this procedure:

1. Download the latest release of ESP32-BLE-Keyboard.zip from this github link: [T-vK/ESP32-BLE-Keyboard](https://github.com/T-vK/ESP32-BLE-Keyboard/releases)
2. Open you Arduino IDE and choose Sketch/Include Library/Add .ZIP Library and select the .zip file you downloaded

The library is now installed and can be used.
## Schematic
Make the following connections:
* GND of HW-504 <-> GND of Dev board
* +5V of HW-504 <-> Vcc(3.3V) of Dev board
* VRx of HW-504 <-> Pin 32 of Dev board
* VRy of HW-504 <-> Pin 34 of Dev board
* SW of HW-504 <-> Pin 35 of Dev board

![temp](https://github.com/CFI-Electronics-Club/Dev-Board-Documentation/blob/main/Moderate%20or%20Difficult%20Projects/Images/joystick.jpg)
## Working of the joystick
The analog joystick basically works based on the **Gimbal** mechanism. 
