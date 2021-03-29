### Communication Protocols 
Proper descriptions of Digital message formats and rules that allow two electronic devices to connect and exchange data with each other            
##### Inter-System Protocol: 
Communication between two different devices - **UART**, **USART**, **USB**, etc.        
##### Intra-System Protocol: 
Communication between two devices within the circuit: Pins of Microcontroller are extended out to connect to external devices - **I2C**, **SPI**, **CAN**, etc.      
##### UART (Universal Asynchronous Transmitter and Receiver):
* Serial communication with two wires - RX: Receiver and TX: Transmitter             
* Takes bytes of data and sends the individual bits in a sequential manner               
* Half-Duplex protocol -  transferring and receiving data, but not at the same time     
* **USART** - Full-Duplex protocol: Simultaneous Transmission and Receiving of Data                 
##### SPI (Serial Peripheral Interface):
* 4-Wire Protocol - MOSI, MISO, SS/CS, SCLK          
* Communication between Master and Slave Devices         
* Full-Duplex Protocol           
##### I2C (Inter-Integrated Circuit):          
* Two Wires - SDA (Serial Data Line) and SCL (Serial Clock Line)          
* Master to Slave Communication Protocol         
* Each Slave has a unique address, using which the Master selects a device to communicate with             
[Further Reading](https://www.elprocus.com/communication-protocols/)     
## Electronics Club Custom Development Board
Our Custom Dev Board is developed around the **ESP32 WROOM SoC** (System-on-Chip) and comes with **WiFi** and **Bluetooth** functionalities. The Board comes with a built-in **IMU** (Inertial Measurement Unit) Sensor and an RGB LED.                        

![](Images/CDB.jpeg)
### Pin Description:
![](Images/PinDescr.png)                  
###### Legend:         
###### GPIO - General Purpose Input/Output Pin, SPI - Serial Peripheral Interface, I2C - Inter-Integrated Circuit, ADC - Analog-to-Digital Converter, DAC - Digital-to-Analog Converter, UART - Universal Asynchronous Receiver-Transmitter, Touch - Capacitive Touch Pin                  
###### Note: Power Supply is +5V 
### Custom Dev Board vs Arduino UNO:
![](Images/CDBvsUNO.png)
##### IMU (Inertial Measurement Unit): 
* An electronic device used to calculate the orientation, angular velocity, etc. of an object, when attached to it        
* Blend of Accelerometer, Gyroscope (6-Axis IMU) and additionally a Magnetometer (9-Axis IMU)          
* Data from an IMU sensor can be manipulated to obtain direction, acceleration, etc.                
* Used in UAVs, IMU based GPS devices and also in Robotic applications
