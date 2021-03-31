# Instagram Follower Counter
## Description
 Displays Instagram followers count and current Time

## Prerequisites
 - NTP (You can refer this: [Internet Clock](https://github.com/CFI-Electronics-Club/Dev-Board-Documentation/blob/main/Sashank/Internet%20Clock.md))
## Components
 - ESP32 Board
 - OLED display
## Schematic
 ![esp32_oled](https://user-images.githubusercontent.com/55990571/113220065-31a12880-92a0-11eb-9ca9-936948460ad8.png)
## Code
```
#include <Wire.h>                   // Only needed for Arduino 1.6.5 and earlier
#include "SSD1306.h"                // alias for `#include "SSD1306Wire.h"`
#include <WiFi.h>
#include <NTPClient.h>
#include <WiFiUdp.h>
#include <HTTPClient.h>
#include <Adafruit_GFX.h>           // https://github.com/adafruit/Adafruit-GFX-Library
#include "Adafruit_LEDBackpack.h"   // https://github.com/adafruit/Adafruit_LED_Backpack
#include <ArduinoJson.h>            // https://github.com/bblanchon/ArduinoJson
#include "InstagramStats.h"         // https://github.com/witnessmenow/arduino-instagram-stats
#include "JsonStreamingParser.h"    // https://github.com/squix78/json-streaming-parser
#include <WiFiClientSecure.h>

#define NTP_OFFSET  +2  * 60 * 60       // segundos
#define NTP_INTERVAL 60 * 1000          // milissegundos
#define NTP_ADDRESS  "0.pool.ntp.org"   // Url NTP
#define SDA_PIN 5                       // GPIO5 -> SDA
#define SCL_PIN 4                       // GPIO4 -> SCL
#define SSD_ADDRESS 0x3c                // i2c

char string[25];

const char* ssid       = "Your-WIFI-SSID";
const char* password   = "Your-WIFI-Password";

String formattedTime;
String ipStr;
String text;

WiFiUDP ntpUDP;
NTPClient timeClient(ntpUDP, NTP_ADDRESS, NTP_OFFSET, NTP_INTERVAL);
SSD1306  display(SSD_ADDRESS, SDA_PIN, SCL_PIN);

WiFiClientSecure secureClient;
WiFiClient client;

InstagramStats instaStats(secureClient);

String userName = "Your-INSTAGRAM-username";
String stats;

unsigned long api_delay = 1 * 60000; //time between api requests (1mins)
unsigned long api_due_time;

void setup() 
{
  Serial.begin(115200);
  Serial.println("");
  Serial.println("ESP32 Instagram Stats");
  Serial.println("");

  display.init();
//  display.flipScreenVertically();
  display.setTextAlignment(TEXT_ALIGN_LEFT);
  display.setFont(ArialMT_Plain_10);
  
  connectWiFi();
  timeClient.begin();
  InstagramUserStats response = instaStats.getUserStats(userName);
  text = (String) response.followedByCount;
}

void loop() 
{
  timeClient.update();
  formattedTime = timeClient.getFormattedTime();
  Serial.print("TIME: ");
  Serial.println(formattedTime);
  if(timeClient.getMinutes() == 00)   //updates every hour.
  {
    Serial.print("The number of Instagram followers of user " + userName + " is: ");
    InstagramUserStats response = instaStats.getUserStats(userName);
    Serial.println(response.followedByCount);
    text = (String) response.followedByCount;
  }
  display.clear();
  display.setFont(ArialMT_Plain_24);
  display.drawString(17, 0,  formattedTime);
  display.setFont(ArialMT_Plain_10);
  display.drawString(0, 25,  "Instagram: ");
  display.drawString(60, 25,  userName);
  display.setFont(ArialMT_Plain_24);
  display.drawString(40, 35,  text);
  display.display();
  delay(1000);
}

void connectWiFi(void)
{
  int i;
  Serial.println();
  Serial.print("Connecting to: ");
  Serial.println(ssid);
  display.drawString(2, 10, "Connected to: ");
  display.drawString(1, 25, String(ssid));
  display.display();
  WiFi.begin(ssid, password);
  while ((WiFi.status() != WL_CONNECTED) && i < 100)
  {
    i ++;
    delay(500);
    Serial.print(".");
    display.drawString((3 + i * 2), 35, "." );
    display.display();
  }
  if ( WiFi.status() == WL_CONNECTED)
  {
    IPAddress ip = WiFi.localIP();
    ipStr = String(ip[0]) + '.' + String(ip[1]) + '.' + String(ip[2]) + '.' + String(ip[3]);
    display.clear();
    display.setFont(ArialMT_Plain_16);
    display.drawString(0, 0, "WiFi Connected!");
    display.drawString(0, 25, ipStr);
    Serial.println("");
    Serial.println("WiFi Connedcted.");
    Serial.println("IP address: ");
    Serial.println(WiFi.localIP());
    Serial.print("Subnet Mask: ");
    Serial.println(WiFi.subnetMask());
    Serial.print("Gateway: ");
    Serial.println(WiFi.gatewayIP());
    display.display();
  }
  else
  {
    ipStr = "NOT CONNECTED";
    display.clear();
    display.setFont(ArialMT_Plain_16);
    display.drawString(0, 0,  "WiFi not connected.");
    display.drawString(0, 25, ipStr);
    Serial.println("");
    Serial.println("WiFi not connected.");
    display.display();
  }
  delay(1000);
}
```
## Follow up Problem Statement
- Try displaying twitter tweets on OLED display or the current song you are listening to in spotify
## References
- https://www.hackster.io/user742153/instagram-followers-counter-with-esp32-and-oled-display-a143ad
- https://github.com/andrei7c4/espspotifydisplay/
- https://hackaday.io/project/25359-esp8266-twitter-client
