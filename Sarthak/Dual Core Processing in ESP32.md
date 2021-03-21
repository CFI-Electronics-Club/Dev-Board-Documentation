# Dual Core Processing in ESP32
## Description
* Using dual core feature of ESP32 to run 2 different tasks parallely on ESP32 (Parallel processing).
* With the help of dual core feature, blinking 2 LED's at two different blinking rates on ESP32
## Concept 
* The ESP32 comes with 2 Xtensa 32-bit LX6 microprocessors: core 0 and core 1 (Dual core). 
* When we run code on Arduino IDE, by default, it runs on core 1.
* The ESP32 comes with 2 Xtensa 32-bit LX6 microprocessors along with 2 cores, **Core 0** and **Core 1**.
* The Arduino IDE supports FreeRTOS for the ESP32, which is a Real Time Operating system (RTOS). This allows us to handle several tasks in parallel that run independently.
* Tasks are pieces of code that execute something. For example, it can be blinking an LED, making a network request, measuring sensor readings, publishing sensor readings, etc
* To assign specific parts of code to a specific core, you need to create tasks. When creating a task you can chose in which core it will run, as well as its priority. Priority values start at 0, in which 0 is the lowest priority. The processor will run the tasks with higher priority first.

## Prerequisites
**None**
## Components
* ESP32 CustomDev Board
* 2 LED's
* 2 x 300 ohm resistor
* Breadboard
* Jumper Wires
## Schematic
...
## Code
```
// The below code blinks 2 LED on different cores (Parallel blinking at different blinking rates)
TaskHandle_t Task1; // Initializing the tasks
TaskHandle_t Task2;

// LED pins
const int led1 = 2;
const int led2 = 4;

void setup() {
  Serial.begin(115200); 
  pinMode(led1, OUTPUT);
  pinMode(led2, OUTPUT);

  //create a task that will be executed in the Task1code() function, with priority 1 and executed on core 0
  xTaskCreatePinnedToCore(
                    Task1code,   /* Task function. */
                    "Task1",     /* name of task. */
                    10000,       /* Stack size of task */
                    NULL,        /* parameter of the task */
                    1,           /* priority of the task */
                    &Task1,      /* Task handle to keep track of created task */
                    0);          /* pin task to core 0 */                  
  delay(500); 

  //create a task that will be executed in the Task2code() function, with priority 1 and executed on core 1
  xTaskCreatePinnedToCore(
                    Task2code,   /* Task function. */
                    "Task2",     /* name of task. */
                    10000,       /* Stack size of task */
                    NULL,        /* parameter of the task */
                    1,           /* priority of the task */
                    &Task2,      /* Task handle to keep track of created task */
                    1);          /* pin task to core 1 */
    delay(500); 
}

//Task1code: blinks an LED every 1000 ms
void Task1code( void * pvParameters ){
  Serial.print("Task1 running on core ");
  Serial.println(xPortGetCoreID());

  for(;;){
    digitalWrite(led1, HIGH);
    delay(1000);
    digitalWrite(led1, LOW);
    delay(1000);
  } 
}

//Task2code: blinks an LED every 700 ms
void Task2code( void * pvParameters ){
  Serial.print("Task2 running on core ");
  Serial.println(xPortGetCoreID());

  for(;;){
    digitalWrite(led2, HIGH);
    delay(700);
    digitalWrite(led2, LOW);
    delay(700);
  }
}

void loop() {
  
}
```
## Follow up Problem Statement
* Figure out how is the above code different from coding the blinking of 2 LED's in series at different blinking rates. You can even code the above without using dual core (Not true in general for any code). 
* Blink 2 LED's at different blinking rates **without using Dual Core (RTOS)**
 PS - You can code in series in a single core. (Hint - Avoid using delay() function. Try searching for and using **millis()** func)
## Reference
* Watch this [video](https://youtu.be/k_D_Qu0cgu8) on **Dual Core Processing in ESP32** by **Andreas Spiess** to gain a deeper knowledge.
