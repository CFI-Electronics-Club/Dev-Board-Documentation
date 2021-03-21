# Flappy Bird Game using ESP32
## Description
The classic Flappy Bird Game on OLED using ESP32
## Prerequisites
* Arduino IDE     
* Download the following [OLED Library](https://github.com/ThingPulse/esp8266-oled-ssd1306) as a *.zip* file and add it to your Arduino IDE. Go through the documentation carefully
## Components
* Electronics Club Custom Dev Board
* OLED Display
* Buzzer
* Pushbutton     
* Jumpers
## Schematic
VCC of OLED --> VCC of CDB      
GND of OLED --> GND of CDB      
SDA of OLED --> 21 of CDB       
SCL of OLED --> 22 of CDB      
Buzzer (longer leg) --> 23 of CDB     
Buzzer (shorter leg) --> GND          
Push Button  --> 4 of CDB              
Push Button --> GND         
![](Images/FlappySchematic.png)
## Code
```
#include <Wire.h>  // Only needed for Arduino 1.6.5 and earlier
#include "SSD1306Wire.h" // legacy include: `#include "SSD1306.h"`

#include "images.h"
#include "fontovi.h"

SSD1306Wire  display(0x3c, 21, 22);

#define DEMO_DURATION 3000
typedef void (*Demo)(void);

float zidx[4];
int prazan[4];
int razmak=32;
int sirinaProlaza=30;
void setup() {
   
  Serial.println();
  Serial.println();

pinMode(2,OUTPUT);
pinMode(23,OUTPUT);
pinMode(4,INPUT_PULLUP); 
  display.init();

  for(int i=0;i<4;i++)
  {
    zidx[i]=128+((i+1)*razmak);
    {prazan[i]=random(8,32);}
    }

  display.flipScreenVertically();
  display.setFont(ArialMT_Plain_10);

}



 // display.drawProgressBar(0, 32, 120, 10, progress);

// display.setPixel(i, i);

 //  display.drawXbm(34, 14, WiFi_Logo_width, WiFi_Logo_height, WiFi_Logo_bits);
//display.fillCircle(96, 32, 32 - i* 3);


//display.drawRect(12, 12, 20, 20);
 //display.setFont(ArialMT_Plain_10);
   // display.setColor(WHITE);
   
  //  display.fillRect(14, 14, 17, 17);
int score=0;
int stis=0;
float fx=30.00;
float fy=22.00;
int smjer=0;
unsigned long trenutno=0;

int igra=0;
int frame=0;
int sviraj=0;
unsigned long ton=0;
void loop() {

   display.clear();

   if(igra==0)
   {
    display.setFont(ArialMT_Plain_16);
    display.drawString(0,4,"Flappy ");
   display.drawXbm(0, 0, 128, 64, pozadina);
   display.drawXbm(20, 32, 14, 9, ptica);

   display.setFont(ArialMT_Plain_10);
   display.drawString(0,44,"press to start");
    if(digitalRead(4)==0)
    igra=1;
    }

   if(igra==1)
   {
   display.setFont(ArialMT_Plain_10);
   display.drawString(3,0,String(score));
   
   if(digitalRead(4)==0)
   {
    if(stis==0)
      {
        trenutno=millis();
        smjer=1;
        sviraj=1;
        stis=1;
        ton=millis();
       
        }
    
    }else{stis=0;}

   
    for(int j=0;j<4;j++){
       display.setColor(WHITE);
    display.fillRect(zidx[j],0,6,64);
     display.setColor(BLACK);
      display.fillRect(zidx[j],prazan[j],6,sirinaProlaza);
   
    }

  display.setColor(WHITE);
 //  display.fillCircle(fx, fy, 4); // igrac
  display.drawXbm(fx, fy, 14, 9, ptica);
  
    for(int j=0;j<4;j++)
    {
    zidx[j]=zidx[j]-0.01;
    if(zidx[j]<-7){
      score=score+1;
      
      
       digitalWrite(23,1);
      prazan[j]=random(8,32);
     
    zidx[j]=128;
    }
    }
  if((trenutno+185)<millis())
  smjer=0;

    if((ton+40)<millis())
  sviraj=0;

if(smjer==0)
 fy=fy+0.01;
else
 fy=fy-0.03;


 if(sviraj==1)
 digitalWrite(23,1);
 else
 digitalWrite(23,0);

if(fy>63 || fy<0){
igra=0;
fy=22;
score=0;
 digitalWrite(23,1);
  delay(500);
   digitalWrite(23,0); 
for(int i=0;i<4;i++)
  {
    zidx[i]=128+((i+1)*razmak);
    {prazan[i]=random(4,30);}
    }
}

for(int m=0;m<4;m++)
if(zidx[m]<=fx+7 && fx+7<=zidx[m]+6)
{
  
 
  if(fy<prazan[m] || fy+8>prazan[m]+sirinaProlaza){
  igra=0;
  fy=22;
  score=0;
  digitalWrite(23,1);
  delay(500);
   digitalWrite(23,0);
  for(int i=0;i<4;i++)
  {
    zidx[i]=128+((i+1)*razmak);
    {prazan[i]=random(8,32);}
    }
  }}
   display.drawRect(0,0,128,64);
   }
  
   display.display();

}
```
#### Header Files included in the code
##### images.h:
```
## References
[ESP32 Flappy Bird Game](https://www.hackster.io/arduinob2016/esp32-flappy-bird-game-54391c)

