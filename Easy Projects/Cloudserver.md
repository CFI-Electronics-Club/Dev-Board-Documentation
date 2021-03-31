# Cloud Server
## Description
In this simple mini project, we are going to pull down data from the board and store it in a cloud server.
## Concept
The concept is really simple. The temperature and hall effect sensor values are obtained and stored in a cloud storage and the data is plotted. 
## Components required
* Electronics club custom development board 
* Jumper wires
## Schematic
The following connections should be made:

* SDA of custom board <-> Pin 21 of custom board
* SCL of custom board <-> Pin 22 of custom board
![temp](https://github.com/CFI-Electronics-Club/Dev-Board-Documentation/blob/main/Ruban/images/cloud.jpg)
## Code
```
#ifdef __cplusplus
extern "C" {
#endif
uint8_t temprature_sens_read();
#ifdef __cplusplus
}
#endif
uint8_t temprature_sens_read();

#include <WiFi.h>
#include "secrets.h"
#include "ThingSpeak.h" // always include thingspeak header file after other header files and custom macros

char ssid[] = SECRET_SSID;   // your network SSID (name) 
char pass[] = SECRET_PASS;   // your network password
WiFiClient  client;

unsigned long myChannelNumber = SECRET_CH_ID;
const char * myWriteAPIKey = SECRET_WRITE_APIKEY;

int h = hallRead();
float t = ((temprature_sens_read()-32)/1.8);                //changing temperature parameter to celsius
String myStatus = "";

void setup() {
  Serial.begin(115200);  //Initialize serial
  while (!Serial) {
    ; // wait for serial port to connect. Needed for Leonardo native USB port only
  }
  
  WiFi.mode(WIFI_STA);   
  ThingSpeak.begin(client);  // Initialize ThingSpeak
}

void loop() {

  // Connect or reconnect to WiFi
  if(WiFi.status() != WL_CONNECTED){
    Serial.print("Attempting to connect to SSID: ");
    Serial.println(SECRET_SSID);
    while(WiFi.status() != WL_CONNECTED){
      WiFi.begin(ssid, pass);  // Connect to WPA/WPA2 network. Change this line if using open or WEP network
      Serial.print(".");
      delay(5000);     
    } 
    Serial.println("\nConnected.");
  }

  // set the fields with the values
  ThingSpeak.setField(1, h);
  ThingSpeak.setField(2, t);
 
  //set the status
  myStatus = String("Values are updated");
  ThingSpeak.setStatus(myStatus);
  
  // write to the ThingSpeak channel
  int x = ThingSpeak.writeFields(myChannelNumber, myWriteAPIKey);
  if(x == 200){
    Serial.println("Channel update successful.");
    Serial.println(String("Hall: ")+h+String(" Temperature: ")+t);
  }
  else{
    Serial.println("Problem updating channel. HTTP error code " + String(x));
  }

   h = hallRead();
   t = ((temprature_sens_read()-32)/1.8);
  delay(20000); // Wait 20 seconds to update the channel again
}
```
## Follow up Problem Statement
As a follow up problem statement, try controlling certain outputs based on the data you obtain. For example, if the temperature graph you plot is having standard deviation below a certain threshold, you should switch on an LED. Else, no need of the LED. You can think of your own problem statements and try using the cloud server to your advantage. Make the required changes to the code so that you can also obtain the IMU sensor values. Try using the Adafruit_MPU6050 library to obtain the filtered data (Taught in the IMU web server session).  
## References
[Data to cloud from ESP32](https://iotdesignpro.com/projects/how-to-send-data-to-thingspeak-cloud-using-esp32)

