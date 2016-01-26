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
</p>
<p>
<ol>
<li>Avplay - A video/audio player like VLC.</li>
<li>Avconv - A multimedia converter plus a video & audio recorder from different sources.
</li><li>Avprobe - A tool that connects to the multimedia file stream and returns many useful information and statistics about it.
</li><li>Libavfilter - A filtering API for different Libav tools.
</li></ol>
</p>
#### Step1: Install Avconv tool

```html

sudo apt-get update
sudo apt-get install libav-tools
```

#### Step2: Start video recording of desktop

```html
avconv -f x11grab -r 25 -s 1366x768 -i :0.0 -vcodec libx264 -threads 4 $HOME/output.avi
```

<p>
Explanation of above command:
<ul>
<li>    avconv -f x11grab is the default command to capture video from the X server.
</li><li>    -r 25 is the frame rate you want, you may change it if you like.
</li><li>    -s 1920×1080 is your system’s screen resolution, change it to your current system resolution, it’s very important to do this.
</li><li>    -i :0.0 is where we want to set our recording start point, leave it like this.
</li><li>    -vcodec libx264 is the video codec that we’re using to record the desktop.
</li><li>    -threads 4 is the number of threads, you may change it as well if you like.
</li><li>    $HOME/output is the destination path where you want to save the file.
</li><li>    .avi is the video format, you may change it to “flv”, “mp4”, “wmv”, “mov”, “mkv”.
</li></ul>
Ctrl+c will terminate the command and save the output file.

open the output using VLC or avplay
</p>
#### Step3: Start Video and Audio recorning of your desktop
<p>
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
<ul>
<li>    -f alsa is an option to capture the sound from the alsa device.
</li><li>    -i hw:1 is an option to take the audio input source from the “hw:1” device which is the first – and the only – input sound device in my computer.
</li></ul>
</p>

<div id="disqus_thread1"></div>
<script>
    /**
     *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
     *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables
     */
    /*
    var disqus_config = function () {
        this.page.url = "jayanthsagar.github.io";  // Replace PAGE_URL with your page's canonical URL variable
        this.page.identifier = "record_audio_video"; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    */
    (function() {  // DON'T EDIT BELOW THIS LINE
        var d = document, s = d.createElement('script');
        
        s.src = '//jayanthsagargithubio.disqus.com/embed.js';
        
        s.setAttribute('data-timestamp', +new Date());
        (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>

