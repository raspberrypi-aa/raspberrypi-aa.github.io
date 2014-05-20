---
layout: default
title: Session 2- LED Blink and Stopwatch Examples
date: September 4, 2013
---

##Session 2 - LED Blink and Stopwatch Examples


This example will be shown in two parts. First, we'll write a short program that will blink an LED every x seconds. The LED should be connected to GPIO Pin 18 as shown in the below diagram. A 560 Ohm resistor is used to limit the current through the LED to 2 mA. A smaller resistor(or none) would allow too much current through the LED and the Raspberry Pi's GPIO port, potentially damaging the port and the LED. 

#### LED Polarity
  First, a word on LED polarity. Some electronics components are polarized, meaning the direction the component is connected in matters. LEDs have a short leg and a long leg (or a flat side and a round side). The long side (or round side of the plastic shell) should always be connected to the point in the circuit with higher voltage (i.e. higher potential). This will allow current to flow through the LED in the proper direction. 
<img src="https://dl.dropboxusercontent.com/u/1733921/Raspberry%20Pi/LedPolarity.svg" width="100px"/>

#### Why a 560 Ohm resistor?
  The resistor is needed to disspiate extra electrical energy (voltage) coming from the Raspberry Pi. The Pi's digital outputs run at 3.3V and most LEDs have a forward voltage of about 2.2 Volts. As the voltage supply (3.3V) always equals the voltage load (LED + Resistor), our resistor will have a drop of 1.1V across it. We must also keep in mind the current drawn from the Raspberry Pi's output pin. The Raspberry Pi can only source up to 4mA of current, meaning we must choose our resistor to limit the current to less than 4mA at 1.1V.  The current through the LED is determined by an equation known as Ohms Law. It takes the form V=IR, where V is the voltage drop across the resistor, I is the current through the resistor and R is the resistance. We'll use 2mA just to be safe. Using Ohm's Law, we can determine the proper resistance to use is 560 Ohms.<br/>
V=IR<br/>
1.1V = .002A * R<br/>
R=550 Ohms<br/>

  A smaller resistor(or none) would allow too much current through the LED and the Raspberry Pi's GPIO port, potentially damaging the port and the LED.

###LED Blink Breadboard Layout
<img src="https://dl.dropboxusercontent.com/u/1733921/Raspberry%20Pi/Schematics/RaspberryPi-LED%20Blink.png" alt="LED Blink Breadboard Layout" width="300px"/>

#### Source Code
<script src="http://gist-it.appspot.com/github/raspberrypi-aa/raspberrypi-aa/blob/master/led-blink.py"></script>

Next, create a stopwatch script. When the user presses a button, a timer should be started and the LED turned on. When the button is pushed again, the timer should be stopped and the LED turned off. Finally, the amount of time between button presses should be printed. The pushbutton should be connected to GPIO Pin 4 as shown below. Don't forget the 10k pullup resistor!

<br/>
###Stopwatch Breadboard Layout
<img src="https://dl.dropboxusercontent.com/u/1733921/Raspberry%20Pi/Schematics/RaspberryPi-Stopwatch.png" alt="Stopwatch Breadboard Layout" width="300px" />

#### Source Code
<script src="http://gist-it.appspot.com/github/raspberrypi-aa/raspberrypi-aa/blob/master/stopwatch.py"></script>

### References
* [Resistor Calculator](http://www.dannyg.com/examples/res2/resistor.htmhttp://www.dannyg.com/examples/res2/resistor.htm)