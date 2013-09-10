---
layout: default
title: Project 1- LED Blink and Stopwatch
date: September 4, 2013
---

## Project 1 - LED Blink and Stopwatch

This project will be completed in two parts. First, write a short program that will blink an LED every x seconds. The user should be asked for input to determine how often to blink the LED. The LED should be connected to GPIO Pin 18 as shown in the below diagram. A 560 Ohm resistor is used to limit the current through the LED to 2mA.

 The current through the LED is determined by an equation known as Ohms law. It takes the form V=IR, where V is the voltage drop across the resistor, I is the current through the resistor and R is the resistance.  In this case, the output of the Raspberry Pi is 3.3V and the voltage drop across the LED is about 1.2V. This means the resistor has 1.1V remaining across it. Additionally, we know the Raspberry Pi output pin is capable of at most 4mA of current, but we will only use 2mA of that available current. So we setup our equation like this: 
V=IR
1.1V = .004A * R
R=550 Ohms




  A smaller resistor(or none) would allow too much curren through the LED and the Raspberry Pi's GPIO port, potentially damaging the port and the LED.

### LED Blink Breadboard Layout
<img src="https://dl.dropboxusercontent.com/u/1733921/Raspberry%20Pi/Schematics/RaspberryPi-LED%20Blink.png" alt="LED Blink Breadboard Layout" width="300px"/>


Next, create a stopwatch script. When the user presses a button, a timer should be started and the LED turned on. When the button is pushed again, the timer should be stopped and the LED turned off. Finally, the amount of time between button presses should be printed. The pushbutton should be connected to GPIO Pin 4 as shown below. Don't forget the 10k pullup resistor!

### Stopwatch Breadboard Layout
<img src="https://dl.dropboxusercontent.com/u/1733921/Raspberry%20Pi/Schematics/RaspberryPi-Stopwatch.png" alt="Stopwatch Breadboard Layout" width="300px" />

Source Code
<script src="http://gist-it.appspot.com/github/limited/IntroToRaspberryPi/blob/master/RaspberryPi_Toolbox/Project1-Beginner.py"></script>
