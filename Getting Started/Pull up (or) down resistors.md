# Pull Up/Down resistors
In digital circuitry, the two valid voltage levels are known as logic HIGH (On) and logic LOW (off). Usually, logic HIGH is the supply voltage value (Vcc) and logic LOW value is the ground. We all know that digital systems work based on the inputs we give. For example, the button we press in a vending machine is a digital input, the keys we type on a keyboard are digital inputs. Their valid states are either logic HIGH or LOW. There is a simple digital circuit present below:

![temp](https://github.com/CFI-Electronics-Club/Dev-Board-Documentation/blob/main/Getting%20Started/Images/logic2.jpg)

Also, sometimes we need an input to be present in it's default state and only need it to switch to the other state when the user enters the input (Pushbutton example). The circuit below seems like it achieves the purpose:

![temp](https://github.com/CFI-Electronics-Club/Dev-Board-Documentation/blob/main/Getting%20Started/Images/logic1.jpg)

However, there are some problems. The problem with the first circuit is that when the switch is open, the input wire is neither HIGH nor LOW. Instead it is **Floating** between Vcc and Gnd. The problem with the second circuit is that when the input we want is logic LOW, the switch should be closed. This leads to shorting of the battery which can cause a lot of issues from damaging the circuitry to energy losses. Hence, we need a different circuit to resolve these issues and that's where the concept of Pull Up/Down resistors was introduced.

## Concept behind Pull Up/Down resistors:

![temp](https://github.com/CFI-Electronics-Club/Dev-Board-Documentation/blob/main/Getting%20Started/Images/pullupdown.jpg)

When a pull Up/Down resistor is used, the input terminal is automatically maintained at a valid default state. When the default state is logic HIGH, the resistor connects the input to Vcc and hence, known as **Pull Up** Resistor. If the default state is logic LOW, then the resistor connects input to Gnd and hence, known as **Pull down** resistor. They ensure that the input terminal is never floating and also won't allow shorting of the source. Now, that we understood the concept of Pull Up/Down resistors, the next step is to find what are their values. But before doing that, first we must understand the range of voltage for different digital states. 

## 
