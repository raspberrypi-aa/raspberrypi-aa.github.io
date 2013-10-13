---
layout: default
title: Audio Output
date: September 24, 2013
---

## Audio Output

Audio output on the Raspberry Pi is done through either the HDMI connector or the 1/8" blue headphone connector. Control of which connector the audio is present on is done through the amixer command. amixer is one of a suite of the ALSA control tools. ALSA is the Advanced Linux Sound Architecture and provides a set of utilities to configure and control sound devices on Linux computers. 
To install the ALSA tools you will need, run

{% highlight bash %}
sudo apt-get update
sudo apt-get install alsa-utils
sudo apt-get install mpg321 to play mp3s
     0 upgraded, 30 newly installed, 0 to remove and 301 not upgraded.
{% endhighlight %}

You can now explore the types of controls that are available to you. The "controls" command shows the different types of controls for the Raspberry Pi's built in sound device. "PCM Playback Route" determines whether the audio goes out the HDMI connection (value=2), the 1/8" headphone jack(value=1) or auto detect (value=0). The "auto" setting defaults to HDMI, but if not connected will use the 1/8" headphone jack.

{% highlight bash %}
pi@pi-friedrich ~ $ sudo amixer controls
  numid=3,iface=MIXER,name='PCM Playback Route'
  numid=2,iface=MIXER,name='PCM Playback Switch'
  numid=1,iface=MIXER,name='PCM Playback Volume'
{% endhighlight %}

The "PCM Playback Switch" control mutes and unmutes the Raspberry Pi. The "PCM Playback Volume" control sets the volume of the Raspberry Pi audio output. The current value of the control can be retrieved with the 'sudo amixer cget numid=<numid>' command, where <numid> is substituted with the number from the controls screen. For example, this command retrieves the current volume level. Here the volume is ... ...........


To change the volume, run 'sudo mixer cset numid=1 70%'. This will set the volume to 70%. You can also use a decibel value to set the volume. You can modify the numid= value above to change the Playback Route or Playback Switch controls. To play audio, you can use the 'aplay' utility to playback .wav files. For .mp3 files, you must use the mpg321 utility. The Occidentalis distribution comes prepackaged with some sound files in:

* /usr/share/scratch/Media/sounds
* /usr/share/sounds/alsa (This directory contains channel-specific sounds that let you play through specifically the front-left speaker, for example.)

Note that each of the commands run until playing the sound is complete. To return to the shell immediately, and allow the sound to continue playing in the background, add an ampersand at the end of the command.

To play audio from a python script, you can call the aplay or mpg321 utilities from the python script using the os.system() function. os.system() lets you execute a command from python, just as if you ran it at the command line. (Note: Python's subprocess module also allows execution of shell commands, but with greater control and more options).

### FAQs:

* Make sure no pulse audio related packages are present (dpkg --get-selections | grep pulse) if pulse audio or libpulse0 are present, purge them

* To fix: Error:ALSAlibpcm.c:2217:(snd_pcm_open_noupdate)UnknownPCMcards.pcm.front Change: /usr/share/alsa/alsa.conf betheline"pcm.front cards.pcm.front" to "pcm.front cards.pcm.default"

* If audio does not work over HDMI, edit /boot/config.txt and uncomment hdmi_drive=2