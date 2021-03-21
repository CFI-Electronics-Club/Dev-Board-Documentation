# ESP32 Face Recognition Door lock
Security is at most concern for anyone nowadays, whether it's data security or security of their own home. With the advancement of technology and the increasing use of IoT, digital door locks have become very common these days. Digital lock doesn’t require any physical key but it uses RFID, fingerprint, Face ID, pin, passwords, etc. to control the door lock.In this tutorial we build a Face ID controlled Digital Door lock system using ESP32 and an external Camera.

## Materials Required
1. ESP32 CAM
2. FTDI Board
3. Relay Module
4. Solenoid Lock
5. Jumper Wires

## Solenoid Lock
A solenoid lock works on the electronic-mechanical locking mechanism. This type of lock has a slug with a slanted cut and a good mounting bracket. When the power is applied, DC creates a magnetic field that moves the slug inside and keeps the door in the unlocked position. The slug will retain its position until the power is removed. When the power is disconnected, the slug moves outside and locks the door. It doesn’t use any power in a locked state. To drive the solenoid lock, you would need a power source that can give 12V @ 500mA. 

![solenoid_lock](https://circuitdigest.com/sites/default/files/inlineimages/u3/Solenoid-Door-Lock.jpg)

## Circuit Diagram

![circuit](https://circuitdigest.com/fullimage?i=circuitdiagram_mic/Face-Recognition-Door-Lock-Circuit-Diagram.png)

The circuit above combined with an FTDI board, Relay Module, and Solenoid Lock. The FTDI board is used to flash the code into ESP32-CAM as it doesn’t have a USB connector while the relay module is used to switch the Solenoid lock on or off. VCC and GND pins of the FTDI board and Relay module is connected to the Vcc and GND pin of ESP32-CAM. TX and RX of the FTDI board are connected to RX and TX of ESP32 and the IN pin of the relay module is connected to IO4 of ESP32-CAM.

## Code

```
#include "esp_camera.h"
#include <WiFi.h>
//
// WARNING!!! Make sure that you have either selected ESP32 Wrover Module,
//            or another board which has PSRAM enabled
//
// Select camera model
//#define CAMERA_MODEL_WROVER_KIT
//#define CAMERA_MODEL_ESP_EYE
//#define CAMERA_MODEL_M5STACK_PSRAM
//#define CAMERA_MODEL_M5STACK_WIDE
#define CAMERA_MODEL_AI_THINKER
#include "camera_pins.h"
const char* ssid = "Galaxy-M20";
const char* password = "ac312124";
#define LED_BUILTIN 4
#define relay 4 
#define buzzer 2
boolean matchFace = false;
boolean activeRelay = false;
long prevMillis = 0;
int interval = 5000;
void startCameraServer();
void setup() {
  Serial.begin(115200);
  Serial.setDebugOutput(true);
  Serial.println();
  pinMode(relay, OUTPUT); 
  pinMode(buzzer, OUTPUT); 
  pinMode (LED_BUILTIN, OUTPUT);
  digitalWrite(LED_BUILTIN, LOW);
  digitalWrite(relay, LOW);
  digitalWrite(buzzer, LOW);
  camera_config_t config;
  config.ledc_channel = LEDC_CHANNEL_0;
  config.ledc_timer = LEDC_TIMER_0;
  config.pin_d0 = Y2_GPIO_NUM;
  config.pin_d1 = Y3_GPIO_NUM;
  config.pin_d2 = Y4_GPIO_NUM;
  config.pin_d3 = Y5_GPIO_NUM;
  config.pin_d4 = Y6_GPIO_NUM;
  config.pin_d5 = Y7_GPIO_NUM;
  config.pin_d6 = Y8_GPIO_NUM;
  config.pin_d7 = Y9_GPIO_NUM;
  config.pin_xclk = XCLK_GPIO_NUM;
  config.pin_pclk = PCLK_GPIO_NUM;
  config.pin_vsync = VSYNC_GPIO_NUM;
  config.pin_href = HREF_GPIO_NUM;
  config.pin_sscb_sda = SIOD_GPIO_NUM;
  config.pin_sscb_scl = SIOC_GPIO_NUM;
  config.pin_pwdn = PWDN_GPIO_NUM;
  config.pin_reset = RESET_GPIO_NUM;
  config.xclk_freq_hz = 20000000;
  config.pixel_format = PIXFORMAT_JPEG;
  //init with high specs to pre-allocate larger buffers
  if(psramFound()){
    config.frame_size = FRAMESIZE_UXGA;
    config.jpeg_quality = 10;
    config.fb_count = 2;
  } else {
    config.frame_size = FRAMESIZE_SVGA;
    config.jpeg_quality = 12;
    config.fb_count = 1;
  }
#if defined(CAMERA_MODEL_ESP_EYE)
  pinMode(13, INPUT_PULLUP);
  pinMode(14, INPUT_PULLUP);
#endif
  // camera init
  esp_err_t err = esp_camera_init(&config);
  if (err != ESP_OK) {
    Serial.printf("Camera init failed with error 0x%x", err);
    return;
  }
  sensor_t * s = esp_camera_sensor_get();
  //initial sensors are flipped vertically and colors are a bit saturated
  if (s->id.PID == OV3660_PID) {
    s->set_vflip(s, 1);//flip it back
    s->set_brightness(s, 1);//up the blightness just a bit
    s->set_saturation(s, -2);//lower the saturation
  }
  //drop down frame size for higher initial frame rate
  s->set_framesize(s, FRAMESIZE_QVGA);
#if defined(CAMERA_MODEL_M5STACK_WIDE)
  s->set_vflip(s, 1);
  s->set_hmirror(s, 1);
#endif
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("WiFi connected");
  startCameraServer();
  Serial.print("Camera Ready! Use 'http://");
  Serial.print(WiFi.localIP());
  Serial.println("' to connect");
}
void loop() {
    if (matchFace == true && activeRelay == false){
      activeRelay = true;
      digitalWrite (relay, HIGH);
      digitalWrite (buzzer, HIGH);
      delay(800);
      digitalWrite (buzzer, LOW);
      prevMillis = millis();
    }
    if(activeRelay == true && millis()- prevMillis > interval){
      activeRelay = false;
      matchFace = false; 
      digitalWrite(relay, LOW);
    }               
}
```

## Refernces
You can find more details of the project and the description of the components [here](https://circuitdigest.com/microcontroller-projects/esp32-cam-face-recognition-door-lock-system)
