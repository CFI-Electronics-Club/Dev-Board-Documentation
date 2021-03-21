# ESP32 GPS vehicle tracker
## Description
ESP32 based GPS mudule is used to track the location of vehicle and it can be monitored using the blynk app from anywhere in the world. Just that you need internet :p
## Concept (Optional)
GPS stands for Global Positioning System, which is a worldwide radio-navigation system. To track the location of the device, the GPS tracking system uses the Global Navigation Satellite System (GNSS) Network. This network consists of a range of satellites that uses microwave signals to transmit the data which will be received by the GPS receiver module.
## Prerequisites
### Blynk APP usage (basics put here):
1.Download the Blynk app from the Google Play Store and create a new account or Login into your existing account. 
2.Now click on ‘New Project’ to start a new project.
3.Now give a name for your project. Select ‘ESP32 Dev Board’ in the CHOOSE DEVICE option and ‘Wi-Fi’ in CONNECTION TYPE. Then click on ‘Create.’
4.After this, Blynk will send an Authorization to the registered Email id. Note down the Auth Token Code. It will be used in the Program. 
5.Now in the next window, click on the “+” sign to add a widget. Inside the Widget box, select the ‘Map’ widget. 
6.After this, click on the MAP widget and select virtual pin ‘V0’ as INPUT.
7.With this final step, you are ready to use your app. By pressing the ‘Play’ button, you can switch your app from EDIT mode to PLAY mode where you can interact with the hardware. 
## Components
1. ESP32
2. GPS Module(ESP32 GPS NEO 6M)
3. OLED Display Module
4. Jumper Wires
5. Breadboard
6. Blynk App
## Schematic
![circuit](https://github.com/CFI-Electronics-Club/Dev-Board-Documentation/blob/main/Sashank/gps.fzz_%20-%20Fritzing%20-%20%5BBreadboard%20View%5D%2021-03-2021%205.41.30%20PM.png)

Here we are interfacing the ESP32 with GPS Module and OLED Display. Vcc and GND pin of GPS Module is connected to 3.3V and GND of ESP32 while the RX and TX pins are connected to TX2 and RX2 pins of ESP32. I2C mode is used to connect the OLED display Module (SSD1306) with ESP32.
## Code
```
#include <TinyGPS++.h>
#include <HardwareSerial.h>
#include <WiFi.h>
#include <Wire.h>               // Only needed for Arduino 1.6.5 and earlier
#include<SH1106.h> 
#include <BlynkSimpleEsp32.h>
float latitude , longitude;
String  lat_str , lng_str;
const char *ssid =  "Galaxy-M20";     // Enter your WiFi Name
const char *pass =  "ac312129"; // Enter your WiFi Password
char auth[] = "loPrSaL0eQFY9clcQ518R1SmYsRVC0eV"; 
WidgetMap myMap(V0); 
SH1106 display(0x3c, 21, 22);
WiFiClient client;
TinyGPSPlus gps;
HardwareSerial SerialGPS(1);
void setup()
{
  Serial.begin(115200);
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
  SerialGPS.begin(9600, SERIAL_8N1, 16, 17);
  Blynk.begin(auth, ssid, pass);
  Blynk.virtualWrite(V0, "clr"); 
}
void loop()
{
  while (SerialGPS.available() > 0) {
    if (gps.encode(SerialGPS.read()))
    {
      if (gps.location.isValid())
      {
        latitude = gps.location.lat();
        lat_str = String(latitude , 6);
        longitude = gps.location.lng();
        lng_str = String(longitude , 6);
        Serial.print("Latitude = ");
        Serial.println(lat_str);
        Serial.print("Longitude = ");
        Serial.println(lng_str);
        display.clear();
        display.setTextAlignment(TEXT_ALIGN_LEFT);
        display.setFont(ArialMT_Plain_16);
        display.drawString(0, 23, "Lat:");
        display.drawString(45, 23, lat_str);
        display.drawString(0, 38, "Lng:");
        display.drawString(45, 38, lng_str);
        Blynk.virtualWrite(V0, 1, latitude, longitude, "Location");
        display.display();
      }
     delay(1000);
     Serial.println();  
    }
  }  
  Blynk.run();
}
```
## References
This Video should help: https://youtu.be/ga_6qcu5LmE
