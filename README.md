## SuperPlayer-v3.0

This is intended to be a new howto for a new release of SuperPlayer.
I have a working BLE (Bluetooth low energy) remote controller on the go, for controlling the Camilladsp volume / play & pause and other stuff.\
The controller is an ESP32 on battery; it's still under heavy development but look out for it here eventually!
I will also write a guide howto setup diyaudio user [pi r]'s excellent samplerate switcher for automatic switching between samplerates in conjunction\
with Camilladsp ofcause... 
##### To be continued...

*Help / thread here on diyaudio.com :: https://www.diyaudio.com/forums/pc-based/361429-superplayer-dsp_engine-camilladsp-samplerate-switching-esp32-remote-control.html*

#### Table of Contents
0. [The story behind this](#Story)
1. [The Camilla GUI & Camilladsp installer script](#Installer-script)
2. [The py_cdsp_samplerate_control way of automatic switching samplerate](#py_cdsp_samplerate_control)
3. [The SuperPlayer Bluetooth controller/remote](#SuperPlayer_Remote)

#### Story

#### SuperPlayer changes fir-filters on the fly when samplerate changes 

Around feburary 2020 i started looking into the possiblities to make some correction for my room & speakers\
Not easy to get started, but i learned the basic and still learning ofcause ;-)\
Around the same time, the camilladsp thread over at diyaudio started\
CamillaDSP is the convolver/DSP engine i use for mine corrections, and they do make a difference.

I'am a long time diyaudio, electronics guy who had build a ton of differents stuff within many years now\
I like the combination of microprocessors and audio setup, gives a hole lot of possibilities.

I started around 2012-2013 with programming the raspberry pi as a player for my Logitech Media Server (LMS), using the TinyCore or more like PiCore distribution\
Later some clever guy's expanded this and did the piCorePlayer, which is a very good and solid platform for us audiophile raspberry pi guy's

So, as i know the background and OS which is underneeth the pCP i figured out howto set it up for my needs\
The possibility to have an player which automatically change fir-filters etc... when a samplerate is detected\
The samplerate detection is a hackup UP squeezelite player, which i did with some help from my older son ;-)

https://www.diyaudio.com/forums/pc-based/349818-camilladsp-cross-platform-iir-fir-engine-crossovers-correction-etc.html
https://www.picoreplayer.org/
http://tinycorelinux.net/ & http://forum.tinycorelinux.net/index.php/board,57.0.html

![SuperPlayer Logo](/SuperPlayer_v3.png)

*My workshop SuperPlayer testrig* 

#### Installer-script 
##### (Created and tested on piCore armv7 & piCorePlayer 6.10)

##### The first thing to try is the script i created to have the newest camilladsp and freind's ::

Installer script - Camilladsp-v0.5.0-beta2 along with the GUI.

* camilladsp v0.5.0-beta2      https://github.com/Lykkedk/SuperPlayer_Camilladsp/archive/v0.5.0-beta-2.zip
* pycamilladsp v0.5.0          https://github.com/HEnquist/pycamilladsp/archive/v0.5.0.zip
* pycamilladsp-plot v0.4.3     https://github.com/HEnquist/pycamilladsp-plot/archive/v0.4.3.zip
* camillagui-backend v0.6.0    https://github.com/HEnquist/camillagui-backend/releases/download/v0.6.0/camillagui.zip
* Websocket & Six              https://github.com/Lykkedk/SuperPlayer_Websocket/archive/v1.0.zip
* StartServer.sh               https://github.com/Lykkedk/SuperPlayer-Extensions/archive/CamillaGUI-start-v1.0.zip


The installer is downloaded here :: https://github.com/Lykkedk/SuperPlayer-Extensions/archive/CDSP-install-v1.0.zip

It could be directly downloaded into the Raspberry Pi with wget...\
```wget https://github.com/Lykkedk/SuperPlayer-Extensions/archive/CDSP-install-v1.0.zip```

When it's downloaded, unzip ```unzip CDSP-install-v1.0.zip``` and go to the created directory...\
```cd /home/tc/SuperPlayer-Extensions-CDSP-install-v1.0``` Then execute ```chmod +x install_rpi_tinycore.sh```
to make the script executeable.

The script is then executed like this : ```sudo ./install_rpi_tinycore.sh```\
Along with the GUI the installer also install's the Camilladsp(.tcz = piCore package) version for the new GUI ::\
Filesystem tree structure shown below for camilladsp.tcz
```
squashfs
   └── usr
        └── local
              └── bin
                   └── camilladsp
```

There is a shiny new webinterface, with nice green bars, showing volume level and stuff ;-)

![CamillaGUI](/CamillaGUI.png)

<< ---------------------------------- >>

#### py_cdsp_samplerate_control
Place for the cdsp samplerate control

Below show's the tree structure from the py_cdsp_samplerate_control.tcz ::
```
squashfs-root
   ├── home
   │   └── tc
   │       └── camilladsp.tar.gz
   └── usr
        └── local
             ├── bin
             │    ├── camilladsp-config.sh
             │    ├── samplerate-daemon.py
             │    ├── samplerate-select.py
             │    └── squeezelite-cdsp.sh
             └── tce.installed
             └── py_cdsp_samplerate_control

```

<< ---------------------------------- >>

#### SuperPlayer_Remote
Place for the BLE remote

<< ---------------------------------- >>  

Good luck ;-)  [ JesperLykke / user lykkedk @ diyaudio.com ]
