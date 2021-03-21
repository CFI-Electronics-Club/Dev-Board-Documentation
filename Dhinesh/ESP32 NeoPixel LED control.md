# Controlling WS2812 NeoPixel LED with ESP32 using Blynk App

NeoPixel LED Strip Lights are programmable RGB LED strip which can be programmed to generate any desired lighting pattern. NeoPixel can produce multiple colors in any combination and brightness. It consumes less power and can be addressed individually via programming. In this project, we will learn to control the WS2812 NeoPixel LED strips using ESP32 and Blynk application.

## Parts Required:

- 25 LEDs WS2812B NeoPixel LED Strip
- 5V, 2 AMP Power supply
- Electronics Club custom ESP32 Development board
- Breadboard
- Jumper wires

## Schematic:

![schematic](https://iotdesignpro.com/sites/default/files/inline-images/WS2812-ESP32-Circuit-Diagram.png)

## Blynk Application Setup for NeoPixel with ESP32

Blynk is an application that can run over Android and IOS devices to control any IoT devices using our smartphones. We can create our own Graphical user interface to design the IoT application GUI.
For more detailed instructions, refer [here](https://iotdesignpro.com/projects/controlling-ws2812-neopixel-led-with-esp32-using-blynk-app#:~:text=Open%20Arduino%20IDE%2C%20then%20go,controlling%20the%20RGB%20LED%20strip.)

## Libraries to include:

Here “Adafruit_NeoPixel.h” is used for controlling the RGB LED strip. For including Adafruit_NeoPixel.h library, download the library from [this link](https://github.com/adafruit/Adafruit_NeoPixel) and include it using the “Include ZIP Library” option.

## Code:

```
#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>
#include <Adafruit_NeoPixel.h>
#define PIN 15
#define NUM 25
Adafruit_NeoPixel pixels = Adafruit_NeoPixel(NUM,PIN, NEO_GRB + NEO_KHZ800);
char auth[] = "HoLYSq-Sxxxx";
char ssid[] = "admin";
char pass[] = "12345678";
int r,g,b,data;
void setup()
{
  Serial.begin(115200);
  Blynk.begin(auth, ssid, pass);
  pixels.begin();
}
void loop()
{
  Blynk.run();
}
BLYNK_WRITE(V2)
{
r = param[0].asInt();
g = param[1].asInt();
b = param[2].asInt();
if(data==0)
static1(r,g,b);
}
BLYNK_WRITE(V3)
{
data = param.asInt();
Serial.println(data);
if(data==0)
{
  static1(r,g,b);
}
else if(data==1)
{
  animation1();
}
}
void static1(int r, int g, int b)
{
  for(int i=0;i<=NUM;i++)
  {
  pixels.setPixelColor(i, pixels.Color(r,g,b));
  pixels.show();
  }
}
void animation1()
{
  for(int i=0;i<NUM;i++)
  {
    pixels.setPixelColor(i, pixels.Color(255,0,0));
    pixels.show();
    delay(100);
  }
  for(int i=NUM;i>=0;i--)
  {
    pixels.setPixelColor(i, pixels.Color(0,255,0));
    pixels.show();
    delay(100);
  }
  for(int i=0;i<NUM;i++)
  {
    pixels.setPixelColor(i, pixels.Color(0,255,255));
    pixels.show();
    delay(100);
  }
  for(int i=NUM;i>=0;i--)
  {
    pixels.setPixelColor(i, pixels.Color(255,255,0));
    pixels.show();
    delay(100);
  }
}
```

## Links:

This tutorial is extracted from [here](https://iotdesignpro.com/projects/controlling-ws2812-neopixel-led-with-esp32-using-blynk-app#:~:text=Open%20Arduino%20IDE%2C%20then%20go,controlling%20the%20RGB%20LED%20strip.). For more detailed explanation, you can refer to this link.

[Adafruit NeoPixel Library](https://github.com/adafruit/Adafruit_NeoPixel)
