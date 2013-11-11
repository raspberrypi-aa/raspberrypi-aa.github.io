---
layout: default
title: Project 1 - Simon Says
date: September 4, 2013
---
# Project 1 - Simon

For this project, you will make a small Simon game. The Raspberry Pi will generate a random sequence of lights which the player will need to repeat back by pressing the correct button. For example, if the Pi blinks Red, Yellow, Yellow, the player will need to push the buttons corresponding to Red(Button 1), Yellow (Button 2), and Yellow(Button 2).

The given starter code contains several functions you will need to implement. Also, the seqLength variable controls the length of the sequence. You should start with this at 1 and then increase as you complete the missing functions.

You may also wish to reorganize the code, creating a new function to play a round of a given length. Then, put this function in a loop that increases the length of the round (seqLength) to increase the difficulty.

## Breadboad Layout
<img src="https://dl.dropboxusercontent.com/u/1733921/Raspberry%20Pi/Schematics/RaspberryPi-Stopwatch.png" alt="Stopwatch Breadboard Layout" width="300px" />

Use the layout from the Stopwatch sketch, but add additional LEDs to ports 14 and 15. Also, add additional switches to pins 17 and 21. You may try using the internal pullup resistors if you would like. If you want to use the internal pullup's you can remove the 1k pullup shown in the diagram.


### Source Code
You'll need to complete some of these functions in order for the program to work. These places are marked by "# IMPLEMENT THIS FUNCTION".
<script src="http://gist-it.appspot.com/github/raspberrypi-aa/raspberrypi-aa/blob/master/Project1-Intermediate.py?footer=0"></script>

[Solution Source Code](https://github.com/raspberrypi-aa/raspberrypi-aa/blob/b839e8b4e9e4ae2409257a986e29a3df6b9e7038/RaspberryPi_Toolbox/Project1-Intermediate.py) - Click to view
