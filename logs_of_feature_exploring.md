# logs of feature exploring

when i am learning new things it sometimes helps me to write up my research and findings. if you are interested in the process of developing this project you can read some of the notes i made along the way here. some of what follows is out of date or not relavent fto the current state of the project but maybe it is interesting or useful for some one to look into ?



<details>
<summary>2018-05 hdmi drop out bug</summary> 

# hdmi drop out bug

some time between when i did compatibility testing , and now - high res videos now often (and consistently cause hdmi drop outs. all 1080 do , and some 720 also) - on my hdmi-to-vga computer display. 

also (possibly related) when trying it on full hdmi projector, it drops out playing all video no matter the resolution ! whats going on ?

some research suggests that not enough power can cause this problem.
 possible fixes / things to rule out include : 
 
 - trying signal-boost in config
 - try overclocking the pi
 - try an old version of the code
 - try an old version of firmware
 - try changing some other config settings (camera mode etc)
 - try creating a new img from old img (with bare minimum) and update piece by piece until it doesnt work.
 
## results:

### vga-hdmi display

- signal boosting didnt help ,
- old version of code didnt help
- old firmware improved a little bit but didnt fix
- changing config etc didnt help
- turning off raspi2fb didnt help

### drastic measures ! :

- flashed a new sd with old raspbian from last year. only installing the minimal to play video.

- while running old code it plays better but not perfect - still drops from time to time but mostly working.
- tried installing some newer dependencies , still running old code :

didnt investigate this route any further

### solved ?

by chance i tried switching the video output to composite and back - ~~somehow after this the hdmi plays 1080 videos just fine. no idea why or when it was introduced , but i created an action to run this on startup and seems fine now. phew.~~ this didnt seems to fix it after all.

i tried setting the hdmi output to 720 instead of 1080 and from here all the videos including 1080 load and play fine. still some dropouts for 1080 video on my vga converter though...

</details>

<details>
<summary>2018-05 compatibility testing</summary> 

# some of what follows is out of date since i have improved a number of problems since i did this. i will have to revisit this page with updates soon !

# compatibility testing

so far i have only been using a small selection of video files while running **r_e_c_u_r** .

i want to try it with a number of different video containers , codecs , resolutions , lengths etc on
both hdmi and composite displays. by compiling and downloading a number of test clips from various places online ,
i now have a folder of videos named in this format : 'width-container-codec'

clip | expectation | results
--- | --- | ---
120-mov-svq1.mov | i wouldnt expect/need this to play with apples custom codec | omx cant reconise codec
240-webm-vp8.webm | i believe vp8 codec is supported on omx but not sure if webm container is | omx cant play webm
360-webm-vp8.webm | same as above | ""
480-avi-mjpeg.avi | this should play fine | ~~wont map `dbus exception no reply`~~ on second try it does map but doesnt play - normal omx player opens but cant play it either
480-flv-vp6.flv | i think this should work although not too worried about supporting flv ! | omx cant play this ?
576-mov-mjpeg.mov | this should work | this works surprisingly well - never lags on load
576-mp4-h264.mp4 | this should work fine | plays good, sometimes lags on custom start
720-mkv-h264.mkv | this should play fine - interested if seeking / sublooping looks ok | plays ok , customstart seems to weirdly jump to middle ...
720-mp4-h264.mp4 | this should play fine | plays good, sometimes lags on load
720-mp4-h264-60fps | ~~my computer struggles to play this~~ think just a laggy video | plays suprisingly well - never lags on custom start
1080-avi-mpeg4.avi | i think this should play ok | does play - sometimes struggles to load the next loop though - similar to the other 1080 one but a bit better (seems to load if current video is paused)
1080-flv-flv1.flv | wouldnt expect/need flv with custom codec | omx doesnt recognize codec
1080-mkv-h264-60mbps.mkv | dont expect this to work | surprisingly this video played though on pi (didnt even open on my computer.) however is showing a weird new bug where when the video finished , the next one wont load but also wont error... _UPDATE: works now gpu has more memory_
1080-mp4-h264.mp4 | hopefully this plays okay ?? | this wouldnt load , error getting video length : `dbus exception no reply` (although it played ok in normal omxplayer) _UPDATE: works now gpu has more memory_
error-mkv-mpeg4.mkv | should fail but not sure how.. | just wont load

