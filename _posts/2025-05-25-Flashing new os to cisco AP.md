---
title: Flashing new os to cisco AP
updated: 2025-05-26 19:52:50Z
created: 2025-05-26 08:58:34Z
---

# What you need 

Hello this should guide you through flashing your Cisco AP, in my case I got a few Cisco AIR-CAP1702I-E-K9, which were used in a big commercial deployment and EOL. 

In order to use the AP's in your personal network, you need:

- Cisco AP
- Cisco Serial Cable (rj45 to usb)
- A computer with ethernet, I will use a mac, for windows/Linux this should be fairly similar
- A **POE** injector
- A compatible **Autonomous** firmware for your AP (that's arguably the hardest part)

&nbsp;

# Preparing everything

## Finding the OS

This is pretty hard if I am honest, there are multiple websites which explain how you can find it and where to download it.

You are looking for a firmware with the **k9w7** ending, this is the autonomous firmware. My firmware is the `ap3g2-k9w7-tar.153-3.JH.tar`

Put that into google and you should find a few places to download it. 

&nbsp;

## Install drivers for your cheap little usb adapter

I borrowed a friends rj45 to usb serial adapter, depending on your OS and adapter this should be fairly straight forward. 

## Give it a power source

**But** don't start it yet, plug your rj45 serial cable into the console port and an ethernet cable into an poe injector/switch which is turned off

&nbsp;

## Finding your serial interface

in my case it's : /dev/tty.Plser

Use this command to find your's

`ls /dev/tty.*`

This should give you a list, run this command twice, once with your adapter connected and once without. Now just compare results. 

&nbsp;

# Connect to your AP 

With the previous command, you should now have found a port, use this command to natively connect to the AP:

`screen /dev/tty.Plser 9600`

It should show you a blank screen, that's normal.

&nbsp;

## Power

Power on the POE power source and start pressing the `esc` key multiple times until it tells you it interrupted booting.

You should see terminal lines fly by  at the beginning until it ask's you this :

```
Technical Support: http://www.cisco.com/techsupport
Compiled Wed 14-Oct-09 18:59 by prod_rel_team

ap:
```

Congrats you are now in the console of your AP !

&nbsp;

In order to start from fresh, we need to clear the previous config. 

Type this to clear the flash :

`format flash:`

This **will** reset your AP's config. 

&nbsp;

&nbsp;

# Transferring the OS

Now set up the tftp server. On a mac it's easy , copy the .tar file into `` `/private/tftpboot` `` by going to the finder and pressing `command + G + Shift` and run this command afterwards :

`sudo chmod -R 777 /private/tftpboot`

Finally run this Command to start the daemon :

`sudo launch``ctl load -F /System/Library/LaunchDaemons/tftp.plist`

and kill it with after you are done.

`sudo launchctl unload /System/Library/LaunchDaemons/tftp.plist`

## Setting the networking parameters

Go back to your terminal window of your AP and paste each line separately with pushing enter after each paste:

`set IP_ADDR 10.0.20.67`  
`set NETMASK 255.255.255.0`  
`set DEFAULT_ROUTER 10.0.20.1`  
`ether_init`  
`tftp_init`

These lines set a static ip and networking for your AP, be sure that the POE switch you are using is not connected to anything else, or you'll run into issues...

## On your computer

Go to your networking settings and configure your IPV4 address manually.

Put in the same values as above for your ethernet connection but modify the Ip address to a different number;

for example `10.0.20.66` and **APPLY** the settings

## On the AP

run this :

`tar -xtract tftp://10.0.20.66/ap3g2-k9w7-tar.153-3.JH.tar flash:`

This will transfer your file over tftp to the AP and write it to the previously erased flash.

Go grab a coffee or do something else, this step will take a couple minutes. 

&nbsp;

After some time it should be done and give you a

`ap:`

This means the transfer was successful

Type this and accept:

`reset`

Now you can let it do it's thing and after you see that it booted up correctly you can plug it into a DHCP network and it will get an IP.

Find the IP by doing a network scan or connecting to your router to find the newly added device.

# Setting up the AP / wireless networks

Open a browser and go to your AP's IP, you should be greeted by a login page. 

The login is :

username : `cisco`

password : `Cisco`

You should now be in the AP's config page

Congrats !

You can now play around BUT you will find that no matter how many times you enable your radios, they will not come online :P

&nbsp;

### **We need to setup our WLan over the serial connection.** 

Still in your terminal connected to your AP type 

`enable`

and the password is `Cisco`

This will get you into the `ap#` directory

type `configure terminal`

then `interface bvl1` if you get the error that bvl1 doesn't exist just type interface and then press `tab` to see the possible options. Choose accordingly. This interface should be the ethernet connection of your AP, there might be multiple if you are using another model. 

### Giving your AP a static IP

We don't want our AP to change IP's when rebooting, so we will give it a static IP in the network it will live in. Modify the values accordingly, in my case my network is a 192.168.1.1/24 :

paste : `ip address  192.168.178.*** 255.255.255.0` , \*\*\* can be any number between 2-255, depending on if you want some resemblance of structure in your network set it to the same range your other AP are in. 

type : `exit` to exit the interface config directory then `ip default-gateway 192.168.178.1` , 192.168.178.1 should be the address of your router. 

### Creating an SSID

Still in the `ap(config)#` directory type

`dot11 ssid your_SSID` , your_SSID will be the name you want your Wifi to be, **WRITE DOWN THIS NAME YOU WILL NEED IT LATER.**  Type `guest-mode`, this will make the network discoverable to other people, else it will be hidden.

Now type `authentication key-management wpa version 2` in order for your network to use a WPA2 for authentication and `wpa-psk ascii YOUR_SUPER_SECURE_PASSWORD` with YOUR_SUPER_SECURE_PASSWORD being your password, in clear text, duh. 

Now `exit` , congrats we should now have a public SSID with a password. 

## Powering up the radios

### 2.4 GHz

That's right, finally we get to the radios, we will start with the 2.4 GHz radio, type `interface dot11Radio 0`, 0 should be your 2.4 GHz radio. Enter  `encrytion mode ciphers aes-ccm`, this will enable aes encryption for added security. `ssid your_SSID` , see I told you you needed it for afterwards, change this to the previously defined SSID. Then `channel least-congested`, this should do the channel hopping automatically, no real need to change anything here. 

Cisco is weird, so they enable devices by telling them to not shutdown ... type `no shutdown` to enable the 2.4GHz radio. You will now be flooded with messages about the radio, that's a good sign. Type `exit` to come back to the `ap(config)` directory. 

### 5 GHz

Just about the same procedure, type `interface dot11Radio 1` ,1 should be your 5GHz radio. Enter  `encrytion mode ciphers aes-ccm`, this will enable aes encryption for added security. Then `ssid your_SSID`, again change to yours, after that `channel least-congested` , `no shutdown` and finally **2 x** `exit` to get back to the ap> directory. 

&nbsp;

Your SSID should now be visible and you should be able to connect to it. Happy Hacking and don't forget to reset your ethernet setting on your computer :) 

I hope this helped, maybe pictures will follow, I will more likely forget it though. 

&nbsp;
