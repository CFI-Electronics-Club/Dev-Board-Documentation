# BLE based Proximity Control using ESP32 – Detect Presence of BLE Devices
Proximity sensors can be described as one of a kind switches that detect a nearby object with the help of light, electromagnetic field, or sound. Typically, these types of devices are designed to detect nearby subjects, and often it's the practical application that most of these sensors are going to be used in. But there are circumstances where the subject is far away from the sensor or the subject is blocked by an obstruction, in these types of situations, we can use BLE (Bluetooth Low Energy) devices to detect and sense the proximity of the object. The ESP32 development board has built-in BLE, which we have used in many other projects. 

## What is Bluetooth Low Energy (BLE)?
BLE stands for Bluetooth Low Energy, and it came to our everyday lives in 2011, because at that time of the year every major manufacturer started embedding BLE technology on their devices. BLE is a low power wireless communication technology that was developed for battery power applications which can be used to communicate among devices over a short distance. Some of the devices you use every day have Bluetooth built into it like your smartphone, your smartwatch, wireless earbuds, wireless speakers, smart home devices, and more embedded Bluetooth to communicate or to get location data.

## How the BLE Proximity Sensor Works?
BLE servers regularly broadcast advertisement signals so the clients can search for it and connect to it. This advertisement signal contains a unique BLE MAC (Media Access Control) address, which is very similar to a MAC address used in Wi-Fi because our ESP32 module has Bluetooth built-in, we could easily detect this broadcasted signal and compare it to a lookup table, to detect the presence of a known device. Once the device is verified, we can turn on a light locally or we can use Adafruit IO to trigger a notification on our android application.

## Materials Required
- ESP32 Board
- Mobile Phone
- Any BLE Enabled device, be it a smartwatch or fitband or bluetooth speaker. Here we take a fitband to demonstrate.

## Code

```
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEScan.h>
#include <BLEAdvertisedDevice.h>
String knownBLEAddresses[] = {"6E:bc:55:18:cf:7b", "53:3c:cb:56:36:02", "40:99:4b:75:7d:2f", "5c:5b:68:6f:34:96"};
int RSSI_THRESHOLD = -55;
bool device_found;
int scanTime = 5; //In seconds
BLEScan* pBLEScan;
class MyAdvertisedDeviceCallbacks: public BLEAdvertisedDeviceCallbacks {
    void onResult(BLEAdvertisedDevice advertisedDevice) {
      for (int i = 0; i < (sizeof(knownBLEAddresses) / sizeof(knownBLEAddresses[0])); i++)
      {
        //Uncomment to Enable Debug Information
        //Serial.println("*************Start**************");
        //Serial.println(sizeof(knownBLEAddresses));
        //Serial.println(sizeof(knownBLEAddresses[0]));
        //Serial.println(sizeof(knownBLEAddresses)/sizeof(knownBLEAddresses[0]));
        //Serial.println(advertisedDevice.getAddress().toString().c_str());
        //Serial.println(knownBLEAddresses[i].c_str());
        //Serial.println("*************End**************");
        if (strcmp(advertisedDevice.getAddress().toString().c_str(), knownBLEAddresses[i].c_str()) == 0)
                        {
          device_found = true;
                          break;
                        }
        else
          device_found = false;
      }
      Serial.printf("Advertised Device: %s \n", advertisedDevice.toString().c_str());
    }
};
void setup() {
  Serial.begin(115200); //Enable UART on ESP32
  Serial.println("Scanning..."); // Print Scanning
  pinMode(LED_BUILTIN, OUTPUT); //make BUILTIN_LED pin as output
  BLEDevice::init("");
  pBLEScan = BLEDevice::getScan(); //create new scan
  pBLEScan->setAdvertisedDeviceCallbacks(new MyAdvertisedDeviceCallbacks()); //Init Callback Function
  pBLEScan->setActiveScan(true); //active scan uses more power, but get results faster
  pBLEScan->setInterval(100); // set Scan interval
  pBLEScan->setWindow(99);  // less or equal setInterval value
}
void loop() {
  // put your main code here, to run repeatedly:
  BLEScanResults foundDevices = pBLEScan->start(scanTime, false);
  for (int i = 0; i < foundDevices.getCount(); i++)
  {
    BLEAdvertisedDevice device = foundDevices.getDevice(i);
    int rssi = device.getRSSI();
    Serial.print("RSSI: ");
    Serial.println(rssi);
    if (rssi > RSSI_THRESHOLD && device_found == true)
      digitalWrite(LED_BUILTIN, HIGH);
    else
      digitalWrite(LED_BUILTIN, LOW);
  }
  pBLEScan->clearResults();   // delete results fromBLEScan buffer to release memory
}
```

## Refernces
You can find more details of the project and the description of the components [here](https://circuitdigest.com/microcontroller-projects/ble-based-proximity-control-using-esp32)