an interesting note : ~~all the videos above behaved the same on hdmi and composite out except for 1080-mp4-h264 which didnt have the failing to load next problem , instead , would flash green for a bit at the start of each clip even on custom starts~~ this also started happening on hdmi out - seems to be unpredictable how it handles 1080p files

## the same mp4 video at different resolutions

i use adobe premiere to edit videos. i imported a raw 10s mts file from a digital camera and exported 3 times : 480 , 720 and 1080. these three were loaded and tested on recur :

- 480 played fine - no lags when custom starting (both on hdmi and composite)
- 720 played ok - video played through and loaded all good. sometimes would lag on custom start point , seemed to be better / not do this when composite out mode
- ~~1080 doesnt work - the video can play through once alright , but it seems like the omxplayer/dbus connection cant load another 1080 file while an existing 1080 file is playing~~

UPDATE: turns out the 1080 files couldnt load because the pi hadnt been assigned enough memory to its gpu . i added `gpu_mem=448` to the config.txt and now 1080 videos seem to load and loop just fine. (still sometimes lags when change is triggered/ custom start)

# summary of findings :

- .mp4 containers with h.264 codec seems to be the best format - long videos play fine (tested up to 3 hours) besides some display confusion for durations over 99 minutes as expected. can play files up to 1080 resolution fine. the chances of micro lags on changeover/custom starts seems to increase with higher resolution (no problems with 480 now) but can still be avoided by setting another custom start just after the position that is lagging.
- .avi , .mov and .mkv containers also work. h.264 is best , mjpeg worked in a .mov container but not in .avi ... there was some issues with setting custom start in one of the .mkv files i tried - this might need some more investigation...


</details>


</details>

<details>
<summary>2018-05 firmware bug</summary> 

## firmware bug

at some point i must have updated the raspi firmware. this caused the recur application to present a bug that makes it basically unusable:

### the bug

the video freezes / lags often , usually at the same spot : 10s in, 2minutes in etc. when a video is reload (ie another video player created underneath current one) the current video usually freezes.

### investigation

- i tried checking out an old version of recur code to rule out a change on my code causing it. this still behaves the same way

- i created a new version of recur from the newest (april) version of stretch. this also displays the bug.

- finally i created a new version of recur from the older version (november) and made sure to not install any of the new packages (midi / capture). this worked without the bug on an old version of the code ! wahoo. i now tried installing those new packages and running the update code. still working !

- next i created an image of this working recur. (known issues with the image so far : screen saver is on, our home wifi is included)

- the firmware that is working is `uname -a : Linux raspberrypi 4.9 59-v7+ #1047 SMP Sun Oct 29 ...`

- next i will try a `sudo apt update` and `sudo apt upgrade` to rule out a package update causing the bug - can confirm its still working after apt upgrades ! must be the firmware :

- the gpu firmware (not sure the exact dif..) is obtained from `sudo /opt/vc/bin/vcgencmd version` and is `Oct 24 2017 .. a3d7660e6749e75e2c4ce4d377846abd3b3be283 (clean) (release) `

the new firmware says its : `754029b1cb414a17dbd786ba5bee4fc936332255` which is what i started typing into `sudo rpi-update 754029b`

now `uname -a` reads `.. 4.9.60-v7+ #1048 .. Fri Nov 3` and the player still works. `sudo /opt/vc/bin/vcgencmd version` says `Nov 3 .. 1bcf9152... (clean) (release)`

pushed forward to v 4.14.20 which failed. pushing back: did this a few times. got it working on 4.9.78 but failing on 4.9.80. looks like it might be this issue : https://www.raspberrypi.org/forums/viewtopic.php?t=195178 - just need to figure out how to turn it off for testing

