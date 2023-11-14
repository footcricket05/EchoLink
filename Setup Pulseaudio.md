## Setup PulseAudio

You need to set up Pulse Audio in every Raspberry Pi that plays music to the connected speaker. Set up PulseAudio by default in every Raspberry Pi as my initial set up.

To install the necessary files, run the following command first
```
sudo apt-get install pulseaudio pulseaudio-module-zeroconf alsa-utils avahi-daemon -y
```

To enable ALSA:
```
sudo modprobe snd-bcm2835                      # load module for single boot
echo "snd-bcm2835" | sudo tee -a /etc/modules  # load module for persistance
```

To set up networking, modify the default pulse audio settings file by running the following command:

```
sudo vi /etc/pulse/default.pa
```

and uncomment the two lines lines

```
load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1;192.168.0.0/16
load-module module-zeroconf-publish
```
while you are in that file, remove or comment the line:
```
load-module module-suspend-on-idle
```
This prevents PulseAudio from sending the audio hardware to sleep.

To start the pulseaudio server, run:
```
pulseaudio -D
```

You should be able to test that it works by playing a wav file with aplay (sudo apt-get install pulseaudio-utils), and also over the network by selecting the sink in your system's sound control panel.

You can change the default audio output to 3.5mm jack. 1 is 3.5 jack, 2 is HDMI and 0 is auto
```
amixer cset numid=3 1
```

Also - make sure the volume is set to high to begin with. The volume can be changed via the UI in the mopidy when playing music.

Run the following command to set volume in UI
```
alsamixer
```
or 

```
amixer set 'PCM' 100% unmute
```

or

```
amixer set 'Master' 100% unmute
```

That's it! You have just enabled and configured Pulse Audio driver, and your Raspberry Pi is ready to play audio through the sound card.

## Troubleshooting

### Is your Pulse Audio skipping the audio?

In /etc/pulse/default.pa, replace the line:

```
load-module module-hal-detect
```
with:
```
load-module module-hal-detect tsched=0 
```
Or:

load-module module-udev-detect
with:
```
load-module module-udev-detect tsched=0 
```

If you are running into issues, yu may want to visit [https://fedoraproject.org/wiki/How_to_debug_PulseAudio_problems](https://fedoraproject.org/wiki/How_to_debug_PulseAudio_problems)

[Back to home page](README.md)
