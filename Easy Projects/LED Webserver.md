# Controlling LEDs - ESP32 Webserver
## Description
The aim of this project is to create a web server using ESP32 and Arduino IDE to control the inbuilt R, G, B LEDs of Custom Dev Board

## Concept
Web server is nothing but the location where our website files stores and Web server process these files to clients based on the request of a client. HTTP, hypertext transfer
control is a protocol is used to transfer data between the web client and Web server. 
After receiving HTTP GET request, the server responds using the web pages. If the Server is down or not available a error 404 message is displayed on the web browser.

* To access ESP32 web pages, its IP address which is assigned to it when it connects a to the internet is required.
* After getting the IP address on serial monitor, paste it in a web browser to get the response from the CUstom Dev Board's host server.
## Prerequisites
* Arduino IDE

## Components
* Dev Board
* Jumpers

## Schematic
* Connect Pin R -> Pin 15
* Connect Pin G -> Pin 22
* Connect Pin B -> Pin 23

## Code Explanation

* WiFi module's library is called WiFi.h. Wifi function is used to connect to the network, and to send and recieve data from the client.
 #include directive will include this pre-installed library in the code.
* Replace the ___ with your WIFI NAME and WIFI PASSWORD. Same network that you want the board to be connected.
* server() will set the port for the WiFiServer at 80 (default)
```
#include <WiFi.h>

const char* WIFI_NAME= "___"; 
const char* WIFI_PASSWORD = "___"; 
WiFiServer server(80);
```
Initially LEDs are set to a state off and GPIO pins 15, 22, 23 are initialized to access the LEDs later in the code.
```
String header;

String LED_ONE_STATE = "off";
String LED_TWO_STATE = "off";
String LED_THREE_STATE = "off";


const int G_22 = 22;
const int B_23 = 23;
const int R_15 = 15;

```
Baud rate of ‘serial.begin’ function is initialized at the rate of 115200 bits per second. GPIO pins are set as digital output using pinMode()
function and digitalWrite() function is used to set them at a LOW state.
```
void setup() {
Serial.begin(115200);
pinMode(G_22, OUTPUT);
pinMode(B_23, OUTPUT);
pinMode(R_15, OUTPUT);

digitalWrite(G_22, LOW);
digitalWrite(B_23, LOW);
digitalWrite(R_15, LOW);
```
The code at this part is pretty much self-explanatory. This is written to connect it to WiFi
```
Serial.print("Connecting to ");
Serial.println(WIFI_NAME);
WiFi.begin(WIFI_NAME, WIFI_PASSWORD);
while (WiFi.status() != WL_CONNECTED) {
delay(1000);
Serial.print("Trying to connect to Wifi Network");
}
Serial.println("");
Serial.println("Successfully connected to WiFi network");
Serial.println("IP address: ");
Serial.println(WiFi.localIP());
server.begin();
}
```
Main loop services the web page to the client and receives data through HTTP get request about the status of GPIO pins. 
If the client request is available, server.avaialble () function stores logical one value in variable client.

Now if the client request is available, These statements will start receiving data from the client and stores the data in header string 
and will continue receiving data until ‘\n’  is not found which means client has disconnected.

