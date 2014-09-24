---
layout: default
title: Motor Control
date: September 17, 2014
---


## Motor Control
 All motors convert electricity into rotary motion. The shaft of the motor may be connected to gears (making it a gear motor) or other mechanical parts. Whenever using a motor, there is the problem of controlling it. Some common questions in motor control are:

* How fast is my motor turning? How do I control its speed?
* How far has the load traveled (i.e. when should I stop the motor)?
* How accurately did the motor move?

There are three main classifications of motors that one could connect to a Raspberry Pi.

1. DC Motor
2. Stepper Motor
3. Servo Motor

#### DC Motors
DC Motors are the easiest to use, but are also the least precise. They can be as small as a vibrating pager motor or as large as a motor for an electric vehicle (scooter, bicycle, Tesla Model S). DC Motors are powered with only 2 connections, a power connection and a ground connection. The higher the voltage across the motor, the faster the motor will spin. Most hobby motors run between 5V and 24V. A downside of DC motors is their lack of feedback about position, this is called open loop operation. Sometimes, other mechanisms will be used to measure how much the motor has spun (and how far something has moved). One of the more popular approaches to this is a rotary encoder. Encoders are often attached to the shaft of the motor and can measure how much the motor has moved. Higher quality encoders (so called quadrature encoders) can even measure the direction of motion. 

#### Stepper Motors
A more sophisticated type of motor, a stepper motor, gives more fine grained control. Instead of only two connections, steppers can have up to six. Connections are paired together, which energize parts of the motor winding in a specific order, causing the shaft to spin. Steppers must be driven by special motor controllers that send pulses to the different parts of the motor. Each pulse moves the motor a very precise amount. Steppers are often rated in steps per revolution. For example, a typical stepper may have 120 steps per revolution, moving 3 degrees every pulse. Faster pulses will cause the motor to spin faster. 
<img src="http://upload.wikimedia.org/wikipedia/commons/6/67/StepperMotor.gif" style="width: 250px;" />

Most printers (inkjet and 3D) are controlled by stepper motors. They can often be harvested from discarded office equipment, if you are willing to do some extra work to find the datasheet or otherwise determine the order of connections. While steppers are more precisely controlled, they still provide no feedback about their position. Depending on situation, is possible for a stepper motor to "slip", missing steps and ending up in an incorrect position. 


#### Servo Motors
The final type of motor, a servo motor combines a DC motor with built-in feedback. Unlike servo and stepper motors, most servos have a limited range of motion. They do not rotate continuously, rather moving only about 180 degrees or so, and are limited at each end of travel. This is similar to hitting the limits of turning the steering wheel in your car. Indeed, servo motors are often used for steering RC cars and planes. They also typically come with gearing already provided. They are most popular in hobbyist use, so while you might find a DC motor big enough to run a lawn mower, large and powerful servos are not widely available.

Like steppers, servo motors are also controlled by pulses. Servo motors do not spin freely, so the speed of pulses does not control speed of the motor. 

Read more about [driving servos](/session3/pwm-servo.html)

