# Telegram - ESP32 Motion Detection

This tutorial shows how to send notifications to your Telegram account when the ESP32 custom development board's detects motion. As long as you have access to the internet in your smartphone, you’ll be notified no matter where you are. The board will be programmed using Arduino IDE. This is a simple project, but shows how you can use Telegram in your IoT and Home Automation projects. The idea is to apply the concepts learned in your own projects.

## Instructions
### Prerequisites
* Telegram App on your Device           
* Arduino IDE
* Download the [Universal Telegram Bot Library](https://github.com/witnessmenow/Universal-Arduino-Telegram-Bot/archive/master.zip) and add this into your Arduino IDE   
###### Note: Don’t install the library through the Arduino Library Manager because it might install a deprecated version             
* Install the **ArduinoJson** library from the Library Manager in Arduino IDE         
#### Creating a Telegram Bot:
* Open the Telegram App and Search for **BotFather** and Open it (or) open this [link](https://t.me/botfather) on your device         
* Click the START button and Type **/newbot** and send to create your bot       
* Give it a name and Username         
* You will receive a message with a link to access the bot and the bot token. **Save the bot token**. You’ll need this for the Custom Dev Board (CDB) to interact with the bot     
###### Important: Anyone that knows your bot username can interact with it. To make sure that we ignore messages that are not from our Telegram account (or any authorized users), you can get your Telegram User ID. Then, when your telegram bot receives a message, CDB can check whether the sender ID corresponds to your User ID and handle the message or ignore it
* To do this, Search for **IDBot** and Open it (or) open this [link](https://t.me/myidbot) on your device          
* Click START and type **/getid**. You will then get your User ID. Save it.
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
## Demonstration:
After the code uploaded on the board, it will send a message to your Telegram account at the bootup: “Bot started up”. Then, move your hand in front of the PIR motion sensor and check that you’ve received the motion detected notification.
![](https://i1.wp.com/randomnerdtutorials.com/wp-content/uploads/2020/06/Motion-Detected-Telegram-Notification.png?w=352&quality=100&strip=all&ssl=1)
![](https://i2.wp.com/randomnerdtutorials.com/wp-content/uploads/2020/06/ESP32-Telegram-Motion-Detected-Serial-Monitor-Demonstration.png?w=669&quality=100&strip=all&ssl=1)
## Links:

- [Detailed Instructions](https://randomnerdtutorials.com/telegram-esp32-motion-detection-arduino/)

- [Universal Telegram Bot Library](https://github.com/witnessmenow/Universal-Arduino-Telegram-Bot/archive/master.zip)

- [ArduinoJson Library](https://github.com/bblanchon/ArduinoJson)

## Follow up Problem Statement:

Detect motion using the inbuilt IMU sensor instead of PIR motion sensor(Hint: This was discussed in one of the session)
