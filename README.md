# Howto install ALSA EQUALIZER on Volumio 2


## Step 1: 
### Update System

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

## Step 4: 
### Configure asound.conf

```
cd /etc
sudo nano -w asound.conf
```

Paste this into the new file

```

ctl.equal {
  type equal;
}

pcm.plugequal {
  type equal;
  # Modify the line below if you don't
  # want to use sound card 0.
  slave.pcm "plughw:0,0";
  # slave.pcm "plughw:1,0"; for card 1 for instance
  # or if you want to use with multiple applications output to dmix
  # slave.pcm "plug:dmix"
}

pcm.!default {
  type plug;
  slave.pcm plugequal;
}

```

## Step 5: 
### Change mpd Configuration

```
cd /etc
sudo nano mpd.conf
```

Search for the device entry in the audio_output and comment the entry by adding a #.
Create a new entry which sets the device to the EQUALIZER. "plug:plugequal"

```
# Audio Output ################################################################
audio_output {
  ...
                # device        "hw:0,0"
                device          "plug:plugequal" 
  ...                
```

## Setp 6: 
### Configure mpd to use the EQUALIZER

```
sudo -H -u mpd alsamixer -D equal
```

