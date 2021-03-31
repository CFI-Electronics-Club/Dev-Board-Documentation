# Serial to Serial Bluetooth
Bluetooth is the technology that enables exchange of data between devices within a short amount of distance.
This can be easily visualized using ESP32's Bluetooth Classic with Arduino IDE to exchange data between an ESP32 and an Android smartphone.

## Wireless Technology & Bluetooth
 * What is Bluetooth? - https://www.businessinsider.in/tech/how-to/what-is-bluetooth-a-beginners-guide-to-the-wireless-technology/articleshow/75851982.cms
 * How does it work? - https://www.scientificamerican.com/article/experts-how-does-bluetooth-work/
 * HC-05 Bluetooth module - https://www.electronicwings.com/sensors-modules/bluetooth-module-hc-05-
 * HC-05 and connections - https://www.youtube.com/watch?v=OhnxU8xALtg

## Terms
 * BluetoothSerial library - https://github.com/espressif/arduino-esp32/blob/master/libraries/BluetoothSerial/src/BluetoothSerial.h
 * Serial Monitor - https://learn.adafruit.com/adafruit-arduino-lesson-5-the-serial-monitor
 * Baud Rate - https://www.arduino.cc/en/Serial.Begin
 * What is an instance variable? - https://en.wikipedia.org/wiki/Instance_variable#:~:text=In%20object%2Doriented%20programming%20with,%2C%20but%20is%20non%2Dstatic.
 * Arguments and Parameters - https://www.geeksforgeeks.org/difference-between-argument-and-parameter-in-c-c-with-examples/
 * Install the Bluetooth Terminal application called “Serial Bluetooth Terminal” available on the Play Store.

## Testing

* Upload the below code onto your board to test if the functionality works after going throught the above resources
* After uploading the code, open the Serial Monitor at a baud rate of 115200. Press the ESP32 Enable button.
```
#include "BluetoothSerial.h" //library

//checks if Bluetooth is properly enabled.
#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif

//creates an instance of BluetoothSerial called SerialBT
BluetoothSerial SerialBT;

void setup() {
  Serial.begin(115200);         //initialize a serial communication at a baud rate of 115200.
  SerialBT.begin("CDBtest");   //Bluetooth device name CDBtest is the argument name, can be changed to anything unique
  Serial.println("The device started, now you can pair it with bluetooth!");
}

void loop() {
  if (Serial.available()) {
    SerialBT.write(Serial.read()); 
    //SerialBT.write() sends data using bluetooth serial.
    //Serial.read() returns the data received in the serial port.
  }
  if (SerialBT.available()) {
    Serial.write(SerialBT.read()); 
  }
  delay(20);
}
```
* On android, open the “Serial Bluetooth Terminal” app with Blutooth connection ON. 
* To pair new device, **Devices -> Pair new device -> CDBtest**
* After recieving the Connected message, you can exchange messages between App and Serial Monitor!!

[References](https://randomnerdtutorials.com/esp32-bluetooth-classic-arduino-ide/)
