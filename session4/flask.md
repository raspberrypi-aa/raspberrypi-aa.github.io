---
layout: default
title: Python Flask Framework
date: October 1, 2013
---

## Running a Raspberry Pi Webserver with Flask

Most communication on the web is run over a protocol known as Hypertext Transfer Protocol (or HTTP for short). It is a request/response protocol between a server and a client. A web browser(client) makes a request for some data to a server (our Raspberry Pi in this case). The server then responds with the content of the page. 

HTTP has several types of requests (or methods) in it. We'll be dealing with the two main ones today: GET and POST. GET retrieves data (it never changes the contents of the requested page) from the server and POST sends data to the server. There are also HEAD, PUSH, and DELETE, among others.

While its possible and pretty straightforward to write your own HTTP server code, Python already has several frameworks that handle all the nitty-gritty of the protocol for us. With Flask, all you need to worry about is how your program behaves in response to an HTTP request. Some alternatives to Flask are CherryPy, Django, Pylons, Zope. I find Flask to be the simplest and lightest weight, so thats what I'll be showing here.

### Installing Flask
Installing Flask is a cinch, but I recommend using a virtualenv because it will pull in a bunch of other pre-prequisites(Werkzeug,Jinja2,itsdangerous,markupsafe) as part of the installation. As always, this will take a few minutes.
{% highlight bash %}
sudo apt-get install python-pip # If you have not already done so
virtualenv FlaskEnv
source FlaskEnv/bin/activate
pip install Flask
{% endhighlight %}


### Creating a Flask application
The first task when using Flask is to create a Flask application and start the srver. By default, the Flask server will be in debug mode. In debug mode, Flask will only be accessible from the Raspberry Pi, not from any other devices. Debug mode will also turn on crash dumps, to make debugging problems easier. To access Flask from any other device (Laptop, smartphone, etc), you must start Flask with the command "app.run(host="0.0.0.0")". By default, Flask will run on port 5000, which you will need to enter as part of the URL to access Flask. 

{% highlight python %}
#!/usr/bin/env python
from Flask import Flask, request, Response

# Create the server
app = Flask(__name__)

if __name__ == '__main__':
   app.debug = True
   app.run()  
   #app.run(host="0.0.0.0")

{% endhighlight %}

This is the most basic Flask program one could write, but it won't do anything yet. To respond to requests, you must create a handler function. Our first handler function will simply write "Hello World" back to the client without any fancy HTML formatting. The funny-looking line starting with @ is called a decorator. Its job is to choose which function runs when a URL is accessed. To run this function, you would enter http://<pi-ip>:5000/ into your web browser. 

{% highlight python %}
# Define handlers for each path
@app.route('/')
def root():
    return "Hello World"
{% endhighlight %}

### GET Parameters
  There are two ways to pass parameters to a webpage, URL path parameters and query parameters. An example of a URL with a path parameter is http://example.com/securitySystem/sensorState/15. Here, the URL is requesting information about sensor 15. Depending on how the program is structured, this could be alternately requested as http://example.com/securitySystem/sensorState?sensor=15. In this example the query parameters are everything after the ? is makes up a set of key/value parameters. Here the key is "sensor" and the value is "15". 

#### Path Parameters
To use a URL path parameter, you must tell Flask where in the URL the parameters lie. The parameter will now be passed to the handler function as an argument, instead of using the requests.args object. You can specify the type of the parameter (i.e. int, string), otherwise it will be left as a string. 

{% highlight python %}
@app.route('/echoParam2/<parameter>')
def echoParam2(parameter):
    return parameter
{% endhighlight %}

#### Query Parameters
To access the query params, Flask provides the request object, which has an args dictionary. Desired keys can be looked up in the args dictionary. If the key is missing, Python will throw a KeyError exception (like all dictionaries). 

{% highlight python %}
# Test with: curl http://localhost:5000/echoParam?k=v\&k1=v1
@app.route('/echoParam')
def echoParam():
    try:
        return str(request.args['k'])
    except KeyError:
        return "Missing parameter"
{% endhighlight %}

### Posting Data
So far we have dealt with retrieving information from the Raspberry Pi, but what if you want to send data to it? For example, you might want to send a command to turn on a light remotely or upload new pictures of cats for safekeeping. To send data to a server via HTTP, we must use an HTTP POST (PUT would work as well). Just as we need to ask Flask to send us requests for a particular URL using the route decorator, we expand that to include the method of the request as well. All of the data posted is available in request.form dictionary.

{% highlight python %}
# Test with:  curl --data key=val --data key2=val2 -X POST http://localhost:5000/postTest
@app.route('/postTest', methods=['POST'])
def postTest():
    postData = request.form['key']
    return str(postData)
{% endhighlight %}

### Templates
Once you have your application all put together and working, you'll probably want to pretty it up. This means using HTML to create a good looking page. Flask separates the presentation (look and feel) of the application from its content and behavior using templates. HTML Templates are a separate file(found in the templates/ directory) that contain what the page should look like and how the results of the processing should be used. <br/>

Templates can even contain conditionals and other bits of programming to change their looks based on Python's output.

This template will output a simple Python variable called name. It will also show either a Happy Birthday message or a slightly meaner message, depending on the value of the birthday boolean. 

{% highlight html %}
<html>
  <head>
    <title>Template Test</title>
  </head>
  
<p>{{ name }} </p>

{% if birthday %}
  <p>Happy Birthday</p>
{% else %}
  <p>NO Birthday</p>
{% endif %}

</html>
{% endhighlight %} 

The Flask code to make use of the template looks the same as all other Flask code described, with the addition of the render_template() function. render_template() is passed several arguments. First the filename of the template, followed by all variables that will be available to the template.

{% highlight python %}
from Flask import render_template
@app.route('/template')
def template():
    return render_template('template_test.html',
        name='Asylum', birthday=False)
{% endhighlight %}


## References
* [Flask Documentation](http://flask.pocoo.org)
* [Jinja2 Template Documentation](http://jinja.pocoo.org/docs/dev/templates/)
* [Flask Example Code](https://github.com/raspberrypi-aa/raspberrypi-aa/blob/master/FlaskTest.py)
