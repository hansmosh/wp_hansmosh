---
ID: 3
post_title: How I set up my Raspberry Pis
author: hansmosh
post_date: 2015-06-26 17:00:31
post_excerpt: ""
layout: post
permalink: http://hansmosh.com/?p=3
published: false
---
Using a Raspberry Pi is fun. You get a fully operational Linux machine with built in GPIO pins for a very low cost. They may be a little intimidating for first timers though so here is what I do when I'm setting up a new Raspberry Pi B+ from scratch. When we're done we'll have a Raspberry Pi that will connect to WiFi automatically on boot up, it will maintain that WiFi connection (which doesn't happen on its own) and you'l never need to plug a keyboard or monitor into it again. Note: I'm using a Mac and do not expect these steps to work flawlessly on other machines. 
### What you need?

*   Raspberry Pi B+
*   8 GB SD card
*   Power (USB cable and wall adaptor)
*   USB Keyboard
*   TV w/ HDMI cable
*   WiFi adaptor - Edimax

### 1\. Get that SD card ready First things first, download 

<a title="Raspbian" href="http://www.raspberrypi.org/downloads/" target="_blank">Raspbian</a>. Then you're going to need to write the image to the SD card. The <a href="http://www.raspberrypi.org/documentation/installation/installing-images/mac.md" target="_blank">docs on the official site are pretty good at explaining this</a> so follow those steps on your Mac and I'll move on. 
### 2\. Plug it in Go ahead and connect all your USB devices to the Raspberry Pi: Keyboard, WiFi adaptor and Bluetooth adaptor. Insert the SD card and plug in the power. You will be prompted to login. Username: pi. Password: raspberry. 

### 3\. Initial Raspbian setup Before bringing you to the command line, Raspbian will provide you with some menus. Feel free to poke around in here. I like to update two things: 

*   The keyboard needs to be a US Keyboard as opposed to a UK keyboard "vim /etc/default/keyboard"
*   Change the password for the user \`pi\`. "passwd" http://theurbanpenguin.com/wp/?p=2407

### 4\. Install Vim Lets update all the things: 

<pre>sudo apt-get update && sudo apt-get upgrade</pre> Vim is my text editor of choice. I'm sure I'll be writing up on it in the future. For now, we just need to install it: 

<pre>sudo apt-get install vim</pre> If you don't know how to use vim, for the rest of this tutorial replace all references to \`vim\` with \`nano\`. Nano is a text editor that comes with Raspbian. 

### 4\. WiFi setup I want my Raspberry Pi on WiFi. Honestly, Raspberry Pi HQ has it 

<a href="http://raspberrypihq.com/how-to-add-wifi-to-the-raspberry-pi/" target="_blank">totally covered</a> but I'm going to go over it here anyway. First, see that you're not on the network: 
<pre>ifconfig</pre> Make sure your WLAN adaptor was loaded by the OS: 

<pre>dmesg | more</pre> Enter you WiFi information in this file: 

<pre>sudo vim /etc/network/interfaces</pre> Make it looks something like: 

<pre>auto lo
iface lo inet loopback
iface eth0 inet dhcp

allow-hotplug wlan0
auto wlan0

iface wlan0 inet dhcp
   wpa-ssid "Your Network SSID"
   wpa-psk "Your Password"</pre> Now reload the network interface: 

<pre>sudo service networking reload</pre> And now when you run ifconfig you should see some network information: 

<pre>ifconfig</pre> Write down your \`inet addr\` so we can SSH in later. 

### 5\. WiFi reconnect script sadfdsaf 

### 6\. Bluetooth setup I write applications that make use of the 

<a href="http://www.ti.com/tool/cc2541dk-sensor" target="_blank">TI SensorTag </a>so it's important to me that I get   http://www.elinux.org/RPi_Bluetooth_LE     
### Generally useful command:

<pre>ifconfig
dmesg | more
route -n
sudo ifup --force <span class="pl-s1">wlan0    --- When you're connected to the local network but can't get online</span></pre>