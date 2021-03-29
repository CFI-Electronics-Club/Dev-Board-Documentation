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
[Reference](https://www.elprocus.com/communication-protocols/)     
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
