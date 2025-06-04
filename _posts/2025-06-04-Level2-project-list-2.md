---
title: Level2 project list
updated: 2025-06-04 21:07:15Z
created: 2025-04-26 21:36:04Z
latitude: 49.56768070
longitude: 5.91254810
altitude: 0.0000
---

- [ ] A thermal printer with a button which prints you a random wikipedia site when you press it / prints an image of you
- [ ] A Matrix clock/Display  
    We have a few of those [Led matrix pannels](https://www.adafruit.com/product/1484) laying around and it would fit right in with our blinking wall :) . That would maily be used to show the time or some other funny visual or data from our Home Assistant installation, like for instance how many people there are or what the outside temperature and humidity are. Possibilities are endless, especially with an esp32/esp6288 with wifi. If there is no need for wifi we already have a few ready to go pcb's for Teensy's .  
    Example of what it could look like :  
    ![](https://www.electronics-lab.com/wp-content/uploads/2018/08/FH0LEIPJJ500268.ANIMATED.LARGE_.gif)
- [ ] Cube  
    If there are too many of those [Led matrix pannels](https://www.adafruit.com/product/1484) we could also build [this](https://learn.adafruit.com/rgb-led-matrix-cube-for-pi/overview) Project. I mean look at it. !!! Do I really have to explain myself ?  
    ![](https://cdn-learn.adafruit.com/assets/assets/000/111/308/original/led_matrices_sand-loop.gif?1651611930)
- [ ] [TranspOtter](https://github.com/lucysrausch/hoverboard-firmware-hack/wiki/Build-Instruction:-TranspOtter), Aka a transport robot using a frankenstein "Hoverboard"  
    The idea is to repurpose a "Hoverboard" I bough ages ago on a flea market for 5 € to help transport our Euroboxes to Event's. In summary the idea would be to use the preexisting stuff and just add on an esp32/8266 with bluetooth and connect a dual shock 4 controller to remote control it. Everything is already there, all that need to be done is to get that STUPID StmCube programmer to reflash the motherboard and do a few physical tweaks... In theory a simply project.   
    Image for illustrative purposes :  
    ![](https://raw.githubusercontent.com/Jan--Henrik/transpOtterNG/master/Image/docs/transpotter_top.jpg)  
    A more advanced step afterwards would be to add an Raspberry Pi with a lidar and have it autonomously navigate the space. You know like a roomba, just with a box filled with snacks ! :)
- [ ] Use one of the many old projectors we have to project Images/Videos on the blinds of Engineering  
    Obviosly only during the night and when the blinds are closed, I don't know the legality of that idea, especially considering this could impact driver safety on the pretty busy Route de thionville... The idea behind this was to make our space more "visible" to the public. Basically through having a bright ass projector beam our weblink / images or even polls we might get people to be interested in what we do :)  We had a working prototype of this using a Raspberry pi and some specific signage software but that never got much further than that. My main concern is with having the projector only on when the blinds are up and not too much light pollution.
- [ ] A midi clock

We have a bunch of radio controlled clocks, which belonged to a bank and were used to know the time in different time zones. They are locked down firmware wise so no reflashing of timezones is easily doable, but the electronics themselves seem very primitive, aka easy to manipulate. My idea was to use a few of these and try and create something similar to a [floppotron](https://www.youtube.com/watch?v=kCCXRerqaJI) , just with clocks and only one octave. IIrc that should be 7 notes ? I have not much of a clue in music but it would be a cool project especially in combination with our LaserHarph !

- [ ] A CRT wall

You know the art installation kind of crt wall with either white noise or weird videos playing.

![](https://static.wixstatic.com/media/c42c00_74815b2999a345f8b9ab9bb943ff56e3~mv2.gif)  
But instead of mirroring the images a all the screens we have independant video streams to stitch together an image of for example a Camera pointed at people. This is mainly for events to again maybe attract people with some interesting and weird looking tech ! :)  
The implementation could be somewhat hard though, either we need to basically setup our own cable network with each part of the image on another channel, which would imply we would need some rf modulator and a computer capable enough to encode how many different video streams simultaniously to different channels... or we would have a seperate pi with composite output to every single crt, and do the processing on an external pc and send over thte video over ethernet, this would arguably be the easiest setup wise, except for the Tv's without composit and only RF ... see many issues.

- [ ] Complete our SatNogs !

This is what a SatNogs looks like, it's basically an antenna rotator which allows you to track satellites with an antenna.

![](https://satnogs.org/wp-content/uploads/sites/2/2015/01/516791411972302453.jpg)  
The final goal of this project would be to successfully recieve data from a weather satellite and have "usable" images. For this we need obviosly a SatNogs, which to be fair we already have most of the parts, an antenna which is also a work in progress project , an rf reciever (an rtl-sdr dongle shoud be enough) and a pc to track/decode the rf signal. If you just want to try it out without the tracking you can use these websites : [n2yo](https://www.n2yo.com/) and [satdump](https://www.satdump.org/)

- [ ] Upgrade our CNC to have end stops  
    You read that right, our "CNC" doesn't have endstops so it WILL crash into the end of it's rails. The task is easy... open up the metal box and wire in 4 limit switches and maybe also one for Z homing, and update it's [GRBL](https://github.com/grbl/grbl) firmware. It would also be nice if it's specs could be written down in a more permanent manner than pencil hieroglyphs, so that it become a more usable tool. :) Limit switches are in the Buttons box and wire is readily available in the SCRAP box in Engineering.

![](https://cdn.thingiverse.com/renders/06/af/12/ea/4e/54a744b0c72af1fbcd018c1da5f28422_preview_featured.JPG)

- [ ] A Kiosk for controlling the multimedia stack

In the chill Area everything is automated, only issue being that in order to launch these automations you need to go push a button... on your laptop or the kiosk in the entrance. So for it to be actually useful we would need a small switch box in Chill which allows you to turn on the amp and beamer with one button press or change sources and very important Volume! I have some mechanical switches left over and there are plenty of encoders laying around which can be used as a turn style volume adjuster. If you want to be really fancy you can use a stepper motor in order to have a nicer feeling when rotating the knob. My idea was to use an Esp32 with EspHome and create simple entities in Home Assistant to just run these actions when pressed. Add a nice box into the mix and it could have the potential to look great and be functional. 

&nbsp;

That's it , for now. There are still MANY project ideas brewing but I think those are the most reasonable/doable. :)

If you want to do any of these please let me know and I can give you the necessary equipment/material and more ressources (websites and so on)

Thank you.