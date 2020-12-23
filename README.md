## SuperPlayer-v3.0

This is intended to be a new howto for a new release of SuperPlayer.
I have a working BLE (Bluetooth low energy) remote controller on the go, for controlling the Camilladsp volume / play & pause and other stuff.\
The controller is an ESP32 on battery; it's still under heavy development but look out for it here eventually!
I will also write a guide howto setup diyaudio user [ pi r ]'s excellent samplerate switcher for automatic switching between samplerates in conjunction\
with Camilladsp ofcause... My old original SuperPlayer is deprecated now, and i really like the way [pi r] handled and created her script's... 

#### Table of Contents
1. [The Camilla GUI & Camilladsp installer script](#Installer-script)
2. [Example2](#example2)
3. [Third Example](#third-example)
4. [Fourth Example](#fourth-examplehttpwwwfourthexamplecom)





#### Installer-script

##### The first thing to try is the script i created to have the newest camilladsp and freind's ::

I have created an installer script, so to have newest camilladsp-v0.5.0-beta2 along with the GUI.

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

The script is then executed like this : ```sudo ./install_rpi_tinycore.sh```
Along with the GUI the installer also install's the Camilladsp version for the new GUI ::

squashfs-root\
└── usr\
    └── local\
        └── bin\
            └── camilladsp\

There is a shiny new webinterface, with nice green bars, showing volume level and stuff ;-)

![CamillaGUI](/CamillaGUI.png)

Good luck ;-)  [ JesperLykke / user lykkedk @ diyaudio.com ]
