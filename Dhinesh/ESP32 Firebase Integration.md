# ESP32 Firebase Integration

Google Firebase is a Google-backed application development software used for creating, managing, and modifying data generated from any android/IOS application, web services, IoT sensors & Hardware. To learn more about the Google Firebase Console, you can read the official [Google Firebase Documentation](https://firebase.google.com/docs) from Google Firebase.

## What is Google Firebase Database?

The Firebase Realtime Database is a cloud-hosted database. When you build cross-platform apps with our iOS, Android, and JavaScript SDKs, all of your clients share one realtime Database instance and automatically receive updates with the newest data.

## Can I use Firebase for Free?

Yes, it’s free. You can use Analytics for advanced messaging – out of the box. Which products are paid? Firebase’s paid infrastructure products are the Realtime Database, Firebase Storage, Hosting, and Test Lab.

## Parts Required:

- Electronics Club ESP32 custom development board
- Potentiometer

## Instructions:

- Follow instructions from this [link](https://www.electroniclinic.com/esp32-firebase-tutorial-send-sensor-data-to-google-firebase-database/) to setup Google Firebase Account Setup for the ESP32. 
- Before you start the programming, first of all make sure that you download all the necessary [libraries](https://www.electroniclinic.com/wp-content/uploads/2019/08/FirebaseArduino.zip). 
- Connect the Potentiometer with the Analog pin A0 of the Electronics Club ESP32 custom development board.
## Code:

```
#include <WiFi.h>
#include <FirebaseESP32.h>


#define FIREBASE_HOST "https://esp32andfirebase.firebaseio.com/"
#define FIREBASE_AUTH "gga1JXWY5rgyBHk56RSXLn3FPpajWfcq6itIM3nI"
#define WIFI_SSID "AndroidAP7DF8"
#define WIFI_PASSWORD "electroniclinic"


//Define FirebaseESP32 data object
FirebaseData firebaseData;
FirebaseJson json;
int Vresistor = A0;
int Vrdata = 0;

void setup()
{

  Serial.begin(115200);

 pinMode(Vresistor, INPUT);


  WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
  Serial.print("Connecting to Wi-Fi");
  while (WiFi.status() != WL_CONNECTED)
  {
    Serial.print(".");
    delay(300);
  }
  Serial.println();
  Serial.print("Connected with IP: ");
  Serial.println(WiFi.localIP());
  Serial.println();

  Firebase.begin(FIREBASE_HOST, FIREBASE_AUTH);
  Firebase.reconnectWiFi(true);

  //Set database read timeout to 1 minute (max 15 minutes)
  Firebase.setReadTimeout(firebaseData, 1000 * 60);
  //tiny, small, medium, large and unlimited.
  //Size and its write timeout e.g. tiny (1s), small (10s), medium (30s) and large (60s).
  Firebase.setwriteSizeLimit(firebaseData, "tiny");

  /*
  This option allows get and delete functions (PUT and DELETE HTTP requests) works for device connected behind the
  Firewall that allows only GET and POST requests.

  Firebase.enableClassicRequest(firebaseData, true);
  */

  //String path = "/data";


  Serial.println("------------------------------------");
  Serial.println("Connected...");

}

void loop()
{
   Vrdata = analogRead(Vresistor);
 int Sdata = map(Vrdata,0,4095,0,1000);
 Serial.println(Sdata);
delay(100);
  json.set("/data", Sdata);
  Firebase.updateNode(firebaseData,"/Sensor",json);

}
```

## Links:

- This tutorial is extracted from [here](https://www.electroniclinic.com/esp32-firebase-tutorial-send-sensor-data-to-google-firebase-database/). For more detailed explanation, you can refer to this link.

- [Firebase Library](https://www.electroniclinic.com/wp-content/uploads/2019/08/FirebaseArduino.zip)

- [Google Firebase Documentation](https://firebase.google.com/docs)
