---
layout: default
title: Python Requests 
date: October 1, 2013
---

## Making HTTP Requests with Python-Requests
Python includes a built-in HTTP client, urllib2, but its interface is clunky and hard to use. There exists a much better and easier to use HTTP client interface called [Python Requests](http://www.python-requests.org/en/latest/). Requests is not included with Python by default, so we must install it. 

## Installing Requests
{% highlight bash %}
pip install requests
{% endhighlight %}

## Making a GET request
The simplest use of this library is making an HTTP GET Request. More flexibility can be added in the form of query parameters, authentication, custom request headers, cookies, and more. Requests also provides a nicely formatted response object, complete with HTTP Status code (that error 404 everyone loves).

At its simplest, Requests is used as:
{% highlight python %}
import requests
r = requests.get('http://google.com')
print r.status_code
print r.headers
print r.text[0:1000]
{% endhighlight %}

To add on a query parameter, include a 'params' object in the request
{% highlight python %}
import requests
r = requests.get('http://google.com', params={'q': 'raspberry pi'})
{% endhighlight %}

To add on a header, include a 'headers' object in the request
{% highlight python %}
import requests
r = requests.get('http://google.com', headers={'X-Platform': 'RaspberryPi'})
{% endhighlight %}


If you are accessing a website that needs authentication (Basic, Digest, OAuth1), Requests can handle it as well. To use authentication, include an 'auth' object in the request. The authentication defaults to HTTP Basic (username/password in the clear) if one is not specified.
{% highlight python %}
import requests
r = requests.get('http://google.com', auth=('user', pass'))
{% endhighlight %}

### Creating a Forecast.IO account
_ This service is free up to 10k requests per day. No billing info is needed to sign up. _
1. Go to [http://https://developer.forecast.io/](http://https://developer.forecast.io/) and click the "Register" button in the top right corner.
2. Make note of your API Key on the bottom of the screen

### Requesting forecast.io data
Use Python-requests to get forecast data for a given location and then print out tomorrow's temperature

{% highlight python %}
import requests
import json

api_url="https://api.forecast.io/forecast/%s/%f,%f"
api_key="<your_api_key>"
lat=42.38205
long=-71.10517

query_url = api_url % (api_key, lat, long)
r = requests.get(query_url)
if r.status_code != 200:
  print "Error:", r.status_code

#print r.text
print "---"
print json.dumps(r.json(),
	sort_keys = True,
	indent=4)

currentWeather =  r.json()['currently']['icon']
if "cloud" in currentWeather:
  print "Cloudy"
elif "rain" in currentWeather:
  print "Rain"
else:
  print "Clear"
{% endhighlight %}

#### References
* [Python Requests](http://www.python-requests.org/en/latest/)
* [HTTP Status Codes](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)
* [Dark Sky API Docs](https://developer.forecast.io/docs/forecast)
