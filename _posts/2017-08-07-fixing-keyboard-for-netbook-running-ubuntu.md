---
layout: post
title: Fixing netbook keyboard running ubuntu
---
<p class="message">
When we install ubuntu on a netbook, everything works fine except keyboard, this will ne annoying as you need to carry external keyboard always with you to work. So, here is a fix to it.
</p>
<p>
I have taken reference from this link <a href="http://blog.yjl.im/2010/08/disable-laptop-ps2-at-keyboard-i8042.html">Here</a>
</p><p>I have copied the C program from above link to a file named keyboard.c
</p>
<p>Following steps will help you compile the code and run it on boot time which eneables keyboard as soon as you start your computer.

<p>1.Compile the C program using #gcc -o keyboard keyboard.c</p>
<p>Copy the output file to your desired location (/home/superman/keyboard)
</p><p>To run the keyboard on startup we will set a init script in /etc/init.d/keyboard with following:</p>
### a
```html #!/bin/sh -e
	### BEGIN INIT INFO
	# Provides:          keyboard
	# Required-Start:    
	# Required-Stop:     
	# Should-Start:      checkroot
	# Should-Stop:
	# Default-Start:     S
	# Default-Stop:
	# Short-Description: Load the modules listed in /etc/modules.
	# Description:       Load the modules listed in /etc/modules.
	### END INIT INFO
	exec /home/superman/keyboard 1
```
<p>Add following code to /etc/init/keyboard</p>
### a

``` description	"Fixing keyboard issue in lenovo ideapad"

	start on runlevel [2345]
	stop on runlevel [!2345]

	exec /etc/init.d/keyboard
```
<p> Final step run the following command to update </p>
### a
``` sudo update-rc.d keyboard defaults``` 

#### From next boot your machine gets booted with keyboard enabled