found it ! i have the latest firmware version and by adding `audio_pwm_mode=0` to the config it now plays as before . phew what a relief ! 

gonna create new version of the image with this fixed , the wifi removed and the screensaver disabled.

</details>

<details>
<summary>2018-05 video input</summary> 

# video input

### background

a common feature of audio samplers is the ability to 'live sample' ie have a line in to the device and use this to record a sample directly onto it - usually to be played back again immediately  as part of a performance.

from what i can tell some hardware video samplers also offer this feature. it would be interesting to explore the possibility of this with recur. some ways to record video into recur include:

- a usb dongle / external capture card
- using a pi camera for live video or rescanning off a screen
- piCapture : a custom video card built for pi to use the pi cam protocol

the first option seems fiddly and difficult / a compromise. i might be interested in trying to capture with a Blackmagic Intensity Shuttle if i get one some day...

options 2 and 3 both seem plausible, and hopefully will be interchangeable once things are working (piCapture claims to act exactly like a piCam so should painless). also this allows both a cheap/hacky solution (rescanning through $2 camera) or a more professional option ($130 addition) and i can start experimenting now with a cheep cam before investing in the expensive piCapture.

i know piCam and piCapture recommend using python and there is libraries for this

### things to find out

i want to know how plausible it would be to add live sampling to my current recur stack (gpio lcd screen, omxplayer video backend).

some things i want know are:

- how to set up the piCam on raspberry pi
- how to record the input from the piCam onto the pi
- will these files play back in omx/recur
- can you preview the stream on the hdmi/composite output ?
- can you preview the stream on the lcd display ? 
- can this preview be adjusted / resized etc ?
- can you adjust params of camera ? in real time ? framerate , brightness , contrast , zoom etc ? 
- how does rescanning look ?
- how could i control piCaptures additional params ?
- is there any reason to think piCapture wouldnt be interchangable with a working piCam feature ?

### research

[cheep cameras] for raspi can be bought from china with 5 mega pixels / recording 1080p video for around $5usd. i have borrowed a camera ~~which i think is a night-vision version~~ to experiment with , although should order one of these myself.

i started by reading the [picamera] python package docs. this seemed to have lots of options regarding recording video , including starting and stopping both previews and video recordings , the ability to set the resolution , framerate , shutter speed of the camera , switching preview to full screen or setting the size and position of it. 

also a bunch of parameters that might be of interest including brightness , colour effect , contrast , flips and rotations , preview alpha , saturation , sharpness. these apear to be controllable in real time while previewing or recording.

it seems like the output from the camera is raw (without frame per second meta data) h264 and to play the video back at correct speed will need a program to wrap it in an mp4 container :
```
$ sudo apt-get install gpac
...
$ MP4Box -add input.h264 output.mp4
```

the [faq] also addresses the 'can i preview to the lcd screen' question : looks like no - at least not without copying the exact framebuffer , similar to my experiments displaying omx on the lcd screen. (this still might be possible in the world of openCv but off the table for now! - or maybe not even then - this [adafruit] tutorial talks about the limitations of displaying on a tft screen - "accelerated software will never appear on the PiTFT (it is unaccelerated framebuffer only)" ) 



### research continued : piCapture

[picapture] is a video capture card designed for the raspberry pi to emulate the piCamera and take advantage of the pi's hardware acceleration. it comes as a 'hat' that also uses ic2 (or serial)
to communicate. there is a python package to access these additional options. 

these come in two types :

- standard def - composite and s-video in , $139usd
- hi def - hdmi and component in $159usd

im not sure which one i would like to try, but they sound cool ! would need to check the pins dont clash with the display but i think these should work together nicely !

### getting started with piCamera

following the picamera docs , i will/have :

- plugged in the camera
- turned on camera in the config
- tried take an image
- installed package with `sudo apt-get install python3-picamera`
- run `sudo apt-get update` and  `sudo apt-get upgrade` (for firmware)
- trying some simple python commands with camera
- write some experimental recur code

