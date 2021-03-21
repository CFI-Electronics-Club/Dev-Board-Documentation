# Game that learns how to play itself

## Description

Building a game that learns how to play itself using a good old game pad.

<div style="text-align:center"><img src="https://static.wixstatic.com/media/fa678f_76a2a3cdf73745858df56801eb81a011~mv2.jpg/v1/fill/w_738,h_1135,al_c,q_85,usm_0.66_1.00_0.01/fa678f_76a2a3cdf73745858df56801eb81a011~mv2.webp" alt="drawing" width="200" height="300"/></div>

This project could also be resolved in a much more efficient way using 'classic' control systems such as PID controllers, LQR... But we focuss on a neural network as it was actually really interesting to see how a neural network would cope with all the drawbacks coming with the hardware (play in the parts, lag in movement, imperfect calibration...)

## Parts Required

- ESP32 development board (Uno should work too)
- 2x MG-90s servos
- 2X MR105ZZ ball bearing (5 x 10 x 4 mm)
- 10X M2x10mm self tapping screw
- Zip ties
- wires
- dupont connectors (optional)
- Xbox controller

## Instructions

For the game and machine learning part we use the game engine Unity coupled with the ML-agents toolkit also developed by Unity. The ML-agent tool kit comes with a few pre-built examples, one of them being a ball balancing exercise, that could easily be turned into a game requiring only a single joystick. Then design a simple arm that would actuate the joystick. The next step was to write a simple program for the Arduino that would allow it to control the joystick.
For detailed explanation on the instructions, arduino and unity sketch, refer Links section below.

## Links:

This tutorial is extracted from [here](https://www.hackster.io/Little_french_kev/a-game-that-learns-how-to-play-itself-db13a0). For more detailed explanation, you can refer to this link.

[https://www.littlefrenchkev.com/xbox-controller-arm](https://www.littlefrenchkev.com/xbox-controller-arm)
