---
layout: default
title: Twitter API Example
date: October 1, 2013
---

# Twitter API Example

## Prerequisite Installation
In a virtual environment, run:
{% highlight bash %}
   pip install requests requests_oauthlib
{% endhighlight %}

## Obtaining an API key
To use the Twitter API, four pieces of information are needed for OAuth: Consumer Key, Consumer Secret, Access Token, Access Token Secret. These are obtainable from Twitter's development website at [http://dev.twitter.com](http://dev.twitter.com). Login with your normal twitter username/password, you can create and delete multiple applicatons under the same account. In the top right corner is your profile picture. Clicking the picture and then "My Applications" will bring you to the page of all existing API keys. Click the button to create  a new application and enter the required information. You will be given the OAuth parameters. If you ever lose these or they are made public, the keys can be changed or revoked from the same website. If you want to post new tweets (instead of just searching/reading tweets), you must change the Access Type to Read/Write on the Settings page and Then "Recreate the Access Token" back on the main page. 

## Source Code
<script src="http://gist-it.appspot.com/github/raspberrypi-aa/raspberrypi-aa/blob/master/TwitterTest.py?footer=0"></script>


## References
* [Python-twitter library](https://github.com/bear/python-twitter)
* [Twitter API Docs](https://dev.twitter.com/)