first hitch : i enabled the camera in the raspi-config , but it seems like the switching screens driver overrides this , ~~so will have to update this too !~~ fixed this by adding a line to the config.txt

besides that the preview / different parameters and effects work as expected. next step is to try recording something , converting it to mp4 and playing back on omxplayer.

i have installed `sudo apt-get install gpac` and am using `subprocess.Popen` to run the `MP4Box` command from inside python. this way i can poll back into it and map the video only when its finished converting to stop blocking in the meantime. i also updated the display to show when the camera is previewing and recording. this all worked smoother than i expected.

i also made a (surprisingly small) change to the browser to show the pi's videos folder next to the external devices. this will be useful for using the recordings saved and for copying files onto recurs disk. (the copying feature has been de-prioritized since it can be done manually with mouse/keyboard and could be risky / might want a confirmation window ...)

another thing still to think about is how to protect from overfilling the sd card / external storage. 
- i have done this by checking before starting to record and every 10 seconds during recording if the disk space is under 10mb in which case it warns and stops the recording.

also displaying info when camera is not attached and catching other types of errors... 
- this will be handled by  bool enabling the capture. if it can not detect the camera it will not allow this to be enabled.

[picamera]: http://picamera.readthedocs.io/en/release-1.0/api.html
[faq]: https://picamera.readthedocs.io/en/release-1.13/faq.html
[adafruit]: https://learn.adafruit.com/adafruit-pitft-3-dot-5-touch-screen-for-raspberry-pi/easy-install-2

[cheep cameras]: https://www.aliexpress.com/item/5MP-Camera-Module-Flex-Cable-Webcam-Video-1080-720p-For-Raspberry-Pi-2-3-Model-B/32860830711.html
[picapture]: https://lintestsystems.com/products/picapture-sd1


</details>

<details>
<summary>2018-05 midi support</summary> 

# midi support

my investigation into controlling recur with midi will be documented here.

### aim

i want to be able to read midi messages to control recur - the simplest intergration would be loading and triggering clips from midi. other paramters controled over cc or otherwise should be easy to add when required. in a similar way to the keypad controls, i would like the mapping to be read from a json file and use this to run actions.

### cheep midi9to-usb dongle

i have decided to start by trying a cheep [usb-to-midi cable] i got for just over $3.

plugging the midi dongle into the pi , i can see it in `client 20` by calling `aconnect -i` , however when connected to a midi controller (deluge) it is not receiving messages when `aseqdump -p 20` is running (besides a few ghost note off events)

running midiox on my windows machine to test the cable also didnt help, it wouldnt receive any messages and complained about not having enough memory...

something seems wrong with the dongle. it also seems like these cheep dongles are not very reliable or useful anyway according to [sy35er] on a yamahamusic forum

update : by using ableton on my windows i could successfully send midi out off usb and receive it on deluge. still no luck reading midi to usb though. 


### usb midi

next i tried using the deluge to output midi over usb. midiox on windows still wouldnt work , but when i tried `aseqdump -p 20` on the pi it recorded the midi perfectly. i will use usb midi to test this feature on recur. (better adapters exist that convert serial midi to usb midi /could make one with ardunio or teensy!), so that will be a good start

i have decided to try using the [mido] python package for responding to midi input.

### midi clock

besides the obvious triggering of clips and controlling parameters, it has also been suggested that the recur could 'sync' to incoming [midi-clock] messages. these communicate a master tempo by sending 24 pulses (clock messages) every quarter note. this could be useful in play modes where the video changes every bar of music (counting pulses from a start message) or even to approximate slowing/speeding of a clip if the speed control of omx would handle it 

### getting started with mido

i install mido and rtmidi for backend : `pip3 install mido python-rtmidi`,
i called `mido.get_input_names()` and searched the results for the first containing the substring `20:0` (this is the port my devices came up as - will need to investigate further why this is and if it will be the case for all external usb midi devices)

