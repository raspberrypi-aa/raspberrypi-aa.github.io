---
layout: default
title: Module 2 - Power Control with Transistors and Relays
date: September 10, 2014
---


# Power Control with Transistors and Relays

### a.k.a How to connect motors, lamps, etc...

The Raspberry Pi can provide about 4mA from each of its GPIO pins. This is about enough power to turn on an LED or a transistor. It is not enough power to run a motor or control a relay. If you want to do more than just lighting up an LED, you will need to add some more circuitry to your RPi output pins. Most likely, this will be a transistor, which can take the Rasberry Pi's 4mA of current and amplify up that to a much more useful ~100mA. This is in contrast to an Arduino, which can output ~40mA on its basic output pins. 

## Switching a Transistor with Raspberry Pi
If you want to use more than 4mA, you can use a transitor on the output of your Raspberry Pi. This particular example will use a Bipolar Junction Transistor (BJT), which is good up to a few hundred milliamps. For more than that, there are bigger, beefier Field Effect Transistors (MOSFET, JFET, etc..). BJT transistors have three terminals, Base, Collector and Emitter. In general, the base controls the amount of this current flow. There are two types of BJT transistors, NPN and PNP, referring to the materials they are made out of. The main difference is this: 

* NPN transistors turn on when the base is brought high (relative to collector voltage). The current flows from collector to emitter.
* PNP transistors turn on when the base is brought low (i.e. connected to ground). The current flows from emitter to collector. 

You can even use a different voltage across the transistor than the base is running on (i.e. 3.3V on base, but transistor is switching 5V, 10V, 24V). 

Finally, transistors work well for DC current only (not AC)!

_Sample Transistor Schematic:_<br/>
<img src="/session2/TransistorSchematic.png" style="width: 280px" />


### Switching a relay with a Raspberry Pi
Relays are good for switching large amount of current (or any AC signal). But with more power, comes more danger.

__If you'd like to experiment with switching AC mains (wall-outlet) electricity, I highly recommend the [Powerswitch Tail](http://www.powerswitchtail.com/Pages/default.aspx). It can be controlled directly from the Raspberry Pi without the need for any transistors at all.__

If you do decide to use a relay, you will need to add on a diode to the above circuit. This diode will ensure that a voltage spike when turning the transistor off will not damage the transistor.<br/>
_Sample Relay Schematic:_<br/>
<img src="/session2/RelaySchematic.png" style="width: 280px" />


#### References
* [RPi eLinux Interface Circuit](http://elinux.org/RPi_GPIO_Interface_Circuits)
* [Using Bipolar Transistors As Switches](http://www.rason.org/Projects/transwit/transwit.htm)