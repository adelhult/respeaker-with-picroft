# Respeaker 4-Mic Array with Picroft
**How i got the [Seeeds ReSpeaker 4-Mic Array](https://www.seeedstudio.com/ReSpeaker-4-Mic-Array-for-Raspberry-Pi-p-2941.html) working together with [Picroft](https://github.com/MycroftAI/enclosure-picroft) (2018-09-11 Stretch Lightning) running on a Raspberry Pi 3B+.**

Note: this documentation was only written as a reminder for myself on how to do this.

These first instructions are taken from Seeeds offical [documentation](https://github.com/SeeedDocument/ReSpeaker-4-Mic-Array-for-Raspberry-Pi/blob/master/ReSpeaker-4-Mic-Array-for-Raspberry-Pi.md)

Get the voice card source code.
```
git clone https://github.com/respeaker/seeed-voicecard.git
cd seeed-voicecard
sudo ./install.sh 4mic
reboot
```
Select the headphone jack for output.
```
sudo raspi-config
# Select 7 Advanced Options
# Select A4 Audio
# Select 1 Force 3.5mm ('headphone') jack
# Select Finish
```
You can now check the sound card name with this command. Mine did not look exactly the same as Seeeds showed in their complete documentation.
```
pi@raspberrypi:~/seeed-voicecard $ arecord -L
```
Now you should be able to try the microphone:
```
arecord -Dac108 -f S32_LE -r 16000 -c 4 hello.wav
aplay hello.wav 
```
However, to get the microphone to actually work together with Mycroft you have to download pulseaudio.
```
sudo apt-get install pulseaudio
reboot
```
After doing this the audio indicator in the Mycroft CLI should now be moving. Although, the audio output might of stopped working. 

To fix the audio output I tried to remove the *suspend on idle* setting in this file ` /etc/pulse/default.pa ` but i don't think it actually has an impact so you can try to skip this step.

Using this command you can see that the audio level for the output is set to 0%.
```
pactl list-sinks
```
To change the volume use:
```
pactl set-sink-volume 0 95%
```
To ensure that this setting will persist you could add the command to the `audio_setup.sh` file.
