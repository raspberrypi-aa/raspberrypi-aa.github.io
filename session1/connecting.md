---
layout: default
title: Connecting to a Raspberry Pi
date: September 10, 2014
---

# Connecting to a Raspberry Pi

### Prerequisities
* Raspberry Pi
* SD Card with Raspberry Pi Image Installed (see below for instructions)
* USB To Serial Cable
* Computer (PC/Mac/Linux OK, driver installation required)

## Installing Raspberry Pi OS to SD card
If you do not already have an SD card with a Raspberry Pi image on it, you will need to follow these instructions. The SD cards that come with the class materials are preloaded, so you can skip this section.

1. Download an image, such as one from the Raspberry Pi [downloads](http://www.raspberrypi.org/downloads/) page.
2. Copy image to card. _Do NOT copy & paste, as it will not work!_ You must use a "helper" application such as Win32DiskImager, PiFiller, or dd.
    1. For Windows:
        1. Insert your SD Card into your computer.
        2. Download Win32DiskImager from [http://sourceforge.net/projects/win32diskimager/](http://sourceforge.net/projects/win32diskimager/)
        3. Once the download is complete, install Win32DiskImager using the downloaded file.
        4. Start Win32DiskImager from the start menu.
        5. Click the folder icon to select the Raspberry Pi image.
        6. Click the "Device" dropdown box to select the Raspberry Pi SD card. You can check "My Computer" to find the correct drive letter. 
        7. Click the "Write" button and wait for the process to complete
![Win32DiskImager](/session1/Win32DiskImager.png)

    2. For OS X:
        1. Download PiFiller from [http://ivanx.com/raspberrypi/](http://ivanx.com/raspberrypi/)
        2. Make sure your SD card is not inserted in your computer. If needed, eject and remove the SD card until instructed to insert it by Pi Filler. 
        3. Click "Continue"
	![Pi Filler Screen 1](/session1/PiFiller1.png)
        4. Select your downloaded Raspberry Pi Image
	![Pi Filler Screen 2](/session1/PiFiller2.png)
        5. Insert your SD card and click "Continue"
        ![Pi Filler Screen 3](/session1/PiFiller3.png)
        6. Wait for your SD card to be found and then click "OK" when prompted.
        7. Wait for the download to complete. This might take as long as 20-25 minutes to finish. 	
    3. For Linux
        1. See instructions from [eLinux.org](http://elinux.org/RPi_Easy_SD_Card_Setup#Using_the_Linux_command_line)


## Initial Setup
1. Install driver:
Please install the following driver (and software) as appropriate for your operating system:  

* Windows 8 : __NOT SUPPORTED__.
* Windows XP/Windows 7: 
    * [Prolific PL2303 Windows Driver](https://dl.dropboxusercontent.com/u/1733921/Raspberry%20Pi/PL2303_Prolific_DriverInstaller_v1_8_0.zip)
    * [PuTTY (SSH Client)](http://the.earth.li/~sgtatham/putty/latest/x86/putty.exe)
* OS X Lion (10.7+):
    * [Prolific PL2303 OS X Lion Driver](https://dl.dropboxusercontent.com/u/1733921/Raspberry%20Pi/PL2303_Serial-USB_on_OSX_Lion.pkg)
* OS X (10.6 and below):
    * [Prolific PL2303 OS X Driver](https://dl.dropboxusercontent.com/u/1733921/Raspberry%20Pi/osx-pl2303-0.3.1-10.4-universal.dmg)

2. Insert SD card into Raspberry Pi. The label of the card should be facing out, so you can read the words (away from the Raspberry Pi).
3. Connect the Ethernet cable from the Raspberry Pi to the network switch.	
4. Connect the USB Serial Cable to the Raspberry Pi Port1 as follows.
    1. P01-1 is the pin closest to the Micro-USB Power connector.
    2. P01-2 is the pin adjacent to the outside of the board.
        1. GPIO P01-2 -> Red (+5V)
        2. GPIO P01-6 -> Black (0V)
        3. GPIO P01-8 -> White (RX)
        4. GPIO P01-10 -> Green (TX)
![Fritzing Diagram](/session1/USBTTLConnection.png)
5. Plug USB connector into your computer's USB port
6. Connect to the Raspberry Pi over the serial port
    1. Linux: At the command line run `sudo screen /dev/ttyUSB0 115200`. Note: If /dev/ttyUSB0 does not work, run `dmesg` to check where the USB adapter was detected.
    2. OS X: As the command line, run: `sudo screen /dev/cu.PL2303-000012FD 115200`. The USB to Serial device will likely be detected at a different address, so try using tab completion (hit the <TAB> key)  after entering /dev/cu.PL2303
    3. Windows: First, determine the COM Port number. This may be displayed as a system notification when you connect the USB device. 
        1. Otherwise, you will need to check Windows Device Manager. To access Device Manager, click on the Start Orb and enter “Device Manager” in the Search box. Then scroll down to Ports and note the COM port associated with the USB to Serial Adapter.
        2. Open PuTTy and enter the settings as below, using the correct COM Port you determined before. 
        3. Click Open to Connect
![Putty Serial Connection](/session1/PuttySetup.png)
7. You should see a text scrolling by while the Rasperry Pi boots up. The login prompt may take several minutes to appear. If you miss the boot sequence, you may need to hit the Enter key several times for the login prompt to appear.
Login with:
<pre>
            Username: pi 
            Password: raspberry
</pre>
You may change the password now if you wish to do so, by running `passwd` 
8. Set the hostname to something unique (some variation on your name or email works well). 
    a) Run sudo nano /etc/hostname
    Change “raspberrypi” to whatever you want your hostname to be. 
    b) Run sudo nano /etc/hosts
        Change the line from:<pre>127.0.0.1 raspberrypi </pre>
            to:
	    <pre>127.0.0.1 whatever</pre>
9. Determine the IP Address of your Raspberry Pi. You’ll use it later to connect over ssh or web browser. 
Run `ip addr show eth0` and note the IP address shown to the right of  the word “inet”:
<pre>
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast 
    state UP qlen 1000
        link/ether 00:e0:4d:2f:91:d0 brd ff:ff:ff:ff:ff:ff
        inet 192.168.1.102/24 brd 192.168.1.255 scope global eth0
        inet6 fe80::2e0:4dff:fe2f:91d0/64 scope link
           valid_lft forever preferred_lft forever
</pre>
In this example the IP Address is 192.168.1.102


### Connecting via SSH
1) At an OS X or Linux command line, run:
ssh <ip address from step 9> -l pi

2) On Windows, start PuTTY and set the options as follows (replacing with your 
IP address):
![Putty SSH Connection](/session1/PuttySSHSetup.png)

3) Click Open to connect

### Connecting via VNC (optional)

1) Install VNC server by running:
`sudo apt-get install tightvncserver`

2) Start a VNC Server by running:
`vncserver :1 –geometry 1920x1080 –depth 24`

3) Use VNC Viewer, TightVNC, Chicken of the VNC to connect to your Raspberry 
Pi using the IP address above and port 5900


### Appendix A: Wi-Fi Setup 
Requires USB Wifi dongle
1. Open a text editor by running:
`sudo nano /etc/network/interfaces`
2. Make the contents of the file as follows:
3. Save and exit the file by hitting Control-X, then Yes
4. Run:
`sudo ifdown wlan0 && sudo ifup wlan0`
5. If all goes well, you can obtain the ip address by running:
`ip addr show wlan0`
![Wifi Setup](/session1/wifiSetup.png)
