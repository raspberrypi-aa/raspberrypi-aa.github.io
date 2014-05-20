---
layout: default
title: Session 2- PWM and Servo Control
date: September 15, 2013
---

## PWM and Servo Control
  Using the GPIO ports provides binary on/off control. Often, more moderate control is required. Dimming LEDs or controlling motor speed are two such examples. One possible way to slow down a motor is to run it on lower voltage (3 volts instead of 5). Lowering the voltage is not possible with the Raspberry Pi (or most microcontrollers) without specialized circuitry. Instead, the Raspberry Pi switches the output on and off very rapidly, making it appear to the motor like the voltage is lower. This technique is called Pulse Width Modulation (PWM). 
  In addition to motor speed control, PWM can also be used to steer a servo motor. Servos are a special class of motors, which do not (usually) spin continuously, but rather set an output angle (position). They are often used for steering robots or adjusting control surfaces on RC planes. 
<br/><br/>
  The Raspberry Pi contains a single hardware PWM/servo driver. The pure hardware PWM from the hardware driver provides a very accurate and clean PWM signal. Support for this driver is limited, currently supported by the semi-outdated Adafruit Occidentalis distribution and the more up to date WiringPi library. As there is only a single PWM channel, only 1 servo can be controlled. 
<br/><br/>
To control more than 1 servo, some timing accuracy can be traded off for more channels. Libraries like RPIO and Servoblaster (what we'll use today) are not a pure hardware solution (they are driven by DMA transfers). Their signal is slightly less clean and might be affected by other programs running on the Raspberry Pi.

## Servoblaster
Installing Servoblaster:<br/>
1. git clone /github.com/richardghirst/PiBits.git <br/>
2. cd PiBits/ServoBlaster/user <br/>
3. make <br/>
4. sudo make install <br/>
5. Verify that servoblaster is installed: ls /dev/servoblaster <br/>

Servoblaster is controlled by writing into the /dev/servoblaster file. The content is written as `<servo-number>=<servo-position>`. The first field written is the servo number. The following table shows which output pin each servo channel is connected to. 
<table>
<tr><th>Servo number</th><th>GPIO number</th><th>Pin in P1 header</th></tr>    
<tr><td>0</td><td>4</td><td>P1-7</td></tr>     
<tr><td>1</td><td>17</td><td>P1-11</td></tr>
<tr><td>2</td><td>18</td><td>P1-12</td></tr> 
<tr><td>3</td><td>21/27</td><td>P1-13</td></tr>  
<tr><td>4</td><td>22</td><td>P1-15</td></tr>  
<tr><td>5</td><td>23</td><td>P1-16</td></tr>  
<tr><td>6</td><td>24</td><td>P1-18</td></tr> 
<tr><td>7</td><td>25</td><td>P1-22</td></tr>
</table>
The allowable position values depend on your servo, for mine values between 80 and 249 were accepted. The servo specification often provides the number of steps the servo supports. 

####Source Code
<script src="http://gist-it.appspot.com/github/raspberrypi-aa/raspberrypi-aa/blob/master/servoblaster.py"></script>

###Appendix A: Adafruit Occidentalis PWM Instructions

The steps to set up the PWM driver are as follows (must be run as root, i.e. with sudo):

1. Set the frequency in the `/sys/class/rpi-pwm/pwm0/frequency` file. Somewhere in the 1000Hz-5000Hz range is recommended
2. Turn on the PWM by writing a value of 1 into the `/sys/class/rpi-pwm/pwm0/active` file
3. Adjust the duty cycle by writing a value between 1-100 in the `/sys/class/rpi-pwm/pwm0/duty` file

####PWM Example Code
<script src="http://gist-it.appspot.com/github/raspberrypi-aa/raspberrypi-aa/blob/master/RaspberryPi_Toolbox/pwm_test.py?footer=0"></script>


### Servo Control
Controlling a servo is very similar to controlling PWM. The same comments apply to the use of hardware or software servos as for PWM. The biggest difference is that the Raspberry Pi must be specifically put into servo mode (it defaults to pwm mode, so no change is required for pwm). Then, instead of writing to the "duty" file for control, instead the "servo" file is used. The units of the servo file are in steps from 0-32. A setting of 0 will turn the servo to 0 degrees, while a setting of 32 will turn the servo to 180 degrees. The maximum servo value can be changed by modifying the "max_servo" file, but this is not usually necessary.

####Servo Example
<script src="http://gist-it.appspot.com/github/raspberrypi-aa/raspberrypi-aa/blob/master/RaspberryPi_Toolbox/servo_test.py"></script>