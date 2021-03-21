# Capacitive Touch Slider
## Description
We make our own capacitive slider using aluminium foil and cellotape, use it as input to ESP32 which changes someother thing such as colour of a rgb bulb.
## Concept (Optional)
The capacitive slider surface can be of any shape and size you want.
The main principle this works on is -
Capacitance changes with increase/decrease in thickness of dielectric between the two electrodes.
Create a gradient of insulator on top of the aluminium foil to get this change in the capacitance from one end of the strip to another.
The choice of dielectric is clear plastic tape (cellotape), that is applied multiple layers. The tape is cut shorter by a few mm each time for the new layer applied on the aluminium foil. This will create a gradual increase in the thickness of the insulation over the strip. The change is still in terms of discreet steps, resolution of which should be decided based on the application and use case.

![image](https://user-images.githubusercontent.com/64363043/111902233-6e7c4c80-8a62-11eb-969b-ff612b217728.png)

## Prerequisites
ESP32 basic, understaning of capacitance
## Components
1.Aluminium Foil
2.Cellotape
3.ESP32
## Schematic
The logic is to read the touch inputs repeatedly and check if the value has increased or decrease as compared to the previous reading. Each time such increase or decrease happens ideally your finger has moved from a point on the strip of one thickness to another point of different thickness.

By having a count of such increase/decrease, we can get an analog value reading as well as direction of swipe. And this can be achieved from one GPIO pin of ESP32 connected to aluminium foil and you moviing your finger across multiple layers of tape.
## Code
```
#include <Arduino.h>
#include "RGBLED.h"


//prototype declaration
int level_inc_or_dec(int val);

int H =0;
RGBLED L1(25, 33, 32, COMMON_CATHODE);

void setup()
{
  Serial.begin(115200);
  delay(1000); // give me time to bring up serial monitor
  Serial.println("ESP32 Touch Swipe Test");
  
  L1.writeHSV(H, 1, 1);
}

int prev_val;
  
void loop()
{
  int val = touchRead(T5);
  int latest_val;
  if ( val < 70 && (val > prev_val+5 || val < prev_val-5) )
  {
    delay(10);
    latest_val = touchRead(T5);
    if ( latest_val < val+5 && latest_val > val-5)
    {
      prev_val = latest_val;
      Serial.println(latest_val);
      int inc_dec = level_inc_or_dec(latest_val);
      Serial.println(inc_dec);
      inc_dec > 0 ? H+=30 : (inc_dec ==0 ? 0:H -=30);
      H < 0 ? H=0:H;
      H > 360 ? H=360: H; 
      L1.writeHSV(H, 1, 1);
    }
  }
  delay(100);
}

int level_inc_or_dec(int val)
{
  int prev_val=val;
  int count = 0;
  while(val < 70)
  {
    delay(100);
    val = touchRead(T5);
    if ( val < 70 && (val > prev_val+5 || val < prev_val-5) )
    {
      val < prev_val ? count-- : count++;
      prev_val = val;
    }
  }
  return count;
}
```
## Problems
There may be problems wrt the making of the sensor and sensitivity of it, you have to experiment with different values of threshold in the code and finally get it right.
## Follow up Problem Statement
Try to make the same more efficient, and utilise it for much better applications
## References
https://hackaday.com/2015/11/30/conjuring-capacitive-touch-sensors-from-paper-and-aluminum-foil/ 
this helps understand how to make a touch sensor work, build up on the slider
