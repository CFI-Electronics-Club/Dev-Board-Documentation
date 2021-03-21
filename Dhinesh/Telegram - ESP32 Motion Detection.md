# Telegram - ESP32 Motion Detection

This tutorial shows how to send notifications to your Telegram account when the ESP32 custom development board's detects motion. As long as you have access to the internet in your smartphone, you’ll be notified no matter where you are. The board will be programmed using Arduino IDE. This is a simple project, but shows how you can use Telegram in your IoT and Home Automation projects. The idea is to apply the concepts learned in your own projects.

## Instructions:

Install Telegram and create telegram bot. Also install Universal Telegram Bot and ArduinoJson libraries in Arduino IDE. Details regarding installation can be referred in the Links section below.

## Parts Required

- Electronics Club Custom development board
- Mini PIR motion sensor (AM312) or PIR motion sensor (HC-SR501)
- Jumper wires
- Breadboard

## Schematic Diagram

![schematic](https://i2.wp.com/randomnerdtutorials.com/wp-content/uploads/2020/07/ESP32-PIR-Motion-Sensor-Wiring-Diagram.png?resize=1024%2C649&quality=100&strip=all&ssl=1)

## Code

```
#include <WiFi.h>
#include <WiFiClientSecure.h>
#include <UniversalTelegramBot.h>
#include <ArduinoJson.h>

// Replace with your network credentials
const char* ssid = "REPLACE_WITH_YOUR_SSID";
const char* password = "REPLACE_WITH_YOUR_PASSWORD";

// Initialize Telegram BOT
#define BOTtoken "XXXXXXXXXX:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"  // your Bot Token (Get from Botfather)

// Use @myidbot to find out the chat ID of an individual or a group
// Also note that you need to click "start" on a bot before it can
// message you
#define CHAT_ID "XXXXXXXXXX"

WiFiClientSecure client;
UniversalTelegramBot bot(BOTtoken, client);

const int motionSensor = 27; // PIR Motion Sensor
bool motionDetected = false;

// Indicates when motion is detected
void IRAM_ATTR detectsMovement() {
  //Serial.println("MOTION DETECTED!!!");
  motionDetected = true;
}

void setup() {
  Serial.begin(115200);

  // PIR Motion Sensor mode INPUT_PULLUP
  pinMode(motionSensor, INPUT_PULLUP);
  // Set motionSensor pin as interrupt, assign interrupt function and set RISING mode
  attachInterrupt(digitalPinToInterrupt(motionSensor), detectsMovement, RISING);

  // Attempt to connect to Wifi network:
  Serial.print("Connecting Wifi: ");
  Serial.println(ssid);

  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    Serial.print(".");
    delay(500);
  }

  Serial.println("");
  Serial.println("WiFi connected");
  Serial.print("IP address: ");
  Serial.println(WiFi.localIP());

  bot.sendMessage(CHAT_ID, "Bot started up", "");
}

void loop() {
  if(motionDetected){
    bot.sendMessage(CHAT_ID, "Motion detected!!", "");
    Serial.println("Motion Detected");
    motionDetected = false;
  }
}

```

## Links:

[Detailed Instructions](https://randomnerdtutorials.com/telegram-esp32-motion-detection-arduino/)

[Universal Telegram Bot Library](https://github.com/witnessmenow/Universal-Arduino-Telegram-Bot/archive/master.zip)

[ArduinoJson Library](https://github.com/bblanchon/ArduinoJson)

## Follow up Problem Statement:

Detect motion using the inbuilt IMU sensor instead of PIR motion sensor(Hint: This was discussed in one of the session)
