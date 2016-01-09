---
layout: post
title: Record your desktop audio and video
---
<p class="message">
This comes handy when you were not allowed to download a video from a website but allowed to play.
</p>
<p>
Libav is the library which is going to help us in such situations.
</p><p>Libav is a set of tools which contains following:
</p><ol>
1.Avplay - A video/audio player like VLC.
2.Avconv - A multimedia converter plus a video & audio recorder from different sources.
3.Avprobe - A tool that connects to the multimedia file stream and returns many useful information and statistics about it.
4.Libavfilter - A filtering API for different Libav tools.
</ol>
<p>
Step1: Install Avconv tool
</p>
```html

sudo apt-get update
sudo apt-get install libav-tools
```
<p>
Step2: Start video recording of desktop
</p>
```html
avconv -f x11grab -r 25 -s 1366x768 -i :0.0 -vcodec libx264 -threads 4 $HOME/output.avi
```

<p>
Explanation of above command:
<ul>
-    avconv -f x11grab is the default command to capture video from the X server.
-    -r 25 is the frame rate you want, you may change it if you like.
-    -s 1920×1080 is your system’s screen resolution, change it to your current system resolution, it’s very important to do this.
-    -i :0.0 is where we want to set our recording start point, leave it like this.
-    -vcodec libx264 is the video codec that we’re using to record the desktop.
-    -threads 4 is the number of threads, you may change it as well if you like.
-    $HOME/output is the destination path where you want to save the file.
-    .avi is the video format, you may change it to “flv”, “mp4”, “wmv”, “mov”, “mkv”.
</ul>
Ctrl+c will terminate the command and save the output file.

open the output using VLC or avplay
</p>
<p>
Step3: Start Video and Audio recorning of your desktop

To record both audio and video we should know the hardware address of our output hardware(speaker)
</p>
```html
arecord -l
```
<p>
The above command will list the available output hardware.
Now start the recording using
</p>
```html
avconv -f alsa -i hw:0 -f x11grab -r 25 -s 1366x768 -i :0.0 -vcodec libx264 -threads 4 output-file2.avi
```
<p>
Explanation:
-    -f alsa is an option to capture the sound from the alsa device.
-    -i hw:1 is an option to take the audio input source from the “hw:1” device which is the first – and the only – input sound device in my computer.
</p>
