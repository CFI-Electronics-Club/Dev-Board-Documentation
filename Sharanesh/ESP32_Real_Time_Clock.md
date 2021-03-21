# Real time Clock using DS3231 module and OLED in ESP32
In this tutorial, we will learn about Real Time Clock (RTC) and its interfacing with the ESP32 and OLED display. We will use DS3231 RTC module to keep track of the correct time and display it on SPI OLED by using ESP32 as our microcontroller.

## What is RTC??
DS3231 is a RTC (Real Time Clock) module. It is used to maintain the date and time for most of the Electronics projects. This module has its own coin cell power supply using which it maintains the date and time even when the main power is removed or the MCU has gone through a hard reset. So once we set the date and time in this module it will keep track of it always. There are several types of RTC ICs available like DS1307, DS3231 etc.

![DS3231_Image](https://circuitdigest.com/sites/default/files/inlineimages/u/DS3231-RTC-Module.jpg)

## Materials Required
- ESP32
- DS3231 RTC module
- 7 pin 128×64 OLED display Module (SSD1306)
- Male-female wires
- Breadboard

## Circuit Diagram

![Circuit_diagram](https://circuitdigest.com/sites/default/files/circuitdiagram_mic/DS3231-Module-based-ESP32-Real-Time-Clock-circuit-diagram.png)

RTC DS3231 IC uses I2C mode of communication. It has SCL, SDA, Vcc and GND pins coming out of it. Connection of RTC module with ESP32 is given below:

1. SCL of RTC -> SCL of ESP32 i.e. Pin D22
2. SDA of RTC -> SDA of ESP32 i.e. Pin D21
3. GND of RTC -> GND of ESP32
4. Vcc of RTC -> Vcc of ESP32

Here, we are using SPI mode to connect our 128×64 OLED display Module (SSD1306) to ESP32. So, it will use 7 pins. Connections with ESP32 are given as:

1. CS(Chip select) pin of OLED -> PIN D5 of ESP32
2. DC pin of OLED -> PIN D4 of ESP32
3. RES pin of OLED -> PIN D2 of ESP32
4. SDA pin of OLED -> PIN D23 i.e. MOSI of ESP32
5. SCK pin of OLED -> PIN D18 i.e. SCK of ESP32
6. Vdd of OLED -> Vcc of ESP32
7. GND of OLED -> GND of ESP32

