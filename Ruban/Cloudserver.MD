# Cloud Server
## Description
In this simple mini project, we are going to pull down data from the board and store it in a cloud server.
## Concept
The concept is really simple. Various data inputs from the board (Hall effect value, temperature, acceleration and angular velocities) are obtained and stored in a cloud storage and the data is plotted. 
## Prerequisites
Arduino IDE 
## Components
* Electronics club custom development board 
* Jumper wires
## Schematic
The following connections should be made:

* SDA of custom board - Pin 21 of custom board
* SCL of custom board - Pin 22 of custom board
## Code
**Note**:(Before reading the code, it is recommended to follow the reference link which is provided in the "References" section.)

    #ifdef __cplusplus
    extern "C" {
    #endif
    uint8_t temprature_sens_read();
    #ifdef __cplusplus
    }
    #endif
    uint8_t temprature_sens_read();
    #include <WiFi.h>
    String apiKey = "   ";                  //  Enter your Write API key from ThingSpeak
    const char *ssid =  "   ";                                    // replace with your wifi ssid and wpa2 key
    const char *pass =  "   ";
    const char* server = "api.thingspeak.com";
    WiFiClient client;
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
         Serial.print(".");
     }
     Serial.println("");
     Serial.println("WiFi connected");
 
    }
    void loop() 
    {
  
    int h = 0;
    float t =0;

    h = hallRead();
    t = ((temprature_sens_read()-32)/1.8);                //changing temperature parameter to celsius
    if (client.connect(server,80))                                 //   "184.106.153.149" or api.thingspeak.com
    {  
                            
     String postStr = apiKey;
     postStr +="&field1=";
     postStr += String(h);
     postStr += "&field2=";
     postStr += String(t);
     postStr += "\r\n\r\n";

     client.print("POST /update HTTP/1.1\n");
     client.print("Host: api.thingspeak.com\n");
     client.print("Connection: close\n");
     client.print("X-THINGSPEAKAPIKEY: "+apiKey+"\n");
     client.print("Content-Type: application/x-www-form-urlencoded\n");
     client.print("Content-Length: ");
     client.print(postStr.length());
     client.print("\n\n");
     client.print(postStr);
 
     Serial.print("Hall: ");
     Serial.println(h);
     Serial.print("Temperature:");
     Serial.print(t);
     Serial.println(" C");
                             
     Serial.println("%. Send to Thingspeak.");
    }
    client.stop();
    Serial.println("Waiting...");
    delay(10000);
    }
## Follow up Problem Statement
As a follow up problem statement, try controlling certain outputs based on the data you obtain. For example, if the temperature graph you plot is having standard deviation below a certain threshold, you should switch on an LED. Else, no need of the LED. You can think of your own problem statements and try using the cloud server to your advantage. Make the required changes to the code so that you can also obtain the IMU sensor values. Try using the Adafruit_MPU6050 library to obtain the filtered data (Taught in the IMU web server session).  
## References
[Data to cloud from ESP32](https://iotdesignpro.com/projects/how-to-send-data-to-thingspeak-cloud-using-esp32)

