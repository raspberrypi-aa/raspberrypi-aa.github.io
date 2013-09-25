---
layout: default
title: I2C - General Purpose Input Output Port Expander
date: September 24, 2013
---

## I2C - GPIO Port Expander

### What is I2C?

I2C (eye-squared-cee) is a communication protocol that the Raspberry Pi can use to speak to other embedded devices (temperature sensors, displays, accelerometers, etc). In this example, we'll be connecting an MCP23008 I/O expander to our Raspberry Pi. The I/O expander adds additional GPIO ports. These can be used as both inputs, and outputs at either 3.3V or 5V. This makes it an ideal level shifter chip for peripherals that are 5V and not natively compatible with the RPi's 3.3V GPIO ports. The MCP23008 can also generate interrupts based on input, but we won't be covering that here.

I2C is a two wire bus, the connections are called SDA (Serial Data) and SCL (Serial Clock). Each I2C bus has one or more masters (the Raspberry Pi) and one or more slave devices, like the I/O Expander. As the same data and clock lines are shared between multiple slaves, we need some way to choose which device to communicate with. With I2C, every device has an address that each communication must be prefaced with. The I/O Expander defaults to an address of 0x20, but it has 3 pins which can be used to change the address to any value up to 0x27. This means you can use up to 8 MCP23008s on a single I2C bus.

The Rasperry Pi has two I2C buses. One is available on the GPIO (P1) header, the other is only available from the P5 header. To access these, you'll need to solder on your own header pins.

### Preparing RPi for I2C

Confirm that the i2c modules are loaded and active:
{% highlight bash %}
pi@pi-friedrich ~ $ lsmod  | grep i2c
i2c_dev                 6276  0
i2c_bcm2708             4121  0
{% endhighlight %}

Install some i2c utilities:
{% highlight bash %}
sudo apt-get update
sudo apt-get install i2c-tools
sudo apt-get install python-smbus
{% endhighlight %}

i2c-tools includes some cool utilities, like i2cdetect, which will enumerate the addresses of all slave devices on a single bus. Try it out by running 'sudo i2cdetect -y 1' with the MCP23008 connected. Another utility, i2cdump lets you query the state of individual settings (registers) on a specific I2C device. This is the output you should see with the MCP23008 connected as below, and configured for the default address of 0x20:
{% highlight bash %}
pi@pi-friedrich ~ $ sudo i2cdetect -y 1
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: 20 -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --
{% endhighlight %}

### Connecting MCP23008
![MCP23008 Pinout](https://dl.dropboxusercontent.com/u/1733921/Raspberry%20Pi/MCP23008-Pinout.png)
![MCP23008 Breadboard Connections](https://dl.dropboxusercontent.com/u/1733921/Raspberry%20Pi/MCP23008%20I2C_bb.png)


### Controlling MCP23008

There is a great I2C Python library available from Adafruit. It is already available in the WebIDE if you are using that tool. Otherwise, the library can be downloaded from [Adafruit's GitHub Page](https://raw.github.com/adafruit/Adafruit-Raspberry-Pi-Python-Code/master/Adafruit_I2C/Adafruit_I2C.py). This file needs to be in the same directory as your python script. Bytes can be read and written from the I2C bus using code like the following:

{% highlight python %}
i2c = Adafruit_I2C(0x20)
i2c.write8(register, value)
i2c.write16(register, value)
value = i2c.readU8(reg)
{% endhighlight %}

You can see that all i2c commands have to be addressed to the MCP23008 device (address 0x20), but also to a specific register on the device. The registers on the MCP23008 are (from the data sheet):
0x00     IODIR       Controls input/output state of each of the 8 inputs. Setting a bit to 1 makes it an input

    0x01     IPOL
    0x02     GPINTEN     Enables interrupt on change
    0x03     DEFVAL      Default value to base interrupt on change for
    0x04     INTCON      Interrupt control - compare against previous value or compare against DEFVAL
    0x05     IOCON
    0x06     GPPU        Configures internal 100k pullup resistors for pins set as inputs
    0x07     INTF        Shows interrupt state of each port 
    0x08     INTCAP      Captures value of pin experiencing interrupt. Reading this clears the interrupt
    0x09     GPIO        State of each individual GPIO pnin
    0x0A     OLAT

Adafruit has already written a good interface library for the MCP23008 and its bigger brother:

{% highlight python %}
io = Adafruit_MCP2300X(0x20)
io.config(0, OUTPUT)
io.output(0, 1) #Set pin 0 high

io.config(2, INPUT)
io.pullup(2, 1)
x = io.input(2)
{% endhighlight %}


### Sample Code - Fill in the blanks

{% highlight python %}
#!/usr/bin/env python
#
# Uses the MCP23008 I2C GPIO Expander 
#
#

import Adafruit_I2C as I2C
import time
import sys

#if True:
#    mcp = Adafruit_MCP230XX(busnum = 1, address = 0x20, num_gpios = 16)
#    mcp.config(0, OUTPUT)
#    mcp.output(0, 1)

IODIR = 0x00 # Set bit to 1 to make it an input
GPIO = 0x09  
GPPU = 0x06 # Set bit to 1 to turn on internal pullup

# Setup Adafruit_I2C library with default address of 0x20


def setAllInput():
    '''Set IODIR register to 0xFF'''
    pass
    
def setAllOutput():
    '''Set IODIR register to 0x00'''
    pass
    
def setPinMode(pin, input, pullup=False):
    '''Pin is the pin number to modify input/output state of.
       Input is True to set the pin as input, False to set it as output'''
    pass
    
    # Change only the value of the bit in the IODIR register given by 'pin'


    # If pin is set to an input, set pullup into the right mode 

def setPin(pin, state):
    '''Change the value of the GPIO register for the pin specified'''
    pass

def readPin(pin): 
    '''Read in the value of the GPIO register for the pin specified'''

if __name__ == '__main__':
    try:
        setAllOutput()
        
        # Set Pin 1 to Input w/ internal pullup enabled
        setPinMode(1, True, True)
        

        # Test output pins - LED Blink
        if True:
            while True:
                setPin(7, True)
                time.sleep(1)
                setPin(7, False)
                time.sleep(1)
      
        if True:
            while True:
                print "IN" , str(readPin(1))
                time.sleep(1)
    
    except KeyboardInterrupt:
        sys.exit(0)

{% endhighlight %}

[Solution Source Code](https://github.com/raspberrypi-aa/raspberrypi-aa/blob/master/RaspberryPi_Toolbox/i2c_mcp23008_test.py)