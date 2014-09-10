---
layout: default
title: Module 2- Home Automation Case Study
date: September 10, 2014
---

# Home Automation Case Study

## Preqreqs

* Need Flask installed
* Need Flask [Bootstrap](http://www.getbootstrap.com) module installed
    * Bootstrap is a piece of software from Twitter that makes it easy to create slick looking websites with little effort.
{% highlight bash %}
sudo pip install flask-bootstrap
{% endhighlight %}

* Need RPi.GPIO installed  
* Get the class source code (either using git clone or WebIDE)
* Be sure to build the part of the circuit on your board before testing the code that uses those pins


### Running the webserver
{% highlight bash %}
  $ cd FlaskGpioTest
  FlaskGpioTest$ sudo ./flaskGpio.py
   * Running on http://0.0.0.0:5000/
   * Restarting with reloader
{% endhighlight %}


## Flask Practice
1. Point your computer's web browser at http://piname.local:5000/. You should see a webpage with a "Test" button on it. 
2. Clicking the "Test" button will make a request to "/test". If you click the button, it should fail with a 404 error, because no handler exists for the "/test" path
3. Edit the flaskGpio.py file to add a handler for "/test" that will display a successful message to the user. You can display a message by setting the "successMsg" parameter in the call to render_template() 
4. Clicking the button should now display a success message in the browser

## Click to turn lamp on/off

Use the "Lighting" tab on the left for this section.

1. Before we can use the RPi.GPIO library, you must import it first. This should happen at the top of the source file
2. Next, call RPi.GPIO.setmode() to use the board in Broadcom (BCM) mode. This will match the numbers on your breakout board. This should be done as part of the server setup, but before starting the server
3. Call RPi.GPIO.setup(pin#, GPIO.OUT) to set the lamp pin (#18) as an output. This should be done immediately after the code for step #2
4. As part of the request handler for clicking the "On" and "Off" buttons, use RPi.GPIO.output(pin#, GPIO.HIGH/GPIO.LOW) to turn the LED on or off
{% highlight python %}
@app.route('/lighting/on')
def lighting_on():
  """Turn the lamp on by setting LAMP_PIN to high. """
  GPIO.output(LAMP_PIN, GPIO.HIGH)
  lightingData={}
  lightingData['active'] = True
  return render_template('flaskGpioTemplate.html', page_title="Lighting", lighting=lightingData) 
{% endhighlight %}

## Set schedule to turn the coffee maker on/off

1. Before using any timer, you must import the threading library
2. To run a timer, create a timer object containing the remaining time and function to run when the timer expires. 
3. Call timer.start()
4. To stop the timer, call timer.stop()
5. You should set the timer in coffee_set() and cancel the timer in coffee_stop()

{% highlight python %}
import threading

def expired():
  print "Timer Expired"

t = threading.Timer(30, expired) #30 seconds
t.start()

# To cancel:
t.cancel()

{% endhighlight %}

## Check if dryer still running
1. Set the dryer pin as an input pin in the setup function.
2. In appliances(), read the state of the input pin. 
3. Put the state of the pin in applianceData['dryer']
4. You may want to add this code to coffee\_set() and coffee\_stop() as well

## Check if doorbell rung since last check
1. Set the doorbell pin as an input pin in the setup function
2. Call add\_event\_detect() for the doorbell\_pin 
3. In the exterior() function, call event\_detected() on the doorbell\_pin. 
4. If the doorbell has been pushed, set exterior['doorbell'] to False

## Track the time the mailman shows up
1. Write a callback function (`mailboxCallback()`) that will record the current time into lastMailboxPush
2. Set the mailbox pin as an input pin in the setup function
3. Call add_event_detect() for the mailbox pin and provide the name of the callback from step 1


