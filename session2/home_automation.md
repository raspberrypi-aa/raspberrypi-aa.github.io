---
layout: default
title: Module 2- Home Automation Case Study
date: September 10, 2014
---

# Home Automation Case Study

## Preqreqs
* Need Flask installed
* Need Flask [Bootstrap](http://www.getbootstrap.com) module installed
* Need RPi.GPIO installed  
* If using a virtual environment, the above packages must be installed inside that virtualenv. (Make sure you run `pip install <package>` inside the virtual environment. Do not use sudo inside the virtual environment, _sudo will install a package systemwide, but not in the virtualenv!_)
** Bootstrap is a piece of software from Twitter that makes it easy to create slick looking websites with little effort.
{% highlight bash %}
# If you are using a virtualenv, preface the below command with 'sudo'
pip install flask-bootstrap
{% endhighlight %}

### Running the webserver
{% highlight bash %}
  $ cd FlaskGpioTest
  FlaskGpioTest$ sudo ./flaskGpio.py
   * Running on http://0.0.0.0:5000/
   * Restarting with reloader
{% endhighlight %}

## Click to turn lamp on/off

## Set schedule to turn lamp on/off

## Check if washer/dryer still running

## Check if doorbell rung since last check

## Track (and graph!) time mail is delivered at each day

## Bonus: SMS (via Twilio) or Tweet when mail arrives