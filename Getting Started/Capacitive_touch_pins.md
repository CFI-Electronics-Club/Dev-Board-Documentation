# Capacitive Touch Pins
The ESP32 has 10 capacitive touch GPIOs. These GPIOs can sense variations in anything that holds an electrical charge, like the human skin. So they can detect variations induced when touching the GPIOs with a finger. These pins can be easily integrated into capacitive pads, and replace mechanical buttons. Additionally, the touch pins can also be used as a wake up source when the ESP32 is in deep sleep.


The GPIO pins which act as touch pins are pins numbered 0, 2, 4, 12, 13, 14, 15, 27, 32, 33.

## Working of a capacitive touch sensor
We’re all accustomed to seeing capacitance in the form of leaded components or surface-mount packages, but actually, all we really need is two conductors separated by an insulating material. 

![](https://www.allaboutcircuits.com/uploads/articles/ICTS_diagram1.JPG)

The insulating separation between the touch-sensitive button and the surrounding copper creates a capacitor. In this case, the surrounding copper is connected to the ground node, and consequently, our touch-sensitive button can be modeled as a capacitor between the touch-sensitive signal and ground.

## The effect of a finger 
