---
layout: default
date: September 22, 2013
title: Raspberry Pi Python Firmata
---

## Controlling an Arduino with PyFirmata

The Raspberry Pi is sometimes seen as competition to micro controllers like the Arduino, or PICs. However the Raspberry Pi has a different sweet spot and can easily be combined with an Arduino to accomplish a wider range of tasks than otherwise possible. The below table provides a comparison of the Raspberry Pi and an Arduino Mega.

### Raspberry Pi and Arduino Comparison

<table style="border: 0px">
<tr><th>Feature             </th><th>Raspberry Pi</th><th>Arduino (Mega)</th></tr>
<tr><td>Price               </td><td> $35        </td><td>     $60  </td></tr>
<tr><td>CPU   	             </td><td> 700Mhz     </td><td>	16Mhz    </td></tr>
<tr><td>Memory	             </td><td> 512MB	  </td><td> 256KB        </td></tr>
<tr><td>Storage	     </td><td> up to 64GB </td><td> None         </td></tr>
<tr><td>GPIO Ports	     </td><td> 17+4	  </td><td> 54           </td></tr> 
<tr><td>Analog Read	     </td><td> No	  </td><td> 16           </td></tr>
<tr><td>PWM	             </td><td> 1 channel  </td><td>	15 pins  </td></tr>
<tr><td>Ethernet	     </td><td> Yes	  </td><td> No (+ $35)   </td></tr>
<tr><td>USB                 </td><td> 2 Ports	  </td><td> No           </td></tr>
<tr><td>HDMI/Video Out	     </td><td> Yes	  </td><td> No           </td></tr>
<tr><td>Audio Out	     </td><td> Yes	  </td><td> No           </td></tr>
<tr><td>Graphics Processor  </td><td> Yes	  </td><td> No           </td></tr>
<tr><td>I2C                 </td><td> Yes	  </td><td> Yes          </td></tr>
<tr><td>SPI                 </td><td> Yes	  </td><td> Yes          </td></tr>
<tr><td>Realtime Processing </td><td> No	  </td><td> Yes          </td></tr>
<tr><td>Power	             </td><td>~700mA	  </td><td> 40mA         </td></tr>
</table>

While the Raspberry Pi is great at interfacing to other components or interacting with users (i.e. usb, display, ethernet), its also good for computationally expensive tasks like computer vision. Arduino is great at doing things with very strict timing constraints or with very little power. Arduinos are also good for compatibility with existing circuitry or Arduino specific shield (add-on boards such as motor control, MIDI interfaces, and more).
<br/>
To combine Raspberry Pi with an Arduino, you can use the Firmata protocol with Python bindings. Firmata is a serial communication protocol that can control the Arduino's GPIO ports, read analog inputs, and control PWM and Servo pins.

### Setting up your Arduino for Firmata

Firmata control of the Arduino requires loading an Arduino with the special Firmata sketch. You can download the Arduino software from the Arduino website. After opening the Arduino IDE, follow these steps to install Firmata on your Arduino:

1. Click File->Examples->Firmata->StandardFirmata
2. From the Tools->Board menu, select the type of Arduino you are using.
3. From the Tools->Serial Port menu, choose the USB port to which your Arduino is connected.
4. Click the upload button (it looks like a right arrow, just next to the checkmark) and wait for your sketch to upload. A message in the bottom black windowwill indicate success or failure
5. Once the Firmata sketch is loaded on your Arduino, you can test it out with the Firmata Test Program.

### Controlling your Arduino from Python

Next, your Raspberry Pi must be setup with the python firmata libraries. Run the following commands:

{% highlight bash %}
  sudo apt-get install python-pip python-serial
  sudo pip install pyfirmata
{% endhighlight %}
Use a USB cable to connect the Arduino with the Raspberry Pi (remember to use the big USB Standard A connector and not the smaller Micro B power connector). You can now find the USB name of the Arduino by running 'ls -lrt /dev/tty*'. On my Raspberry Pi, it was listed as /dev/ttyUSB0. Remember this value for later.

### Connecting to an Arduino

To control an Arduino from a Python script on your Raspberry Pi, you must first import the Arduino and util classes from the pyfirmata module. Then, create an object using the USB address you found in the previous step

{% highlight python %}
  >>> from pyfirmata import Arduino, util
  >>> board = Arduino('/dev/ttyUSB0')
{% endhighlight %}

### Controlling Arduino GPIO

The Arduino's digital input and output ports can be controlled using the board.digital[] list. Calling write() can set the pin values high or low (1 and 0 respectively). The read() method returns the current value of the pin.

{% highlight python %}
>>> board.digital[2].write(1)
>>> print board.digital[2].read()
{% endhighlight %}

If you'd like to use a pin repeatedly, its cumbersome to keep referring to it as board.digital[2]. Instead, you can get a reference to a pin with the board.get_pin() function. To this function, you pass a string of the format "[a|d]:[pin#]:[i:o:p:s]". It is split by colons into three sections. The first section determines if the pin will be used in analog or digital mode. The second section is the number of the pin you would like to use. The third section selects the pin mode between input, output, pwm, and servo. The returned pin can be assigned to a variable and then later used to call read() and write() methods.

{% highlight python %}
  >>> pin2 = board.getpin('d:2:o')
  >>> pin2.write(1)
{% endhighlight %}

### Controlling Analog Pins

To read the value on an analog pin, you have to first turn on the analog value reporting on that pin. However, this continuously sends the analog value from the Arduino to the Raspberry Pi. If not continuously read, this will clog up the serial connection and prevent the rest of your script from running properly. To read the values, it is helpful to use an iterator thread.

{% highlight python %}
   >>> it = util.Iterator(board)
   >>> it.start()
   >>> board.analog[0].enable_reporting()
   >>> board.analog[0].read()
   >>> it.start()
{% endhighlight %}

To turn off the reporting of analog values, call disable_reporting() on the pin object

### References
* [PyFirmata Library](https://github.com/tino/pyFirmata)
* [Firmata Documentation](http://firmata.org/)
