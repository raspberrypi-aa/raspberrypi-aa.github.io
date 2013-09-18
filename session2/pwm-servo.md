---
layout: default
title: Session 2- PWM and Servo Control
date: September 15, 2013
---

## PWM and Servo Control

The Raspberry Pi contains a single servo and PWM driver. A special PWM kernel module, only available from the Occidentalis distribution (rpi_pwm) is required to us the hardware PWM. Additional PWM channels are available through RPIO or ServoBlaster.
<br/>
All hardware Servo and PWM output from the kernel module comes out of Pin 18 on the Raspberry Pi.
<br/>
The pure hardware PWM from the kernel module provides a very accurate and clean PWM signal. Libraries like RPIO and Servoblaster are not a pure hardware solution (they are driven by DMA transfers). Their signal is slightly less clean and might be affected by other programs running on the Raspberry Pi. A fully software implementation of PWM (like that in the latest RPi.GPIO package) uses lots of CPU power. Soft-PWM is also very susceptible to being disrupted by other tasks running on the Raspberry Pi. Try to avoid Soft-PWM if you can. If only one channel of PWM is needed and the kernel module is available, go for that. Otherwise, RPIO or Servoblaster is recommended.

###PWM Instructions

The steps to set up the PWM driver are as follows (must be run as root, i.e. with sudo):

1. Set the frequency in the /sys/class/rpi-pwm/pwm0/frequency file. Somewhere in the 1000Hz-5000Hz range is recommended
2. Turn on the PWM by writing a value of 1 into the /sys/class/rpi-pwm/pwm0/active file
3. Adjust the duty cycle by writing a value between 1-100 in the /sys/class/rpi-pwm/pwm0/duty file

####PWM Example Code
<script src="http://gist-it.appspot.com/github/raspberrypi-aa/raspberrypi-aa/blob/master/RaspberryPi_Toolbox/pwm_test.py?footer=0"></script>

### Servo Control

Controlling a servo is very similar to controlling PWM. The same comments apply to the use of hardware or software servos as for PWM. The biggest difference is that the Raspberry Pi must be specifically put into servo mode (it defaults to pwm mode, so no change is required for pwm). Then, instead of writing to the "duty" file for control, instead the "servo" file is used. The units of the servo file are in steps from 0-32. A setting of 0 will turn the servo to 0 degrees, while a setting of 32 will turn the servo to 180 degrees. The maximum servo value can be changed by modifying the "max_servo" file, but this is not usually necessary.

####Servo Example
<script src="http://gist-it.appspot.com/github/raspberrypi-aa/raspberrypi-aa/blob/master/RaspberryPi_Toolbox/servo_test.py"></script>
