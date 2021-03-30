# Capacitive Touch Pins
The ESP32 has 10 capacitive touch GPIOs. These GPIOs can sense variations in anything that holds an electrical charge, like the human skin. So they can detect variations induced when touching the GPIOs with a finger. These pins can be easily integrated into capacitive pads, and replace mechanical buttons. Additionally, the touch pins can also be used as a wake up source when the ESP32 is in deep sleep.


The GPIO pins which act as touch pins are pins numbered 0, 2, 4, 12, 13, 14, 15, 27, 32, 33.

## Working of a capacitive touch sensor
We’re all accustomed to seeing capacitance in the form of leaded components or surface-mount packages, but actually, all we really need is two conductors separated by an insulating material. 

![](https://www.allaboutcircuits.com/uploads/articles/ICTS_diagram1.JPG)

The insulating separation between the touch-sensitive button and the surrounding copper creates a capacitor. In this case, the surrounding copper is connected to the ground node, and consequently, our touch-sensitive button can be modeled as a capacitor between the touch-sensitive signal and ground.

## The effect of a finger 
The finger is insulated from the capacitor by the PCB’s solder mask and usually also by a layer of plastic that separates the device’s electronics from the external environment. So the finger is not discharging the capacitor, and furthermore, the amount of charge stored in the capacitor at a particular moment is not the quantity of interest, rather, the quantity of interest is the capacitance at a particular moment.

![](https://www.allaboutcircuits.com/uploads/articles/ICTS_diagram2.JPG)

* Finger as dielectric:

We certainly cannot change the physical dimensions of the capacitor just by touching it, but we can change the dielectric constant, because a human finger has different dielectric characteristics than the material (presumably air) that it is displacing. Since the human flesh is a good dielectric, the finger’s interaction with the capacitor’s electric field represents an increase in the dielectric constant and hence an increase in the capacitance.

* Finger as conductor:

We know that the human skin is also conductive. We can assume that this new capacitor created by the finger (let’s call it the finger cap) is in parallel with the existing PCB capacitor. This situation is a bit complex because the person using the touch-sensitive device is not electrically connected to the PCB’s ground node. We can think of the human body as providing a virtual ground because it has a relatively large capacity to absorb electric charge.

## Code
Reading the touch sensor is straightforward. In the Arduino IDE, you use the touchRead() function, that accepts as argument, the GPIO you want to read.

'''
void setup() {
  Serial.begin(115200);
  delay(1000); // give me time to bring up serial monitor
  Serial.println("ESP32 Touch Test");
}

void loop() {
  Serial.println(touchRead(4));  // get value of Touch 0 pin = GPIO 4
  delay(1000);
}
'''
