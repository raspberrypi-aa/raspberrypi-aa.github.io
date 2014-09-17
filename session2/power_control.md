---
layout: default
title: Module 2 - Power Control with Transistors and Relays
date: September 10, 2014
---


# Power Control with Transistors and Relays

The Raspberry Pi can provide about 4mA from each of its GPIO pins. This is about enough power to turn on an LED or a transistor. It is not enough power to run a motor or control a relay. If you want to do more than just lighting up an LED, you will need to add some more circuitry to your RPi output pins. Most likely, this will be a transistor, which can take the Raspberry Pi's 4mA of current and amplify up that to a much more useful ~100mA. This is in contrast to an Arduino, which can output ~40mA on its basic output pins. 

## Switching a Transistor with Raspberry Pi
If you want to use more than 4mA, you can use a transistor on the output of your Raspberry Pi. This particular example will use a Bipolar Junction Transistor (BJT), which is good up to a few hundred milliamps. For more than that, there are bigger, beefier Field Effect Transistors (MOSFET, JFET, etc..). BJT transistors have three terminals, Base, Collector and Emitter. In general, current flows from collector to emitter, and the base controls the amount of this current flow. There are two types of BJT transistors, NPN and PNP, referring to the materials they are made out of. The main difference is this:
* NPN transistors turn on when the base is brought low (i.e. connected to ground)
* PNP transistors turn on when the base is brought to high (relative to collector voltage) 

Finally, transistors work well for DC current only (not AC)!

Sample Schematic:


### Switching a relay with a Raspberry Pi
Relays are good for switching large amount of current (or any AC signal). But with more power, comes more danger.

**If you'd like to experiment with switching AC mains (wall -outlet) electricity, I highly recommend the [Powerswitch Tail](http://www.powerswitchtail.com/Pages/default.aspx). It can be controlled directly from the Raspberry Pi without the need for any transistors at all. 
