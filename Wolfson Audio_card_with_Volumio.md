Wolfson Audio card working on a Raspberry Pi 1 B, great software.

It was fairly easy to do, I previously had it working manually following the instructions here http://www.horus.com/~hias/cirrus-driver.html and selecting the DAC in the webUI. Making sure I had a back up of my working SD card, needless to say on updating through the webUI and restart, I had no sound.

All I had to do was add a line to /volumio/app/plugins/system_controller/i2s_dacs/dacs.json after RPi-DAC:
CODE: SELECT ALL
    {"id":"rpi-cirrus","name":"RPi-Cirrus","overlay":"rpi-cirrus-wm5102","alsanum":"1","mixer":"","modules":"","script":"","needsreboot":"yes"},

Restart and then I had sound and volume control works on Youtube and Spotify :D, only frustration is I seem to have lost my local music database  :( (next thing to fix).

I assume that if you start from a clean system then doing the above will not result in sound, but if after your first reboot having selected the Wolfson card, then carry out some of the instructions on http://www.horus.com/~hias/cirrus-driver.html please check on the original that I have copied correctly i.e.
1. ssh into your Volumio, log in as volumio default password volumio
2. Create a file /etc/modprobe.d/cirrus.conf with the following content i.e. sudo nano /etc/modprobe.d/cirrus.conf:
CODE: SELECT ALL
softdep arizona-spi pre: arizona-ldo1

3. Download cirrus-ng-scripts.tgz and extract it for example in /home/pi/bin.
CODE: SELECT ALL
cd ~
wget http://www.horus.com/~hias/tmp/cirrus/cirrus-ng-scripts.tgz
mkdir bin
cd bin
tar zxf ../cirrus-ng-scripts.tgz

4. You have to start the appropriate scripts before you can use the card. For example:
CODE: SELECT ALL
./Reset_paths.sh
./Playback_to_Lineout.sh
./Playback_to_SPDIF.sh
./Record_from_Linein.sh


5. Then check on http://www.horus.com/~hias/cirrus-driver.html to see if there is anything else you want to change and check the troubleshooting if it doesn't work for you.

My /boot/config.txt now contains:
CODE: SELECT ALL
initramfs volumio.initrd
gpu_mem=16
max_usb_current=1
#dtparam=audio=on
audio_pwm_mode=2
dtparam=i2c_arm=on
disable_splash=1

#### Volumio i2s setting below: do not alter ####
dtoverlay=rpi-cirrus-wm5102


Needless to say all errors are mine, all great stuff is down to the Team for Volumio and HIAS the Wolfson insights.

Source: https://forum.volumio.org/wolfson-audio-card-with-volumio-t851-230.html
