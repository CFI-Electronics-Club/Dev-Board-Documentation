# Cricket Shot Identification
## Description:
Are you vivid cricket fan/player,if so here is an interesting project that you can try out.
Objective: Predict the cricketting shot that you played

## Concept:
Concept behind this project is almost same as what we did in the last Session(HP wand using ML) 

You can refer this: [Day5](https://github.com/Fruitseye/ESP32_Mega_Session/tree/main/Day5_Machine_Learning_on_MCU)

Differences from the Harry potter wand in case of this project:
 - You need to attach your ESP32 firmly on a Cricket bat (Attach such a way that ball doesn't hit the esp32 board while playing)
 - After you upload the code ,you need to remove it from the laptop and power it externally (Figure out how to do so)
 - You might have a question of how do you collect the data from the board if not connected in serial?
    - Setup a server and send data there(Like covered in Day2 and Day 3 of the sessions) or
    - Send data over Bluetooth Serial

***Note: Calibration is must***

## Prerequisites:
  - Should have attended/watched Day 5 ML Session

## Schematic:
 No extra components for this project,but if you want you can attach a buzzer to notify once the shot is identified
 
## Components:
  - ESP32 Board
  - Cricket bat
  - Buzzer (Optional)

## Follow-up problem Statement:
 - This project in itself is a follow up of the ML session
 - Likewise,you can extend do the same for other bat or racquet games