from here i called a polling method that reads the next message coming from that port and if there is one will call into the mapping function (from json file)

i created a midi_action.json mapping in the same format as the key press but with `type (value)` as the keys. eg `note_on 70` is an example, `clock` is another. for now i have mapped exactly the same as with the key presses

this works perfectly for pressing keys on the deluge and seeing the corresponding action on recur. even when pressing keys with the sequencer running (lots of clock messages being sent) it still responds quickly and consistently. however when triggering the same notes from the sequencer, it seems to drop lots on the notes : usually only picking up either note_on or note_off , sometimes both and sometimes neither.

I tried changing the way recur handles the incoming messages (working through a list of unprocessed messages rather than one at a time) , but this didnt help - its not a problem of response time as can be shown by pressing keys with sequencer on. even looking at the raw input to the port on `aseqdump -p 20` i can see some messages missing. i updated the deluge firmware to a stable release but no changes.

however when i tried turning off the clock messages from the deluge , the sequencer messages started coming through clearly ! and it syncs up nicely... how strange.

### further clock debugging

other things to try is learning how to use abelton as a midi controller and see whether it works there with/without clock messages. could also try other gear that can output midi / controllers with recur and try deluge with other computer / gear 

### experimenting with cc

besides figuring out whats up with the incoming clock signal , another thing to experiment with is using a cc knob to control parameters in recur. there is nothing pressing i want for this but could try it with : fades , speed? , length of seeks , seeking? 

i ended up trying cc with some camera parameters. there are plenty more interesting examples of continuous control there. it works well , but it seems like running the methods on every change when they are sequenced / coming in consistently is a bit much for recur to respond in real time.

one idea to reduce this is to only process a incoming cc message if it is outside a range for that channel - ie only action on every 3rd cc change for each control for example. - this works well. updating every 5 cc values had no lag under heavy use but looks quite jumpy. step size of 2 looks smooth but can lag quite a lot. i have it set to 4 atm but can revisit when other features are under cv control etc. 




[usb-to-midi cable]: https://www.aliexpress.com/item/Hot-Selling-1pcs-Keyboard-to-PC-USB-MIDI-Cable-Converter-PC-to-Music-Keyboard-Cord-USB/32813475019.html
[instructable]: http://www.instructables.com/id/PiMiDi-A-Raspberry-Pi-Midi-Box-or-How-I-Learned-to/
[mido]: https://mido.readthedocs.io/en/latest/
[midi-clock]: https://en.wikipedia.org/wiki/MIDI_beat_clock
[sy35er]: https://yamahamusicians.com/forum/viewtopic.php?t=8218


</details>


</details>

<details>
<summary>2018-05 porting to other sbc</summary> 

# porting to other sbc

a collection of thoughts / research / attempts at porting r_e_c_u_r

## attempting to port r_e_c_u_r to an orange pi plus

i bought an [orange pi plus] at the same time as my raspberry pi 3 , thinking that since they are 
similarly spec-ed (orange pi is a little weaker but also much cheaper ~18US vs ~38USD i payed for rpi3) this might be an interesting alternative to offer.

i (naively) figured since opi claim to support raspbian as an os, that this might be as simple as installing the same dependencies outlined in the [preparing image] notes and maybe some fiddling with the lcd-screen drivers...

it seems like the only raspbian image for orange pi plus was a fork of wheezy distro from 2015 with pre-installed desktop. this is not supported or updated by orange pi and seems to be a token gesture to compete on paper with raspberry pi. with this image i managed to install some dependencies, but the dbus packages needed for the omx wrapper couldnt be installed (i think the os was too old). also their is no config.txt on orangepi so settings like composite video out and different hdmi modes was going to be more difficult (not to mention the lcd screen driver)

## porting to armbian

