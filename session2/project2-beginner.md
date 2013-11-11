---
layout: default
title: Session 2- Reaction Timer
date: September 15, 2013
---

##Session 2 - Reaction Timer 


This project will reuse the circuit layout from Project 1. You will create a reaction timer. The Raspberry Pi will turn on an LED and start a timer. The goal is to test your reaction time by pressing the button as close as possible to when the light turns on. When the button is pressed, turn the light off and print out the amount of time between the light turning on and the button press.

For better accuracy, you should use the GPIO interrupt functions (think wait_for_edge(pin, GPIO.FALLING))


<img src="https://dl.dropboxusercontent.com/u/1733921/Raspberry%20Pi/Schematics/RaspberryPi-Stopwatch.png"/>

###Source Code
<script src="http://gist-it.appspot.com/github/raspberrypi-aa/raspberrypi-aa/blob/master/Project2-Beginner.py"></script>



