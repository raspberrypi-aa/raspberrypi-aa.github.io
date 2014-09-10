---
layout: default
title: Session 2- Polled and Interrupt Driven Input
date: September 4, 2013
---
## Inputs on the Raspberry Pi
This is the circuit to add an input switch to the Raspberry Pi:<br/>
<img src="https://dl.dropboxusercontent.com/u/1733921/Raspberry%20Pi/Schematics/RaspberryPi-Stopwatch.png" />

There is a switch connected to ground on one side and to +3.3V on the other side. The connection to 3.3V is via a 10k "pull-up" resistor. This resistor limits the amount of current flowing through the switch when the button is pressed. 

## Internal Pull-ups and Pull-down Resistors
The Raspberry Pi has internal 50k Ohm pull up and pull down resistors on many of its GPIO pins. These internal resistors can be activated when setting a GPIO pin as an input as we will see below. 

* If using the pull-up resistor, no external resistor is needed and the switch should be connected between GPIO pin and ground
* If using the pull-down resistor, no external resistor is needed and the switch should be connected between GPIO pin and 3.3V

{% highlight python %}
  GPIO.setup(pin, GPIO.IN, pull_up_down=GPIO.PUD_UP);
#                or
  GPIO.setup(pin, GPIO.IN, pull_up_down=GPIO.PUD_DOWN);
{% endhighlight %}

## Polled and Interrupt Driven Input Methods

Input on the Raspberry Pi can be done in two modes, polled and interrupt. Polling for input means checking for input periodically on an as needed basis. Interrupt driven input means taking an action when an input changes in a desired way. 
Polled input is the simplest method of input on the Raspberry Pi. For a desired pin, the state of the input pin is checked for the input value. If the input value changes, the program can change its behavior. The simplest example of polled input is:

{% highlight python %}
  GPIO.setup(pin15, GPIO.input)

  while GPIO.input(pin15) == 1:
     time.sleep(.01)

  print "Pin 15 set low"
{% endhighlight %}

In the above program, pin 15 is originally high. When the button is pressed, pin 15 goes low (a value of 0) and the while loop exits. The message is printed to the output. The "time.sleep(.01)" command is used to add a slight delay to the program, giving other tasks on the system time to run. Without the delay, the program will drive the CPU usage of the Raspberry Pi up to 100%, possibly slowing other programs down or even preventing them from running. With the above code, it is technically possible that a button press can be missed if the signal goes to 0 and then returns back to 1 before the input state is checked again. This is especially likely if the timeout is longer than 10ms, the input is a pulse shorter than 10ms, or there are other actions occurring in the while loop. 

### wait\_for\_edge()
The GPIO library provides a cleaner way of doing polled input, using the `wait_for_edge()` function. Using `wait_for_edge()` eliminates the need for the while loop or the delay. There is little to no chance of missing an input event here.

{% highlight python %}
  GPIO.wait_for_edge(pin15, GPIO.RISING)
  print "Pin 15 set low"
{% endhighlight %}

The second parameter to the `wait_for_edge()` can be either GPIO.RISING, GPIO.FALLING, or GPIO.BOTH. These describe either rising (logic low to logic high) or falling (logic high to logic low) levels, or either for GPIO.BOTH. We will see this values used in the interrupt driven input calls as well. 

### event_detected()
A mix between polled and interrupt driven input uses the `event_detected()` function. While your program explicitly checks if an input has changed, it is implemented inside the GPIO library using interrupts. To use the `event_detected()` function, you have to first call `GPIO.add_event_detect(pin, GPIO.RISING/FALLING/BOTH)`. Then, calling `GPIO.event_detected(pin)` will return True or False, depending on if the given event has occurred since the last time `event_detected()` was called. This is a good way to ensure that no input events such as button presses are missed. Your program can check periodically (once every second or so) so it can spend its time doing other things. 

### add\_event\_detect() Callback Mechanism
The final, and arguably most powerful form of input is interrupt driven. You specify a function that is run (i.e. called) as soon as the input changes state. This function is known as a callback function. The GPIO library keeps a separate thread for all interrupt callbacks, and the callback functions are serialized (run one at a time) on this separate thread. To use these interrupts, first setup the callback and associate it to the pin using `add_event_detect()`. You can associate multiple callbacks with the same input pin:

{% highlight python %}
def callback_function_print(input_pint): 
  print "Input on pin", input_pin

GPIO.add_event_detect(pin15, GPIO.BOTH, callback=callback_function_print)

{% endhighlight %}
Noe that the function named `callback_function()` has to take 1 argument as input. This will be an integer containing the number of the input pin activated. This will allow you to share one function between multiple input pins. Also keep in mind that `callback_function()` will be run on a separate thread of execution, so be careful modifying or reading variables from the main thread. Use locks or queues if necessary!


### For more info:
* [RPi-GPIO Python Inputs](http://sourceforge.net/p/raspberry-gpio-python/wiki/Inputs/)
* [Python Queue Docs](http://docs.python.org/2/library/queue.html)
