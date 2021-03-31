# Pulse Width Modulation (PWM)
## What is PWM?
Digital signals have two positions: on or off, interpreted in shorthand as 1 or 0. Analog signals, on the other hand, can be on, off, half-way, two-thirds the way to on, and an infinite number of positions between 0 and 1 either approaching 1 or descending down to zero. The two are handled very differently in electronics, but very often must work together (that’s when we call it “mixed signal electronics.”) Sometimes we have to take an analog (real world) input signal (e.g., temperature) into a microcontroller (which only understands digital). Often engineers will translate that analog input into digital input for the microcontroller (MCU) by using an analog-to-digital converter. But what about outputs?

This is when PWM helps. PWM is a way to control analog devices with a digital output. Another way to put it is that you can output a modulating signal from a digital device such as an MCU to drive an analog device. It’s one of the primary means by which MCUs drive analog devices like variable-speed motors, dimmable lights, actuators, and speakers. PWM is not true analog output, however. PWM “fakes” an analog-like result by applying power in pulses, or short bursts of regulated voltage.

An example would be to apply full voltage to a motor or lamp for fractions of a second or pulse the voltage to the motor at intervals that made the motor or lamp do what you wanted it to do. In reality, the voltage is being applied and then removed many times in an interval, but what you experience is an analog-like response.

A device that is driven by PWM ends up behaving like the average of the pulses.
![]()
## How We Can Use It
There are two ways to implement PWM, one way would be to implement it manually, another way would be using a builtin function. Let us see how it is implemented manually.
```
void setup()
{
  pinMode(13, OUTPUT);
}

void loop()
{
  digitalWrite(13, HIGH);
  delayMicroseconds(100); // Approximately 10% duty cycle @ 1KHz
  digitalWrite(13, LOW);
  delayMicroseconds(1000 - 100);
}
```
The output pin 13 is at '1' state for 100 microseconds and it is in '0' state for 900 microseconds. This means that it is ON for only 10% of the time. Hence the duty cycle is 10%.

PWM can be implemeted directly using the analogWrite() function as follows.
```
analogWrite(0) means a signal of 0% duty cycle.

analogWrite(127) means a signal of 50% duty cycle.

analogWrite(255) means a signal of 100% duty cycle.
```
