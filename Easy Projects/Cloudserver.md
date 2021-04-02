# Cloud Server
## Description
In this simple mini project, we are going to pull down data from the board and store it in a cloud server.
## Concept
The concept is really simple. The temperature and hall effect sensor values are obtained and stored in a cloud storage and the data is plotted. 
## Components required
* Electronics club custom development board 
* Jumper wires
## Libraries required
* WiFi.h
* ThingSpeak.h
* secrets.h

WiFi.h is a standard Arduino library header file. However, ThingSpeak.h is different and it needs to be installed first. Follow the steps given below:

1. Open you Arduino IDE and choose Sketch/Include Library/Manage Libraries. Click the ThingSpeak Library (by MathWorks) from the list, and click the Install button.
2. Create a .ino file which you are going to use for this code. The sketch folder will be automatically generated and the .ino file will be present in it.
3. Create secrets.h and store it in the sketch folder. Also, write the following code into it:
```ino
// Use this file to store all of the private credentials 
// and connection details

#define SECRET_SSID "Temp"		// replace Temp with your WiFi network name
#define SECRET_PASS "Temp"	// replace Temp with your WiFi password

#define SECRET_CH_ID Temp			// replace Temp with your channel number
#define SECRET_WRITE_APIKEY "Temp"   // replace Temp with your channel write API Key
```
## Schematic
There are no external connections required. Just connecting the Dev board to the laptop using the USB cable is enough. However, following connections should be made if you also want to obtain the MPU6050 IMU values:

* SDA of custom board <-> Pin 21 of custom board
* SCL of custom board <-> Pin 22 of custom board
![temp](https://github.com/CFI-Electronics-Club/Dev-Board-Documentation/blob/main/Easy%20Projects/Images/cloud.jpg)
## Code
### Libraries are included
```ino
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
```
### Network details are accessed
```ino
char ssid[] = SECRET_SSID;   
char pass[] = SECRET_PASS;   
WiFiClient  client;

unsigned long myChannelNumber = SECRET_CH_ID;
const char * myWriteAPIKey = SECRET_WRITE_APIKEY;
```
### Useful variables are declared
```ino
int h = hallRead();
float t = ((temprature_sens_read()-32)/1.8);                //changing temperature parameter to celsius
String myStatus = "";
```
### Network connection is set up
```ino
void setup() {
  Serial.begin(115200);  //Initialize serial
  while (!Serial) {
    ; // wait for serial port to connect. Needed for Leonardo native USB port only
  }
  
  WiFi.mode(WIFI_STA);   
  ThingSpeak.begin(client);  // Initialize ThingSpeak
}
```
### Values are sent to cloud and updated
```ino
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
  ThingSpeak.setField(2, t);              //Field values are stored

 
  //set the status
  myStatus = String("Values are updated");
  ThingSpeak.setStatus(myStatus);              //Status is written
  
  // write to the ThingSpeak channel
  int x = ThingSpeak.writeFields(myChannelNumber, myWriteAPIKey);            //Values are sent to cloud server
  if(x == 200){
    Serial.println("Channel update successful.");
    Serial.println(String("Hall: ")+h+String(" Temperature: ")+t);
  }
  else{
    Serial.println("Problem updating channel. HTTP error code " + String(x));
  }

   h = hallRead();
   t = ((temprature_sens_read()-32)/1.8);                           //Input values are read at next time instant
  delay(20000); // Wait 20 seconds to update the channel again
}
```

## Full code
```ino
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

char ssid[] = SECRET_SSID;   
char pass[] = SECRET_PASS;   
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
  ThingSpeak.setField(2, t);              //Field values are stored

 
  //set the status
  myStatus = String("Values are updated");
  ThingSpeak.setStatus(myStatus);              //Status is written
  
  // write to the ThingSpeak channel
  int x = ThingSpeak.writeFields(myChannelNumber, myWriteAPIKey);            //Values are sent to cloud server
  if(x == 200){
    Serial.println("Channel update successful.");
    Serial.println(String("Hall: ")+h+String(" Temperature: ")+t);
  }
  else{
    Serial.println("Problem updating channel. HTTP error code " + String(x));
  }

   h = hallRead();
   t = ((temprature_sens_read()-32)/1.8);                           //Input values are read at next time instant
  delay(20000); // Wait 20 seconds to update the channel again
}
```
## Follow up Problem Statement
As a follow up problem statement, try controlling certain outputs based on the data you obtain. For example, if the temperature graph you plot is having standard deviation below a certain threshold, you should switch on an LED. Else, no need of the LED. You can think of your own problem statements and try using the cloud server to your advantage. Make the required changes to the code so that you can also obtain the IMU sensor values. Try using the Adafruit_MPU6050 library to obtain the filtered data (Taught in the IMU web server session).  
## References
1. [Tutorial: Data to cloud from ESP32](https://iotdesignpro.com/projects/how-to-send-data-to-thingspeak-cloud-using-esp32)
2. [ThingSpeak library documentation example](https://github.com/mathworks/thingspeak-arduino/blob/master/examples/ESP32/WriteMultipleFields/WriteMultipleFields.ino)
