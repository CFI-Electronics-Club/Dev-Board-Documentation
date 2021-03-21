# ESP32 Lora communication

In this tutorial we’ll explore the basic principles of LoRa, and how it can be used with the ESP32 for IoT projects using the Arduino IDE. To get you started, we’ll also show you how to create a simple LoRa Sender and LoRa Receiver with the RFM95 transceiver module.

## What is LoRa?

- LoRa is a radio modulation technique;
- LoRa allows long-distance communication of small amounts of data and requires low power;
- You can use LoRa in point to point communication or in a network;
- LoRa can be especially useful if you want to monitor sensors that are not covered by your Wi-Fi network and that are several meters apart.

## Parts Required:

- 2x ESP32 Board
- 2x LoRa Transceiver modules (RFM95)
- RFM95 LoRa breakout board (optional)
- Jumper wires
- Breadboard or stripboard

## Installing the LoRa Library

[arduino-LoRa library by sandeep mistry](https://github.com/sandeepmistry/arduino-LoRa)

## Schematic:

![schematic](https://i2.wp.com/randomnerdtutorials.com/wp-content/uploads/2018/06/LoRa_ESP32_Wiring.png?w=794&quality=100&strip=all&ssl=1)

## LoRa sender sketch:

```
#include <SPI.h>
#include <LoRa.h>

//define the pins used by the transceiver module
#define ss 5
#define rst 14
#define dio0 2

int counter = 0;

void setup() {
  //initialize Serial Monitor
  Serial.begin(115200);
  while (!Serial);
  Serial.println("LoRa Sender");

  //setup LoRa transceiver module
  LoRa.setPins(ss, rst, dio0);

  //replace the LoRa.begin(---E-) argument with your location's frequency
  //433E6 for Asia
  //866E6 for Europe
  //915E6 for North America
  while (!LoRa.begin(866E6)) {
    Serial.println(".");
    delay(500);
  }
   // Change sync word (0xF3) to match the receiver
  // The sync word assures you don't get LoRa messages from other LoRa transceivers
  // ranges from 0-0xFF
  LoRa.setSyncWord(0xF3);
  Serial.println("LoRa Initializing OK!");
}

void loop() {
  Serial.print("Sending packet: ");
  Serial.println(counter);

  //Send LoRa packet to receiver
  LoRa.beginPacket();
  LoRa.print("hello ");
  LoRa.print(counter);
  LoRa.endPacket();

  counter++;

  delay(10000);
}

```

## LoRa sender sketch:

```
#include <SPI.h>
#include <LoRa.h>

//define the pins used by the transceiver module
#define ss 5
#define rst 14
#define dio0 2

void setup() {
  //initialize Serial Monitor
  Serial.begin(115200);
  while (!Serial);
  Serial.println("LoRa Receiver");

  //setup LoRa transceiver module
  LoRa.setPins(ss, rst, dio0);

  //replace the LoRa.begin(---E-) argument with your location's frequency
  //433E6 for Asia
  //866E6 for Europe
  //915E6 for North America
  while (!LoRa.begin(866E6)) {
    Serial.println(".");
    delay(500);
  }
   // Change sync word (0xF3) to match the receiver
  // The sync word assures you don't get LoRa messages from other LoRa transceivers
  // ranges from 0-0xFF
  LoRa.setSyncWord(0xF3);
  Serial.println("LoRa Initializing OK!");
}

void loop() {
  // try to parse packet
  int packetSize = LoRa.parsePacket();
  if (packetSize) {
    // received a packet
    Serial.print("Received packet '");

    // read packet
    while (LoRa.available()) {
      String LoRaData = LoRa.readString();
      Serial.print(LoRaData);
    }

    // print RSSI of packet
    Serial.print("' with RSSI ");
    Serial.println(LoRa.packetRssi());
  }
}
```

## Taking it further:

Now, you should test the communication range between the Sender and the Receiver on your area. The communication range greatly varies depending on your environment (if you live in a rural or urban area with a lot of tall buildings). To test the communication range you can add an OLED display to the LoRa receiver and go for a walk to see how far you can get a communication.

## Link:

This tutorial is extracted from [here](https://randomnerdtutorials.com/esp32-lora-rfm95-transceiver-arduino-ide/). For more detailed explanation, you can refer to this link.

[arduino-LoRa library by sandeep mistry](https://github.com/sandeepmistry/arduino-LoRa)
