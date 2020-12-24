## SuperPlayer-v3.0

#### This is intended to be a howto for the release of SuperPlayer V3.0

It's best to take one step at a time, e.g follow the *Table of Content*... 
Things should be able to work, but my guide here is far from bulletproof :exploding_head:
Tested on armv7 Raspberry Pi 3B+ & 4...

Remember it's kindoff complicated stuff, and somethings are hard to explain & understand.

##### To be continued...

We have two good way's of automatic samplerate switching now. The most "native" one is the Alsa like plugin ::
https://github.com/Lykkedk/Alsa_plugin_cdsp-samplerate-control
This one is proberly the most correct way of doing this, but i must say, the way user "pi r" over @ diyaudio.com figured out howto do this is working very good also :smiley: This way pi r created it is called *py_cdsp_samplerate_control* (Link #3 in Table of Contents) - I use this setup myself btw...\
... Well besides that, i have a working BLE (Bluetooth low energy) remote controller on the go, for controlling the Camilladsp volume / play & pause and other stuff. The controller is an ESP32 on battery; it's still under heavy development but look out for it here eventually!

*Help / thread here on diyaudio.com :: https://www.diyaudio.com/forums/pc-based/361429-superplayer-dsp_engine-camilladsp-samplerate-switching-esp32-remote-control.html*

<< ---------------------------------- >>

#### Table of Contents

0. [The story behind this](#Story)
1. [Setup the Raspberry Pi](#Setup)
2. [The Camilla GUI & Camilladsp installer script](#Installer-script)
3. [The py_cdsp_samplerate_control way of automatic switching samplerate](#py_cdsp_samplerate_control)
4. [The SuperPlayer Bluetooth controller/remote](#SuperPlayer_Remote)

<< ---------------------------------- >>

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
The samplerate detection is a hackup UP squeezelite player, which i did with some help from my older son ;-)... This way is deprecated now\
and there are other way's of doing this which work's much better!... !Stand by!

https://www.diyaudio.com/forums/pc-based/349818-camilladsp-cross-platform-iir-fir-engine-crossovers-correction-etc.html
https://www.picoreplayer.org/
http://tinycorelinux.net/ & http://forum.tinycorelinux.net/index.php/board,57.0.html

![SuperPlayer Logo](/SuperPlayer_v3.png)

*My workshop SuperPlayer testrig* 

<< ---------------------------------- >>

#### Setup

##### Download and install piCorePlayer (pCP) aka' this one :: 
https://repo.picoreplayer.org/insitu/piCorePlayer6.1.0/piCorePlayer6.1.0.zip

Extend filesystem as instructed for piCorePlayer. \[Main page, Resize
FS\]

SSH into the pCP/RPI ```ssh tc@192.168.1.95``` (with the right ip number
ofcause)\
Default password is: ```piCore``` 

##### Disable onboard analog audio on the Pi:

```
mount /mnt/mmcblk0p1
cd /mnt/mmcblk0p1
nano config.txt
```
Near the end of this file, comment-out\[\#\] the 2 lines right after the \#onboard audio delay line\
It should look like this afterwards:
```
# onboard audio overlay
#dtparam=audio=on
#audio_pwm_mode=2
```
##### Install python 3.6:

```tce-load -w -i python3.6``` (this loads and install’s python3.6 which we
shall use later)

##### Install needed editor:

```tce-load -w -i nano``` (RPI downloads and installs nano now)

Nano is an easy texteditor\
Quick use : \[ctrl\] + o = save, \[ctrl\] + x = exit

##### Save the machine
Save files on machine and reboot:

```sudo filetool.sh -b``` and 
```sudo reboot``` (Wait some and ssh into the machine again)

Now it's a good idea to have your'e DAC or whatever used to work, as to verify installation is good.\
##### Setup end's here, now you should have a "working" installation to move on with (hopefully)

<< ---------------------------------- >>

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
So now we need to start the GUI, which is done by ```sudo /home/tc/StartServer.sh```
If you wan't to have it start at boot, add the line to bootlocal.sh :: ```nano /opt/bootlocal.sh```
```
#!/bin/sh
# put other system startup commands here

GREEN="$(echo -e '\033[1;32m')"

echo
echo "${GREEN}Running bootlocal.sh..."
#pCPstart------
/usr/local/etc/init.d/pcp_startup.sh 2>&1 | tee -a /var/log/pcp_boot.log
#pCPstop------

/home/tc/StartServer.sh
```
Remember to save the machine ```sudo filetool.sh -b``` :raised_eyebrow:

There is a shiny new webinterface, with nice green bars, showing volume level and stuff ;-)

![CamillaGUI](/CamillaGUI.png)

<< ---------------------------------- >>

#### py_cdsp_samplerate_control
HOWTO ::

User pi r did explain the setup in a short and really good way :: 
(See this post here for more :: https://www.diyaudio.com/forums/pc-based/361429-superplayer-dsp_engine-camilladsp-samplerate-switching-esp32-remote-control-post6388205.html)

*The problem:*
*CamillaDSP needs to load a new config file whenever the sample rate at its input changes, and can't do that by itself.*
*The solution:*

    We hijack the log output from squeezelite, and sends it to a "named pipe".
    A Python daemon reads the log messages from the pipe,
    finds and extracts the sample rate of a new track to be played.
    Based on which sample rate, the daemon sends a message to CamillaDSP
    though a websocket call, containing the name of the config file to load.

*(A named pipe is a "trick" to make a pipe look like a file, that can be read in real time by another program. Just as with a "normal" pipe, like: ps | grep camilladsp)*

Below show's the tree structure from the py_cdsp_samplerate_control.tcz ::
```
squashfs
   ├── home
   │   └── tc
   │       └── camilladsp
   │           ├── coeffs
   │           ├── configs
   │           └── example
   │               ├── 44100.yml
   │               ├── 48000.yml
   │               ├── 88200.yml
   │               └── 96000.yml
   │       
   └── usr
       └── local
           ├── bin
           │   ├── camilladsp-config.sh
           │   ├── samplerate-daemon.py
           │   ├── samplerate-select.py
           │   └── squeezelite-cdsp.sh
           └── tce.installed
               └── py_cdsp_samplerate_control
```

#### Quick setup for the impatience one's :star_struck:

The .tcz for the install can be downloaded here :: https://github.com/Lykkedk/SuperPlayer-cdsp-samplerate-control/archive/v1.0.zip
Directly on the Raspberry this way ::\
```wget https://github.com/Lykkedk/SuperPlayer-cdsp-samplerate-control/archive/v1.0.zip```\
```unzip v1.0.zip```\
```cd SuperPlayer-cdsp-samplerate-control-1.0```\
```cp py_cdsp_samplerate_control.tcz /mnt/mmcblk0p2/tce/optional```\
```tce-load -i py_cdsp_samplerate_control.tcz```

Now add theese 3 lines at the end of your'e onboot.lst
```nano /mnt/mmcblk0p2/tce/onboot.lst```
```
py_websocket.tcz
camilladsp.tcz
py_cdsp_samplerate_control.tcz
```

Mine looks like this (don't mind the other stuff i have for now) ::
```
tc@TestRig:~$ cat /mnt/mmcblk0p2/tce/onboot.lst

pcp.tcz
pcp-6.1.0-www.tcz
nano.tcz
rpi-vc.tcz
pcp-bt6.tcz
bluez-5.54.tcz
py_websocket.tcz
camilladsp.tcz
py_cdsp_samplerate_control.tcz
```
Save files on machine and reboot:

```sudo filetool.sh -b``` and 
```sudo reboot``` (Wait some and ssh into the machine again)

For a start try to made the 44100.yml located in /home/tc/camilladsp/example/44100.yml work with your'e DAC or whatever used.
Look out for the *format: S32LE* string; make sure your'e DAC can handle it. 

```
devices:
  samplerate: 44100
  chunksize: 4096
  queuelimit: 1

  capture:
    type: File
    channels: 2
    filename: /dev/stdin
    format: S32LE
  playback:
    type: Alsa
    channels: 2
    device: "hw:0,0"
    format: S32LE

filters:
  clipgain:
    type: Gain
    parameters:
      gain: 0.0
      inverted: false

  Baseboost:
    type: Biquad
    parameters:
      type: Highshelf
      freq: 80
      gain: -6
      slope: 6

  volume_lr:
    type: Volume
    parameters:
      ramp_time: 200
                          
pipeline:
- type: Filter
  channel: 0
  names:
    - clipgain
    - Baseboost
    - volume_lr
- type: Filter
  channel: 1
  names:
    - clipgain
    - Baseboost
    - volume_lr
```

When edited to your'e setup try to restart the samplerate control setup like this ::
```
c@TestRig:~$ camilladsp-config.sh -n example

- Setting CamillaDSP configuration to: example

- Creates or updates symbolic links in folder: /home/tc/camilladsp
    44100.yml -> example/44100.yml
    48000.yml -> example/48000.yml
    88200.yml -> example/88200.yml
    96000.yml -> example/96000.yml

Sending signal to CamillaDSP to apply configuration: example
```
 When all this is working paste the line ```sudo /usr/local/bin/squeezelite-cdsp.sh start &``` to have it start at boot time :: 
 
 ```
tc@TestRig:~$ cat /opt/bootlocal.sh

#!/bin/sh
# put other system startup commands here

GREEN="$(echo -e '\033[1;32m')"

echo
echo "${GREEN}Running bootlocal.sh..."
#pCPstart------
/usr/local/etc/init.d/pcp_startup.sh 2>&1 | tee -a /var/log/pcp_boot.log
#pCPstop------

/home/tc/StartServer.sh
sudo /usr/local/bin/squeezelite-cdsp.sh start &
 ```
#### Hey good luck...

This is complicated stuff, and it's hard to explain different scenarios, look here and please ask quistions if you like ::\
https://www.diyaudio.com/forums/pc-based/361429-superplayer-dsp_engine-camilladsp-samplerate-switching-esp32-remote-control.html
https://www.diyaudio.com/forums/pc-based/361429-superplayer-dsp_engine-camilladsp-samplerate-switching-esp32-remote-control-post6388205.html


<< ---------------------------------- >>

#### SuperPlayer_Remote
NOT DONE YET, Under construction ;-) Place for the BLE remote

User TNT over @ diyaudio.com eventually posted this https://www.diyaudio.com/forums/pc-based/361429-superplayer-dsp_engine-camilladsp-samplerate-switching-esp32-remote-control-post6381474.html ... And my journey into Bluetooth / BLE began.

<< ---------------------------------- >>  

Good luck :grin: [ JesperLykke / user lykkedk @ diyaudio.com ]
