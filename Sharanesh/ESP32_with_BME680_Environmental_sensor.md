# ESP32: BME680 Environmental Sensor using Arduino IDE
The BME680 is an environmental digital sensor that measures gas, pressure, humidity and temperature. In this guide you’ll learn how to use the BME680 sensor module with the ESP32 board using Arduino IDE. The sensor communicates with a microcontroller using I2C or SPI communication protocols.

## BME680 Environmental Sensor Module
The BME680 is an environmental sensor that combines gas, pressure, humidity and temperature sensors. The gas sensor can detect a broad range of gases like volatile organic compounds (VOC). For this reason, the BME680 can be used in indoor air quality control.

The BME680 is a 4-in-1 digital sensor that measures:

- Temperature
- Humidity
- Barometric pressure
- Gas: Volatile Organic Compounds (VOC) like ethanol and carbon monoxide

![env_sensor](https://i1.wp.com/randomnerdtutorials.com/wp-content/uploads/2020/07/BME680-Gas-sensor-humidity-barometric-pressure-ambient-temperature-gas-air-quality-front.jpg?w=750&quality=100&strip=all&ssl=1)

## Materials Required
1. ESP32 Board
2. BME680 Environmental sensor

## Circuit Diagram
![circuit](https://i2.wp.com/randomnerdtutorials.com/wp-content/uploads/2020/07/ESP32_BME680_Wiring_Diagram_I2C.png?w=786&quality=100&strip=all&ssl=1)

## Code

```
#include <Wire.h>
#include <SPI.h>
#include <Adafruit_Sensor.h>
#include "Adafruit_BME680.h"

/*#define BME_SCK 18
#define BME_MISO 19
#define BME_MOSI 23
#define BME_CS 15*/

#define SEALEVELPRESSURE_HPA (1013.25)

Adafruit_BME680 bme; // I2C
//Adafruit_BME680 bme(BME_CS); // hardware SPI
//Adafruit_BME680 bme(BME_CS, BME_MOSI, BME_MISO, BME_SCK);

void setup() {
  Serial.begin(115200);
  while (!Serial);
  Serial.println(F("BME680 async test"));

  if (!bme.begin()) {
    Serial.println(F("Could not find a valid BME680 sensor, check wiring!"));
    while (1);
  }

  // Set up oversampling and filter initialization
  bme.setTemperatureOversampling(BME680_OS_8X);
  bme.setHumidityOversampling(BME680_OS_2X);
  bme.setPressureOversampling(BME680_OS_4X);
  bme.setIIRFilterSize(BME680_FILTER_SIZE_3);
  bme.setGasHeater(320, 150); // 320*C for 150 ms
}

void loop() {
  // Tell BME680 to begin measurement.
  unsigned long endTime = bme.beginReading();
  if (endTime == 0) {
    Serial.println(F("Failed to begin reading :("));
    return;
  }
  Serial.print(F("Reading started at "));
  Serial.print(millis());
  Serial.print(F(" and will finish at "));
  Serial.println(endTime);

  Serial.println(F("You can do other work during BME680 measurement."));
  delay(50); // This represents parallel work.
  // There's no need to delay() until millis() >= endTime: bme.endReading()
  // takes care of that. It's okay for parallel work to take longer than
  // BME680's measurement time.

  // Obtain measurement results from BME680. Note that this operation isn't
  // instantaneous even if milli() >= endTime due to I2C/SPI latency.
  if (!bme.endReading()) {
    Serial.println(F("Failed to complete reading :("));
    return;
  }
  Serial.print(F("Reading completed at "));
  Serial.println(millis());

  Serial.print(F("Temperature = "));
  Serial.print(bme.temperature);
  Serial.println(F(" *C"));

  Serial.print(F("Pressure = "));
  Serial.print(bme.pressure / 100.0);
  Serial.println(F(" hPa"));

  Serial.print(F("Humidity = "));
  Serial.print(bme.humidity);
  Serial.println(F(" %"));

  Serial.print(F("Gas = "));
  Serial.print(bme.gas_resistance / 1000.0);
  Serial.println(F(" KOhms"));

  Serial.print(F("Approx. Altitude = "));
  Serial.print(bme.readAltitude(SEALEVELPRESSURE_HPA));
  Serial.println(F(" m"));

  Serial.println();
  delay(2000);
}
```

Upload the code to your ESP32 board. Go to Tools > Board and select the ESP32 board you’re using. Go to Tools > Port and select the port your board is connected to. Then, click the upload button.

Open the Serial Monitor at a baud rate of 115200, press the on-board RST button. The sensor measurements will be displayed.

## Referneces
You can find more details of the project and the description of the components [here](https://randomnerdtutorials.com/esp32-bme680-sensor-arduino/)
