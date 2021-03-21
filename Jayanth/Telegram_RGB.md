# Controlling GPIOs using Telegram
## Description
This project demonstrates how to control outputs of GPIOs using Telegram. As an example, we will control the internal RGB LED in the Custom Development Board using Telegram messages
## Prerequisites
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
## Components
* Electronics Club Custom Development Board CDB
## Schematic
R of CDB --> Any GPIO (12 in Code)                   
G of CDB --> Any GPIO (14 in Code)              
B of CDB --> Any GPIO (27 in Code)            
![](Images/Telegram_RGB.png)
## Code
```
#include <WiFi.h>
#include <WiFiClientSecure.h>
#include <UniversalTelegramBot.h>   // Universal Telegram Bot Library written by Brian Lough: https://github.com/witnessmenow/Universal-Arduino-Telegram-Bot
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

// Checks for new messages every 1 second.
int botRequestDelay = 1000;
unsigned long lastTimeBotRan;

const int ledPinR = 12;
const int ledPinG = 14;
const int ledPinB = 27;
bool ledStateR = LOW;
bool ledStateG = LOW;
bool ledStateB = LOW;

// Handle what happens when you receive new messages
void handleNewMessages(int numNewMessages) {
  Serial.println("handleNewMessages");
  Serial.println(String(numNewMessages));

  for (int i=0; i<numNewMessages; i++) {
    // Chat id of the requester
    String chat_id = String(bot.messages[i].chat_id);
    if (chat_id != CHAT_ID){
      bot.sendMessage(chat_id, "Unauthorized user", "");
      continue;
    }
    
    // Print the received message
    String text = bot.messages[i].text;
    Serial.println(text);

    String from_name = bot.messages[i].from_name;

    if (text == "/start") {
      String welcome = "Welcome, " + from_name + ".\n";
      welcome += "Use the following commands to control your outputs.\n\n";
      welcome += "/redled_on to turn RED LED ON \n";
      welcome += "/redled_off to turn RED LED OFF \n";
      welcome += "/statered to request current RED LED state \n";
      welcome += "/greenled_on to turn GREEN LED ON \n";
      welcome += "/greenled_off to turn GREEN LED OFF \n";
      welcome += "/stategreen to request current GREEN LED state \n";
      welcome += "/blueled_on to turn BLUE LED ON \n";
      welcome += "/blueled_off to turn BLUE LED OFF \n";
      welcome += "/stateblue to request current BLUE LED state \n";
      bot.sendMessage(chat_id, welcome, "");
    }

    if (text == "/redled_on") {
      bot.sendMessage(chat_id, "RED LED state set to ON", "");
      ledStateR = HIGH;
      digitalWrite(ledPinR, ledStateR);
    }
    
    if (text == "/redled_off") {
      bot.sendMessage(chat_id, "RED LED state set to OFF", "");
      ledStateR = LOW;
      digitalWrite(ledPinR, ledStateR);
    }
    
    if (text == "/statered") {
      if (digitalRead(ledPinR)){
        bot.sendMessage(chat_id, "RED LED is ON", "");
      }
      else{
        bot.sendMessage(chat_id, " RED LED is OFF", "");
      }
    }
    
    if (text == "/greenled_on") {
      bot.sendMessage(chat_id, "GREEN LED state set to ON", "");
      ledStateG = HIGH;
      digitalWrite(ledPinG, ledStateG);
    }
    
    if (text == "/greenled_off") {
      bot.sendMessage(chat_id, "GREEN LED state set to OFF", "");
      ledStateG = LOW;
      digitalWrite(ledPinG, ledStateG);
    }
    
    if (text == "/stategreen") {
      if (digitalRead(ledPinG)){
        bot.sendMessage(chat_id, "GREEN LED is ON", "");
      }
      else{
        bot.sendMessage(chat_id, " GREEN LED is OFF", "");
      }
    }
    
    if (text == "/blueled_on") {
      bot.sendMessage(chat_id, "BLUE LED state set to ON", "");
      ledStateB = HIGH;
      digitalWrite(ledPinB, ledStateB);
    }
    
    if (text == "/blueled_off") {
      bot.sendMessage(chat_id, "BLUE LED state set to OFF", "");
      ledStateB = LOW;
      digitalWrite(ledPinB, ledStateB);
    }
    
    if (text == "/stateblue") {
      if (digitalRead(ledPinB)){
        bot.sendMessage(chat_id, "BLUE LED is ON", "");
      }
      else{
        bot.sendMessage(chat_id, " BLUE LED is OFF", "");
      }
    }
  }
}

void setup() {
  Serial.begin(115200);
  pinMode(ledPinR, OUTPUT);
  pinMode(ledPinG, OUTPUT);
  pinMode(ledPinB, OUTPUT);
  digitalWrite(ledPinR, ledStateR);
  digitalWrite(ledPinG, ledStateG);
  digitalWrite(ledPinB, ledStateB);
  
  // Connect to Wi-Fi
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi..");
  }
  // Print ESP32 Local IP Address
  Serial.println(WiFi.localIP());
}

void loop() {
  if (millis() > lastTimeBotRan + botRequestDelay)  {
    int numNewMessages = bot.getUpdates(bot.last_message_received + 1);

    while(numNewMessages) {
      Serial.println("got response");
      handleNewMessages(numNewMessages);
      numNewMessages = bot.getUpdates(bot.last_message_received + 1);
    }
    lastTimeBotRan = millis();
  }
}
```
## Follow-up Problem Statement
Try to integrate any sensor(s) of your choice and get their readings through the Telegram Bot. Also think about any other features you can implement by just adding to/modifying the above given code
## References
[Telegrmam: Contol of ESP32/ESP8266 Outputs](https://randomnerdtutorials.com/telegram-control-esp32-esp8266-nodemcu-outputs/)