however what is supported and updated is an os called armbian , which (similar to raspbian)
is a version of debian (or linux) made for ARM dev boards. if i can get r_e_c_u_r working on opi running latest armbian , it might also work on a bunch of other sbc including other orange pis, ondroids , banana pi etc (beaglebone's run straight debian so perhaps i should see if it works there too)

given that the software dependencies are available in these alternative os (im not sure if they are yet but will just have to try it) some other problems to solve when trying to port r_e_c_u_r to other sbcs are:

- connecting a lcd screen / playing video to one framebuffer while running python on another. 
[Kaspars Dambis' blog] describes how to configure a similar lcd display to mine on an orange pi zero running armbian so hopefully this could help porting it to orange pi pc
- playing the video through one framebuffer (hdmi or composite) while the python code displays on another (lcd screen)
this kindof 'just worked' for me on raspberry pi running rasbian but i dont know how it will translate...
- video playback might be weaker depending on the gpu acceleration of these alternative boards 
(omxplayer is accelerated for rpi but probably not for these others). also things like h.2 video codex
licencing things etc might come into it
- configuring different hdmi and composite video settings. (pi seems to do this particularly well)

## conclusion for now

some more research into this is required , but at this point it seems like the extra effort to get recur running on other smc's might not be worth the savings in cost or flexibility.

r_e_c_u_r is an embedded solution and the choice of hardware (raspi3) is tied to the application : 

- lcd screen drivers
- omxplayer w acceleration
- (future features using pi camera / capture devices)

right now rpi3 still seems like the best tool for the job and the benefits of running cross-boards are
not enough to distract from improving this implementation. (perhaps a future recur independent of omxplayer might benefit more from running on main debian / armian)

## r_e_c_u_r on other raspberry pi boards

as an aside, i am still hopeful that r_e_c_u_r will run on rpi2 and/or zero with little to no changes required. this might be a more useful and achievable port to focus on for now.




[orange pi plus]: https://www.aliexpress.com/item/Orange-Pi-PC-linux-and-android-mini-PC-Beyond-Raspberry-Pi-2/32448079125.html?spm=a2g0s.9042311.0.0.kWJI0G
[preparing image]: ./preparing_image.md
[Kaspars Dambis' blog]: https://kaspars.net/blog/linux/spi-display-orange-pi-zero


</details>


<details>
<summary>2018-07 cv and serial midi</summary> 

### serial midi from rpi gpio pin

i believe it is possible to read midi in from an i/o pin (that can read serial) which might be desirable in some cases but this is a good start for now. this [instructable] explains how to input/output midi with the gpios, it looks like the tx/rx (serial?) pins on the pi3 are currently used/covered by the gpio screen that i am using. if i was serious about external circuits interfacing with pi/recur, i might look into using an lcd screen that doesnt use up the gpios. (would also be worth checking if piCapture would work with the gpio screen i have...)

### gpio pins : 

the [adafruit tft display] mentioned above also uses the gpios to connect to the pi - in particular it uses 5 spi pins and two standard pin outs. by cross referencing the [raspi gpio] docs it does not use either of the rx serial pin , which would be needed if i were to receive midi directly (rather than through usb), it also leaves plenty of pins for receiving cv from a [mcp3008] through software spi for example. it is likely that my gpio lcd screen communicates with the pi in a similar way and that i could figure out a way to connect these extensions if desired.

what follows is the interface of cheep lcd from the shop page :

PIN NO.| SYMBOL | DESCRIPTION
--- | --- | ---
1, 17| 3.3V | Power positive (3.3V power input)
2, 4 | 5V | Power positive (5V power input)
3, 5, 7, 8, 10, 12, 13, 15, 16 | NC | NC
6, 9, 14, 20, 25 | GND | Ground
11 | TP_IRQ | Touch Panel interrupt, low level while the Touch Panel detects touching
18 | LCD_RS | Instruction/Data Register selection
19 | LCD_SI / TP_SI | SPI data input of LCD/Touch Panel
21 | TP_SO | SPI data output of Touch Panel
22 | RST | Reset
23 | LCD_SCK / TP_SCK | SPI clock of LCD/Touch Panel
24 | LCD_CS | LCD chip selection, low active
26 | TP_CS | Touch Panel chip selection, low active

from this i should b able to work out which pins i can use for midi in and for analog-to-digital inputs (also piCapture needs some inputs too)

# gpio inputs for recur:

here are the pins needed for different parts of the recur connections:

note that pins 1 to 26 are covered by the lcd screen, even though not all are used by it

## lcd screen 

as stated above, the screen uses the following pins:
- 1 , 17 : 3.3v
- 2, 4 : 5v
- 11 : TP_IQR (for touch panel)
- 18 : LCD_RS
- 19 : LCD_SI
- 21 : TP_SO (touch panel output)
- 22 : reset
- 23 : LCD_SCK
- 24 : LCD_CS
- 26 : TP_CS

## piCapture

piCapture be default will use pins 3, 5, 7 to comunicate 

## serial-midi in

pin 10 (rx) is needed for midi in plus 3.3v (combined with octocoupler 6n138 etc and resistors)

## analog in

using a MCP3008 via hardware SPI, can connect up to 8 analog inputs using pins 35, 36, 38, 40 (SPI1) + 3.3v , can also connect with software SPI with any four pins if more inputs were needed. these inputs can be used for pots/sliders & gate/cv jacks. by default will react to 0-3.3v. if wanting to use larger range than this will need some kind of scaling electronics (tl074d?)

providing 4 pins on under the lcd screen cover can be accessed by the board (and 3.3v can be distributed) i should be able to create a circuit that connects all these inputs to the pi. 


[instructable]: http://www.instructables.com/id/PiMiDi-A-Raspberry-Pi-Midi-Box-or-How-I-Learned-to/
[adafruit tft display]: https://www.adafruit.com/product/2441
[raspi gpio]: https://www.raspberrypi.org/documentation/usage/gpio/
[mcp3008]: https://learn.adafruit.com/raspberry-pi-analog-to-digital-converters/mcp3008

</details>


<details>
<summary>2018-10 signal culture + future plans</summary> 

# signal culture + future plans

this is an update on the progress and new ideas as a direct result of my residency at signal culture (sept-oct 2018) and an outline of some of my future plans with this project

the initial hack that became recur was in response to a specific hole in my video-hardware workflow (~dec2017). the encouragement and enthusiasm for the idea on VideoCircuits was enough to motivate me to tidy and share recur_v1 on github/fb (may2018). a dozen or so people building this and talking about it was satisfying but i had no immediate plans to continue developing for it, besides maintenance, bugfixes, simple user requests etc...

however after being invited to a 3-week toolmaker residency in Owego NY i felt encouraged and enabled to explore some bigger new ideas for the instrument.

the name __r_e_c_u_r__ refers to the video sampling/looping feature at the core of this device. as the scope of what it can do is expanded, naming and organizing the (optional) extensions helps define their use.

## c_a_p_t_u_r

_an optional extension for live sampling through the pi camera input_

this was partially included in v1 although limited to inputs from the rpi-camera. while at SC i had the chance to try it with a piCaptureSd1 hat which allows sd video input from composite, component and svideo. some settings have been added to improve the captur image.

- TODO : experiment with capture from usb-webcam and cheep usb capure cards. with prob be lowfi and or laggy but also still fun.

## c_o_n_j_u_r

_an alternative openframeworks backend for extended video control and glsl-shader integration_

this is the largest addition from the v1 release. although omxplayer is a gpu-accelerated videoplayer that is stable and forgiving it is designed as a mediaplayer for watching films etc, not as a platform for creative coding. r_e_c_u_r can sequence omxplayer to playback samples exactly how they are (and seek etc for sublooping) .

openframeworks is more suited for video manipulation and opens a lot of possibilities to how the samples can be played.

### shaders

a few other projects have been based around using a raspberry pi as a visual instrument in another way - instead of focusing on video-clip playback, these play glsl-shaders, fragments of code that run directly on the gpu, allowing the creation of interesting digital visual generation.

although generated in real time, shader-playback is similar to video playback in that you can select a prepared source (video-file or shader-code) and perform with it - trigger when to start and stop, interact with parameters live (video start/stop/alpha/position or user defined shader parameters).

recur already has the ui to browse folders and files, select and map them, to display the relevant infomation, and openframeworks has the ability to load and play shaders much like you would load and play videos. this seemed like a logical and powerful extension to the sampler core.

## i_n_c_u_r

_become subject to (something unwelcome or unpleasant) as a result of one's own behavior or actions_

this is related to extending recurs sequencing/performability by exposing its controls to external manipulation.

usb-midi control over recur actions was in the v1 release including the first example of continuos inputs via midi-cc. this gives control over parameters which otherwise are difficult to interact with on a numpad alone (video/preview alpha). as the amount of control increases so does the need for continuous inputs.

at SC i created a circuit that allows 8 analog inputs (4 pots, 4 0-5v cv) and serial(din)-midi into recur.

## modulation

having defined these different areas of the __r_e_c_u_r__ video instrument, we also have created some powerful combinations (some are trivial/obvious like _i_n_c_u_r_ + _r_e_c_u_r_ for external sequencing of video samples, or _c_a_p_t_u_r_ + _r_e_c_u_r_ for recording live input directly followed by sampling it) others include:

- _i_n_c_u_r_ + _c_o_n_j_u_r_ : shaders can be written to respond in real time based on user input. usually this is in the form of the mouse-x&y position. for recur i have defined normalized parameter inputs that can be used to manipulate any portion of the glsl code. having knobs or cv control over these parameters greatly increases the playablity of the shaders.

- _r_e_c_u_r_ + _c_o_n_j_u_r_ : at first i was thinking of video-files and glsl-shaders as separate sources for creating video. however then i discovered how you can also _use_ a glsl-shader to process a video-file (shaders can take textures as input, one of which can be a video), leading me to make the distinction in recur between _0-input shaders_ and _1-input shaders_ .

- c_a_p_t_u_r_ + _c_o_n_j_u_r_ : not only can _1-input shaders_ accept video as a texture-input, they can also take texture from a live input (a camera or capture card for example). this means recur can also be used to process live video input in (almost) real time.

## direction

what started as a simple solution for seamless prerecorded video playback is starting to look something closer to the video equivalent to audios groovebox - where a (good) groovebox may include sampling, sequencing, synth-presets, audio-effects and live-input/effect-through , this new __r_e_c_u_r__ + _i_n_c_u_r_ + _c_o_n_j_u_r_ + _c_a_p_t_u_r_ may come close to a fully open, customizable and diy digital video groovebox.

## future plans

much of what is outlined above is in varying stages of development. proofs of concepts have been completed, but lots of the new (esp openframework) code is buggy and needs tidying and testing. for example my incur circuit is thrown together on perf-board and soldered straight to the pi...

here are some things im thinking about doing/trying from here:

- ~~get this messy web of features polished enough that its worth creating a new sd img so others can flash and try these software updates out (im including a switch between omxplayer backend to retain existing functionality, and the option to try and test the more experimental openframeworks backend )~~
- investigate sending external control (midi + analog readings) straight to openframeworks rather than python. this probably involves sacrificing some of the customizablity of mapping any input to any action for a performance increase (as it is i doubt my python code is fast enough to respond to eurorack without noticeable stepping...)
- ~~create another video demo to show these new ~~
- create a new physical version : this time using a raspi compute board on a custom pcb for eurorack standard with cv/trigger in circuits , faceplate and using the numpad detached via wifi (probably 20hp)
- hopefully have the software running stable enough and a buildpipe thats slightly more optimized than 'wait 9 hours for the case to print' so that small assembled runs become feasible for getting these to non-diy-ers
- ~~investigate the feasibility of creating even more accessible stripped version (composite only?) on the raspi0 (probably not feasible but iv been constantly surprized at what gpu accelerated sbc's can do!)~~
- collaborate on creating some workshops for: _intro to video hardware instruments : some hacky open diy projects_ ...


</details>