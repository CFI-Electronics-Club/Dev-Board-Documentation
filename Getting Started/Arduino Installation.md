# Arduino IDE Installation
This tutorial will walk you through the installation part of Arduino Integrated Development Environment

## Instructions:
__Step 1:__ Installing Arduino IDE(Integrated development Environment)
- https://www.arduino.cc/en/Main/Software
<img src="https://github.com/CFI-Electronics-Club/Dev-Board-Documentation/blob/main/Getting%20Started/Images/arduino.png?raw=true" width="500" />

__Step 2:__ Installing ESP32 add on in Arduino IDE

- In arduino IDE,go to File>Preferences (or) Press Ctrl+Comma
- In the “Additional Boards Manager URLs” field paste this below url:
- https://dl.espressif.com/dl/package_esp32_index.json 
- Press OK 
<img src="https://github.com/CFI-Electronics-Club/Dev-Board-Documentation/blob/main/Getting%20Started/Images/boardmanager.png?raw=true" width="500" />

__Step 3:__ Installing ESP32 board

- In the IDE ,go to Tools>Boards>Board manager
- Type “esp32” in the search box
- Click esp32 by Espressif Systems” and Install the latest version(1.0.4)
<img src="https://github.com/CFI-Electronics-Club/Dev-Board-Documentation/blob/main/Getting%20Started/Images/library.png?raw=true" width="500" />

__Step 4:__ Selecting ESP32 Board

- In the IDE ,go to Tools>Boards>ESP32 Dev module
<img src="https://github.com/CFI-Electronics-Club/Dev-Board-Documentation/blob/main/Getting%20Started/Images/boardselecct.jpg?raw=true" width="500" />

## Troubleshooting
- Port needs to be assigned once the board is connected
  - For windows : “COM3” (or) “COM7”
  - For linux: “/dev/ttyUSB0”
  - For mac: “/dev/cu.SLAB_USBtoUART”

- If you get this error: “Port ____ cannot be found”
  - Install the USB to UART driver from this below link:
  - https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers

- Once you disconnect and reconnect the board ,press “RESET” button for regaining the previously compiled and uploaded sketch

- How to install Libraries:
  - In the IDE ,go to Tools>Manage Libraries (or) Ctrl+Shift+I
  - Search for a library and download it
