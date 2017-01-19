---
ID: 19
post_title: XBee is easy!
author: hansmosh
post_date: 2015-04-27 19:23:23
post_excerpt: ""
layout: post
permalink: http://hansmosh.com/?p=19
published: true
---
Okay, so you've heard of XBee and how you can use it to bring wireless communication to your project but that sounds hard, right? Well, it's not. Like anything tech, there is some barrier of entry and dozens of places you can go wrong, but here is how I get XBee up and running as simply as I know how. We will have one XBee sending analog sensor data to another XBee that is hooked up to my laptop and we will print out the values every second with a simple Python script. 
### What do you need?

*   2 XBee Series 1
*   1 XBee Explorer USB Board (and USB cable-- make sure the cable is wired up for data in addition to power!)
*   1 XBee Breakout Board
*   Power supply, breadboard, sensor, etc... Basically, whatever you want to do to provide 3.3 Volts of power to the XBee Breakout Board and whatever analog sensor you've got.

### Get things installed As is often the case, getting everything installed is often the trickiest part. We'll be reading from the XBee with Python so: 

<pre>pip3 install xbee</pre> You also need to install the FTDI driver for the XBee Explorer USB Board. It's not as simple as installing the Python module but just follow the official docs and you'll be good: 

<pre>http://www.ftdichip.com/Support/Documents/AppNotes/AN_134_FTDI_Drivers_Installation_Guide_for_MAC_OSX.pdf</pre>

### Configure the XBees I'll break down some XBee configuration basics: 

*   PAN ID: All your XBees need to have the same Personal Area Network ID in order for them to talk to each other.
*   MY: Each XBee should have "my address" set to a unique value within your PAN ID.
*   Destination Address: This is the address of the XBee to send data to. In this example, we'll have XBee with address 2 send data to XBee with address 1. As others have done in similar tutorials, I'm simply going to reference you to an already great guide to configuring your XBees. Part Two will get your XBees all setup and good to go: 

<pre>http://www.brettdangerfield.com/post/raspberrypi_tempature_monitor_project/</pre>

### Wiring up the sensor Plug XBee 2 into the Breakout Board. When looking at the XBee from above, the top left pin is number 1 and they count up in a counter-clockwise circle with pin number 20 being the top right. Send 3.3 volts to pins 2 and 14 and ground pin 1. Your sensor goes to pin 20. I set mine up with a potentiometer such that I can control the voltage to pin 20 with a value between 0 and 3.3 volts. At this point your XBee is already sending data! 

### Read the sensor Plug the receiving XBee into the Explorer USB Board, connect it to your computer and read some serial data! Save the following to xbee_test.py: 

<pre>import serial
from xbee import XBee

SERIALPORT = "/dev/tty.usbserial-DA013Y6Q" # Check yours with `ls /dev/tty.*`
BAUDRATE = 9600 # the baud rate we talk to the xbee

ser = serial.Serial(SERIALPORT, BAUDRATE)

xbee = XBee(ser)

while True:
    try:
        response = xbee.wait_read_frame()
        print(response)
    except KeyboardInterrupt:
        break

ser.close()</pre> and run it: 

<pre>python3 xbee_test.py</pre> Turn the pot and see adc-0 change. That's it! Ctlr-c once you get bored. 

### Conclusion I've been intimidated by these little guys for 7 years and now that I've finally got them up and running I can't believe I waited so long. The wireless is amazing but I'm finding myself just as excited to have some analog pins on my MacBook Pro.