```
void loop(){
WiFiClient client = server.available(); 
if (client) { 
Serial.println("New Client is requesting web page"); 
String current_data_line = ""; 
while (client.connected()) { 
if (client.available()) { 
char new_byte = client.read(); 
Serial.write(new_byte); 
header += new_byte;
if (new_byte == '\n') { 
         
if (current_data_line.length() == 0) 
{
            
client.println("HTTP/1.1 200 OK");
client.println("Content-type:text/html");
client.println("Connection: close");
client.println();

```
This part of the loop involves multiple conditional statements to control the different LEDs.
```

if (header.indexOf("LED0=ON") != -1) 
{
Serial.println("G LED is ON");
LED_ONE_STATE = "on";
digitalWrite(G_22, HIGH);
} 
if (header.indexOf("LED0=OFF") != -1) 
{
Serial.println("G LED is OFF");
LED_ONE_STATE = "off";
digitalWrite(G_22, LOW);
} 
if (header.indexOf("LED1=ON") != -1)
{
Serial.println("B LED is ON");
LED_TWO_STATE = "on";
digitalWrite(B_23, HIGH);
}
if (header.indexOf("LED1=OFF") != -1) 
{
Serial.println("B LED is OFF");
LED_TWO_STATE = "off";
digitalWrite(B_23, LOW);
}
if (header.indexOf("LED2=ON") != -1) 
{
Serial.println("R LED is ON");
LED_THREE_STATE = "on";
digitalWrite(R_15, HIGH);
}
if(header.indexOf("LED2=OFF") != -1) {
Serial.println("R LED is OFF");
LED_THREE_STATE = "off";
digitalWrite(R_15, LOW);
}
```
Displaying HTML on web server
This is to display HTML and CSS code to the web browser. 
client.println () function is used to send HTML and CSS commands to the client who is accessing a web server through the IP address. 
```
            
client.println("<!DOCTYPE html><html>");
client.println("<head><meta name=\"viewport\" content=\"width=device-width, initial-scale=1\">");
client.println("<link rel=\"icon\" href=\"data:,\">");
client.println("<style>html { font-family: Helvetica; display: inline-block; margin: 0px auto; text-align: center;}");
client.println(".button { background-color: #4CAF50; border: 2px solid #4CAF50;; color: white; padding: 15px 32px; text-align: center; text-decoration: none; display: inline-block; font-size: 16px; margin: 4px 2px; cursor: pointer; }");
client.println("text-decoration: none; font-size: 30px; margin: 2px; cursor: pointer;}"); 
// Web Page Heading
client.println("</style></head>");
client.println("<body><center><h1>ESP32 Web server LED controlling example</h1></center>");
client.println("<center><h2>Web Server Example Microcontrollerslab.com</h2></center>" );
client.println("<center><h2>Press on button to turn on led and off button to turn off LED</h3></center>");
client.println("<form><center>");
client.println("<p> LED one is " + LED_ONE_STATE + "</p>");
// If the PIN_NUMBER_22State is off, it displays the ON button 
client.println("<center> <button class=\"button\" name=\"LED0\" value=\"ON\" type=\"submit\">LED0 ON</button>") ;
client.println("<button class=\"button\" name=\"LED0\" value=\"OFF\" type=\"submit\">LED0 OFF</button><br><br>");
client.println("<p>LED two is " + LED_TWO_STATE + "</p>");
client.println("<button class=\"button\" name=\"LED1\" value=\"ON\" type=\"submit\">LED1 ON</button>");
client.println("<button class=\"button\" name=\"LED1\" value=\"OFF\" type=\"submit\">LED1 OFF</button> <br><br>");
client.println("<p>LED three is " + LED_THREE_STATE + "</p>");
client.println ("<button class=\"button\" name=\"LED2\" value=\"ON\" type=\"submit\">LED2 ON</button>");
client.println ("<button class=\"button\" name=\"LED2\" value=\"OFF\" type=\"submit\">LED2 OFF</button></center>");
client.println("</center></form></body></html>");
client.println();
break;
} 
else 
{ 
current_data_line = "";
}
} 
else if (new_byte != '\r') 
{ 
current_data_line += new_byte; 
}
}
}
// Clear the header variable
header = "";
// Close the connection
client.stop();
Serial.println("Client disconnected.");
Serial.println("");
}
}
```
## Follow up Problem Statement
Try to implement a controller that can control the brightness of the LED through a webserver.

## References
* [ESP32 Web server in Arduino IDE: Controlling LEDs](https://microcontrollerslab.com/esp32-web-server-arduino-led/)
* [ESP32 Web server](https://randomnerdtutorials.com/esp32-web-server-slider-pwm/)
