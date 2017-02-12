# How to install ALSA EQUALIZER on Volumio 2 with HiFiBerry


## Step 1: 
### Update the system

```
sudo apt-get update -y
```

## Step 2: 
### Install the alsaeq modules

```
sudo apt-get install -y libasound2-plugin-equal
```

## Step 3: 
### Detect the device number of your sound card

```
aplay -l
```

#### Raspberry pi with HiFiBerry
![alt text](https://github.com/thomasdoerr/volumio-equalizer/blob/master/images/volumio-with-hifiberry-dac-plus.png "Raspberry pi with HiFiBerry")


## Step 4:
### Set the mixer type to hardware
![alt text](https://github.com/thomasdoerr/volumio-equalizer/blob/master/images/volumio-mixer-type-hardware.png "Set Mixer Type to Hardware")

## Step 5: 
### Configure asound.conf

```
cd /etc
sudo nano -w asound.conf
```

Paste this into the new file or add it at the end, if volumio already created that file.

```

ctl.equal {
  type equal;
}

pcm.plugequal {
  type equal;
  # Modify the line below if you don't
  # want to use sound card 1.
  
  slave.pcm "plughw:1,0";
  # slave.pcm "plughw:0,0"; for card 0 

  # or if you want to use with multiple applications output to dmix
  # slave.pcm "plug:dmix"
}

pcm.!default {
  type plug;
  slave.pcm plugequal;
}

```

## Step 6: 
### Change the mpd configuration

```
cd /etc
sudo nano mpd.conf
```

Search for the device entry in the audio_output and comment the entry by adding a `#` at the beginning of the line.
Create a new entry which sets the device to the EQUALIZER. "plug:plugequal"

```
# Audio Output ################################################################
audio_output {
  ...
                # device        "hw:1,0"
                device          "plug:plugequal" 
  ...                
```


## Setp 7: 
### Configure the mpd to use the EQUALIZER

```
sudo -H -u mpd alsamixer -D equal
```

![alt text](https://github.com/thomasdoerr/volumio-equalizer/blob/master/images/alsamixer.png "AlsaMixer")

## Setp 8: 
### Reboot the system


## Links:
This article tries to summaries to following links. I documents the way, it worked on my raspberry pi 3 with HiFiBerry. It didn't work on the same device with a Sound Blaster USB device instead of HiFiBerry.

[https://support.hifiberry.com/hc/en-us/articles/205311292-Adding-equalization-using-alsaeq](https://support.hifiberry.com/hc/en-us/articles/205311292-Adding-equalization-using-alsaeq)
[https://volumio.org/forum/equalizer-t45.html](https://volumio.org/forum/equalizer-t45.html)
[http://www.hifi-forum.de/viewthread-253-600.html](http://www.hifi-forum.de/viewthread-253-600.html)
[http://wiki.cyberleo.net/wiki/KnowledgeBase/AlsaEqual](http://wiki.cyberleo.net/wiki/KnowledgeBase/AlsaEqual)
[https://volumio.org/forum/equalizer-t45.html](https://volumio.org/forum/equalizer-t45.html)
[https://support.hifiberry.com/hc/en-us/community/posts/207226345-Problem-configuring-alsa-equalizer](https://support.hifiberry.com/hc/en-us/community/posts/207226345-Problem-configuring-alsa-equalizer)


