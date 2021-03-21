# Soil moisture sensor circuit
## Description
In this mini project, we are going to use the LM393 soil moisture sensor and integrate it with the custom board to read the soil moisture data and display it on an OLED display.
## Concept 
The concept of this project is fairly simple. The readings from the soil moisture sensor are read and stored in an open source cloud platform named **Adafruit IO**. The data can be stored, analysed, plotted, etc. using this online platform. Also, the instantaneous soil moisture value is displayed on an OLED display.
## Prerequisites
Arduino IDE
## Components
* Electronics club custom development board
* OLED display
* Soil moisture sensor module
* Jumper wires
## Schematic
The following connections should be made:

* VCC of custom board - VDD of OLED - VCC of soil moisture sensor module
* GND of custom board - GND of OLED - GND of soil moisture sensor module
* VP of custom board - AO of soil moisture sensor module
* SCL of custom board - SCK of OLED
* SDA of custom board - SDA of OLED
* +,- terminals of soil moisture sensor module - FC-28 probe's terminals

![temp](https://github.com/CFI-Electronics-Club/Dev-Board-Documentation/blob/main/Ruban/images/soilmos.jpg)
## Code
<ul>
#include <WiFi.h>
#include <Wire.h>               // Only needed for Arduino 1.6.5 and earlier
#include<SH1106.h>      // legacy: #include "SSD1306.h"
#include "Adafruit_MQTT.h"
#include "Adafruit_MQTT_Client.h"
const char *ssid =  "Galaxy-M20";     // Enter your WiFi Name
const char *pass =  "ac312129"; // Enter your WiFi Password
SH1106 display(0x3c, 21, 22);
WiFiClient client;
#define MQTT_SERV "io.adafruit.com"
#define MQTT_PORT 1883
#define MQTT_NAME "choudharyas" // Your Adafruit IO Username
#define MQTT_PASS "988c4e045ef64c1b9bc8b5bb7ef5f2d9" // Adafruit IO AIO key
const int moisturePin = A0;             // moisteure sensor pin
int moisturePercentage;              //moisture reading
//Set up the feed you're publishing to
Adafruit_MQTT_Client mqtt(&client, MQTT_SERV, MQTT_PORT, MQTT_NAME, MQTT_PASS);
Adafruit_MQTT_Publish AgricultureData = Adafruit_MQTT_Publish(&mqtt,MQTT_NAME "/f/AgricultureData");  // AgricultureData is the feed name where you will publish your data
void setup()
{
  Serial.begin(115200);
  delay(10);
  Serial.println("Connecting to ");
  Serial.println(ssid);
  WiFi.begin(ssid, pass);
  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");              // print ... till not connected
  }
  Serial.println("");
  Serial.println("WiFi connected");
  display.init();
  display.flipScreenVertically();
  display.setFont(ArialMT_Plain_10);
}
 void loop()
{
    MQTT_connect();
  moisturePercentage = ( 100 - ( (analogRead(moisturePin) / 1023.00) * 100 ) );
  Serial.print("Soil Moisture is  = ");
  Serial.print(moisturePercentage);
  Serial.println("%");
  display.clear();
  display.setTextAlignment(TEXT_ALIGN_RIGHT);
  display.drawString(10, 128, String(millis()));
  display.setTextAlignment(TEXT_ALIGN_LEFT);
  display.setFont(ArialMT_Plain_24);
  display.drawString(50, 0, "Soil");
  display.drawString(25, 20, "Moisture");
  //display.setFont(ArialMT_Plain_24);
  String data1 = String(moisturePercentage);
  display.drawString(45, 40, data1);
  display.drawString(60, 40, "0 %");
  display.display();
       if (! AgricultureData.publish(moisturePercentage)) //This condition is used to publish the Variable (moisturePercentage) on adafruit IO. Change thevariable according to yours.
       {                     
         delay(5000);   
          }
 delay(6000);
}
void MQTT_connect() 
{
  int8_t ret;
  // Stop if already connected.
  if (mqtt.connected()) 
  {
    return;
  }
  uint8_t retries = 3;
  while ((ret = mqtt.connect()) != 0) // connect will return 0 for connected
  { 
       mqtt.disconnect();
       delay(5000);  // wait 5 seconds
       retries--;
       if (retries == 0) 
       {
         // basically die and wait for WDT to reset me
         while (1);
       }
  }
}
</ul>
Video
## Anything Extra (Debugging) (Optional)
...
## Follow up Problem Statement
...
## References
...
