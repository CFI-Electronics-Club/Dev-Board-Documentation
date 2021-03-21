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
