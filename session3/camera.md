---
layout: default
title: Raspberry Pi Camera
date: May 14, 2014
---

## Raspberry Pi Camera
  The Raspberry Pi has the unique ability among hobbyist embedded systems to interface with a camera. As the Raspberry Pi is based around essentially a smartphone CPU, it uses a camera very similar to a cellphone camera.
  Some other embedded platforms (Arduinos, PICs, etc...) are too slow to make use of live video feeds. Other platforms (Beaglebone for example) are fast enough to process video, but it places a large burden on the CPU. The Raspberry Pi's graphics processor is responsible for handling all camera data so the CPU doesn't have to. This means the camera can record high quality FullHD (1080p30) video without stressing the Raspberry Pis CPU, leaving it available for other tasks (motor control, machine vision, serving Internet requests). For still pictures the camera has a resolution of 2592x1944 (5 Megapixels). 
  There are two variants of the Raspberry Pi camera, a normal camera and one with the infrared filter removed. The latter is called the Pi NoIR and is meant for scientific research, education or infrared photographers. Images taken with PiNoIR are not meant for everyday use (don't take friends and family snapshots with them).

### Connecting the camera
  The camera is connected through a flex cable, see pictures below for more detail. 


### Enabling the camera
Before use, the Raspberry Pi must be setup to use the camera. From a command line terminal run: 

{% highlight bash %}
pi@raspberrypi: sudo raspi-config
{% endhighlight %}

Select option 5, "Enable Camera".
You will be asked if you want to enable or disable the camera, choose "Enable".
Select "Finish" at the bottom of the menu page.
You will need to reboot before the camera is available for use.


### Taking Pictures/Videos

The camera can be controlled either through a command line interface. There are three utilities available, `raspistill`, `raspivid`, and `raspiyuv`: <br/>
* `raspistill` - Basic camera application
* `raspivid` - Records live video
* `raspiyuv` - Camera application but outputs uncompressed (large) images. These are similar to RAW images if you are familiar with digital photography

While each of these commands have many options, we will only cover the most basic and useful options here. The full set of options can be listed by running the command with a `-h` argument.

### Picture Taking
The most basic use of the Raspberry Pi camera is with the command:<br/>
`raspistill -o output_filename.jpg`<br/>
This will save a full resolution image to output_filename.jpg. If you'd like to copy this file off your Raspberry Pi, the easiest way is to use `scp`. If your host computer is Windows, I recommend the WinSCP program. IF your host computer is OS X or Linux, run: <br/>
`scp pi@<raspberry_pi_IP>:~/output_filename.jpg ./`<br/>
You can also upload the image to an image viewing website like flickr or Facebook. 

A few of the more important options to `raspistill` are:<br/>
<table>
<tr><td>-w _width_</td><td>Set width of image, maximum of 2592</td></tr>
<tr><td>-h _height_</td><td>Set height of image, maximum of 1944</td></tr>
<tr><td>-vf</td><td>Flip image vertically</td></tr>
<tr><td>-hf</td><td>Flip image horizontally</td></tr>
<tr><td>-tl _ms_ </td><td>Timelapse Mode, time in milliseconds between pictures</td></tr>
</table>

There are also many other options as you would expect from a camera: exposure modes, metering modes, even Instagram-like filters. 

### Recording a video
To record a video, use `raspivid`. By default, the camera will record five seconds of HD video and then stop. To change the duration, use the -t _ms_ option. Setting a time of 0 milliseconds will record indefinitely (until disk space runs out). _ It is highly recommended to power your Raspberry Pi with a wall wart (not through a computer's USB port) when recording video!_

##### Basic Example
`raspivid -t 0 -o output_video.h264`
This will record until stopped (with Ctrl-C for example).<br/>
<br/>
`raspivid` also has some very useful triggers for creating multiple video files. The camera can stop recording and move to a new file based on time intervals or key presses. 
<table>
<tr><td>-w _width_</td><td>Width of video, maximum of 1920</td></tr>
<tr><td>-h _height_</td><td>Height of video, maximum of 1080</td></tr>
<tr><td>-b _bitrate_</td><td>Number of bits per second. Defaults to 17000000, set to 15000000 for HD</td></tr>
<tr><td>-f _fps_</td><td>Number of frames per second, typically 30</td></tr>
</table>

This can be played back using VLC or even streamed live to other computers. 

### Using camera in Python
  To use the camera directly from Python, use the `picamera` library. The library interface to use the camera is simple to use and offers many of the same controls as the command line utilities.

Before using the library, it must first be installed on your Raspberry Pi. pip (Pip Installs Python) is the application which manages Python packages, and it too must first be installed before use.

<% highlight bash %>
pi@raspberrypi:~$ sudo apt-get install python-pip
pi@raspberrypi:~$ sudo pip install picamera
<% end_highlight %>
_source code here_

Both flickr and Facebook offer Public APIs to upload your photos directly from python. Using web APIs will be discussed in more detail next week. 

### Example Source
* Photobooth
* Color picker
* Timelapse  

### FAQ
* Q: Why should I use timelapse mode instead of calling `raspistill` repeatedly?<br/>
A: Timelapse mode allows you to take more than 1 image per second and to more precisely time the interval between images. Also, the camera needs a small amount of time to warm-up, otherwise color shifts can occur. Using timelapse mode keeps the camera powered on throughout to avoid these color shifts. 
* Q: What is the power usage of the camera?<br/>
  A: The camera draws about 250mA additional to the Raspberry Pi
* Q: Why does my Raspberry Pi freeze when recording a video?
A: Power? Bitrate?

### References
* [raspistill, raspivid, raspiyuv Documentation](http://www.raspberrypi.org/wp-content/uploads/2013/07/RaspiCam-Documentation.pdf)
* [Raspberry Pi Camera Documentation](http://www.raspberrypi.org/documentation/usage/camera/README.md)
* [Motion](http://www.lavrsen.dk/foswiki/bin/view/Motion/WebHome) - Detects motion in a webcam
