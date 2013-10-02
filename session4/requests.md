---
layout: default
title: Python Requests 
date: October 1, 2013
---

## Making HTTP Requests with Python-Requests
Python includes a built-in HTTP client, urllib2, but its interface is clunky and hard to use. There exists a much better and easier to use HTTP client interface called [Python Requests](http://www.python-requests.org/en/latest/). Requests is not included with Python by default, so we must install it. 

## Installing Requests
Still using a virtual environment:

{% highlight bash %}
virtualenv requests
source requests/bin/activate
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


## References
* [Python Requests](http://www.python-requests.org/en/latest/)
* [HTTP Status Codes](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)
