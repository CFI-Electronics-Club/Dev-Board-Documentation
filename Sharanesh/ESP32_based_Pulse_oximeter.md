# IoT Based Heart Rate Monitor using MAX30100 Pulse Oximeter and ESP32

Pulse oximetry is a widely used medical measurement instrument and it is a non-invasive and painless test that measures oxygen saturation level in our blood that can easily detect small changes in oxygen. In the current Covid-19 situation, it has become important to track the oxygen level of multiple patients at the same time remotely without getting into contact with the patient.

## Description
So, in this project, we build a pulse oximeter using MAX30100 Pulse oximeter and ESP32 that will track the Blood Oxygen level and send the data via internet by connecting to a Wi-Fi network. This way, we can monitor multiple patients remotely by maintaining social distance with the patients. The obtained data will be shown as a graph which makes it easier for tracking and analyzing the patient’s condition. Previously, we have also built other heart rate monitors using pulse sensors. And if you are interested in other Covid-19 related projects, you can check out the Human body thermometer, Smart IR Thermometer for fever monitoring, and Wall-Mount Temperature scanner that we build earlier.

## Pulse Oximeter sensor : MAX30100 Sensor
MAX30100 sensor is integrated pulse oximetry and heart rate monitor module. It communicates with the I2C data line and provides the SpO2 and Pulse information to the host microcontroller unit. It uses photodetectors, optical elements where red, green IR LED modulates the LED pulses. The LED current is configurable from 0 to 50mA. The below image is showing the MAX30100 sensor.The above sensor module works with 1.8V to the 5.5V range. The pull-up resistors for the I2C pins are included in the module.

## Materials Required
1. ESP32
2. A WiFi connection
3. MAX30100 Sensor
4. Adafruit IO user id and a custom created dashboard (Will make it further)
5. 5V adequate power supply unit with the rated current of at least 1A
6. USB cable Micro USB to USBA

