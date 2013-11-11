---
layout: default
title: Python Virtual Environments
date: October 1, 2013
---


# Python Virtual Environments (virtualenv)
* Testing different versions of libraries (one API might require python-requests v3 another might need v2)

### Installing virtualenv
{% highlight bash %}
sudo pip install virtualenv
{% endhighlight %}


### Using your first virtualenv
To create your first virtualenv, run:
{% highlight bash %}
virtualenv flaskenv
{% endhighlight %}
This will create a virtual environment without any of the packages installed on your Raspberry Pi. If you want to keep the packages already installed, add the "--system-site-packages" option to the virtualenv command.

To begin using the virtualenv, change into the new directory and run:
{% highlight bash %}
pi@ericpi ~/flaskenv$ cd flaskenv
pi@ericpi ~/flaskenv$ source bin/activate
(flaskenv)pi@ericpi ~/flaskenv $
{% endhighlight %} 

Notice that the command prompt changes, once you enter the venv so you know where you are. The (flaskenv) will disappear once you exit the virtualenv later.

At this point, any python libraries you install will only be installed in that virtual environment and will not affect the rest of the system. To see what packages you have installed, run: 

{% highlight bash %}
# If installing yolk outside the venv, prefix the command with "sudo"
easy_install yolk
yolk -l
{% endhighlight %}
On my system, there are only 5 packages present in flaskenv,  while in a virtualenv created with --system-site-packages there are a bit over 10. Remember that installing yolk only affects the current virtualenv, so to run it again on your base system, you will need to do easy_install again. 

To use the virtualenv in a python script, change the #!/usr/bin/python at the top of the file to #!<path_to_venv>/bin/python

To get out of the virtualenv, run "deactivate" at the bash prompt. 
