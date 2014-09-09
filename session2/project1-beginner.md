---
layout: default
title: Session 2- LED Blink and Stopwatch Examples
date: September 4, 2013
---

##Session 2 - LED Blink and Stopwatch Examples

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
