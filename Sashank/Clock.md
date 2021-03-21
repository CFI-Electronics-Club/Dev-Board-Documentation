# Internet Clock
## Description
We will be making a clock displayed in oled using time extracted online accessing WIFI via the ESP32
## Concept (Optional)
### NTP:
Network Time Protocol (NTP) is a networking protocol used for synchronization of time between systems and Data networks.The NTP framework depends on Internet Time servers. NTP has algorithms to precisely adjust the time of day.NTP servers have software which send the clock's time of day to client computers using UDPport 123. So here in this project, we are getting time from NTP server using ESP32 and showing it on OLED display.

### Oled:
we use SPI mode to connect our 128×64 OLED display Module (SSD1306) to ESP32.
## Prerequisites
Understand WIFI usage in ESP32 with NTP,SPI protocols.They are pretty easy to learn, and can be learnt while doing this project.
## Components
ESP32
128*64 OLED display
Breadboard
Male-female wires
## Schematic
![circuit](https://circuitdigest.com/sites/default/files/circuitdiagram_mic/Circuit-diagram-for-Internet-Clock-using-ESP32-and-OLED-Display.png)
## Code
```#include <WiFi.h>

#include <SPI.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#include <NTPClient.h>
#include <WiFiUdp.h>
const char* ssid     = "*******";
const char* password = "*********";

#define NTP_OFFSET  19800 // In seconds 
#define NTP_INTERVAL 60 * 1000    // In miliseconds
#define NTP_ADDRESS  "1.asia.pool.ntp.org"

WiFiUDP ntpUDP;
NTPClient timeClient(ntpUDP, NTP_ADDRESS, NTP_OFFSET, NTP_INTERVAL);

#define OLED_MOSI  23
#define OLED_CLK   18
#define OLED_DC    4
#define OLED_CS    5
#define OLED_RESET 2
Adafruit_SSD1306 display(OLED_MOSI, OLED_CLK, OLED_DC, OLED_RESET, OLED_CS);

void setup()
{
display.begin();
Serial.begin(9600);

Serial.println();
Serial.println();
Serial.print("Connecting to ");
Serial.println(ssid);
WiFi.begin(ssid, password);
while (WiFi.status() != WL_CONNECTED)
{
delay(500);
Serial.print(".");
}
Serial.println("");
Serial.println("WiFi connected.");
Serial.println("IP address: ");
Serial.println(WiFi.localIP());

timeClient.begin();

display.begin(SSD1306_SWITCHCAPVCC);

display.clearDisplay();
display.setTextColor(WHITE);
//display.startscrollright(0x00, 0x0F);
display.setTextSize(2);
//display.setCursor(0,0);
//display.print("  Internet ");
//display.println("  Clock ");
//display.display();
//delay(3000);
}

void loop()
{

timeClient.update();
String formattedTime = timeClient.getFormattedTime();

// Serial.println(formattedTime);

display.clearDisplay();
display.setTextSize(2);
display.setCursor(0, 0);
display.println(formattedTime);

display.display();   // write the buffer to the display
delay(10);
delay(100);

} ```
## Anything Extra (Debugging) (Optional)
...
## Follow up Problem Statement
...
## References
...
