# Images on Web server
## Description
In this project, we will be displaying images on a webserver using the custom board. The images will be stored in the custom board's filesystem and can also be transmitted to other devices through wireless communication.
## Concept 
The concept is very simple. We store the images in the custom board's memory and display it on a webserver using wireless communication. This project will introduce you to the libraries that deal with wireless communication, asynchronous web server and ESP32 filesystem.
## Prerequisites
* Arduino IDE
* HTML (Not a compulsory prereq)
## Components
* Electronics club custom development board
* OLED display (For the follow up PS)
* Jumper wires (For the follow up PS)
## Schematic
As for the main problem statement, there is no external connections required. Just connecting the custom board to the laptop and uploading the code will do the job. However, for the follow up PS, the custom board must be interfaced with the OLED display. The connections which should be made are:

* VCC of custom board <-> VDD of OLED
* GND of custom board <-> GND of OLED
* SCL of custom board <-> SCK of OLED
* SDA of custom board <-> SDA of OLED

The schematic is given below:

![temp](https://github.com/CFI-Electronics-Club/Dev-Board-Documentation/blob/main/Ruban/images/imagewebserver.jpg)
## Code
    #include <WiFi.h>
    #include <ESPAsyncWebServer.h>
    #include <SPIFFS.h>
    
    // Replace with your network credentials
    const char* ssid = "REPLACE_WITH_YOUR_SSID";
    const char* password = "REPLACE_WITH_YOUR_PASSWORD";

    // Create AsyncWebServer object on port 80
    AsyncWebServer server(80);

    const char index_html[] PROGMEM = R"rawliteral(
    <!DOCTYPE HTML><html>
    <head>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <h2>ESP Image Web Server</h2>
      <img src="sun">
      <img src="sun-cloud">
      <img src="cloud">
      <img src="rain">
      <img src="storm">
      <img src="snow">
    </body>  
    </html>)rawliteral";


    void setup(){
      // Serial port for debugging purposes
      Serial.begin(115200);
  
      // Connect to Wi-Fi
      WiFi.begin(ssid, password);
      while (WiFi.status() != WL_CONNECTED) {
        delay(1000);
        Serial.println("Connecting to WiFi..");
      }
      if(!SPIFFS.begin()){
            Serial.println("An Error has occurred while mounting SPIFFS");
            return;
      }

      // Print ESP32 Local IP Address
      Serial.println(WiFi.localIP());

      // Route for root / web page

      server.on("/", HTTP_GET, [](AsyncWebServerRequest *request){
        request->send_P(200, "text/html", index_html);
      });
  
      server.on("/sun", HTTP_GET, [](AsyncWebServerRequest *request){
        request->send(SPIFFS, "/sun.png", "image/png");
      });
      server.on("/sun-cloud", HTTP_GET, [](AsyncWebServerRequest *request){
        request->send(SPIFFS, "/sun-cloud.png", "image/png");
      });
      server.on("/cloud", HTTP_GET, [](AsyncWebServerRequest *request){
        request->send(SPIFFS, "/cloud.png", "image/png");
      });
      server.on("/rain", HTTP_GET, [](AsyncWebServerRequest *request){
        request->send(SPIFFS, "/rain.png", "image/png");
      });
      server.on("/storm", HTTP_GET, [](AsyncWebServerRequest *request){
        request->send(SPIFFS, "/storm.png", "image/png");
      });
      server.on("/snow", HTTP_GET, [](AsyncWebServerRequest *request){
        request->send(SPIFFS, "/snow.png", "image/png");
      });
      // Start server
      server.begin();
    }
 
    void loop(){
  
    }
    
All you need to do is upload the code in the board, run it and obtain the IP address displayed on the serial monitor. The images will be displayed on this website irrespective of what device you use (It need not be the same laptop). However, ensure that the device is connected to the same WiFi to which the custom board is connected to. (It will be the WiFi corresponding to the network credentials you entered in the code).
## Follow up Problem Statement
As a follow up problem statement try displaying the images on an OLED display. This [link](https://randomnerdtutorials.com/esp32-ssd1306-oled-display-arduino-ide/) will help you to get started with it. 
## References
[Images displayed on a web server](https://randomnerdtutorials.com/display-images-esp32-esp8266-web-server/)
