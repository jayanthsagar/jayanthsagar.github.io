---
layout: post
title: Fixing netbook keyboard running ubuntu
---
<p class="message">
When we install ubuntu on a netbook, everything works fine except keyboard, this will ne annoying as you need to carry external keyboard always with you to work. So, here is a fix to it.
</p>
<p>
I have taken reference from this link <a href="http://blog.yjl.im/2010/08/disable-laptop-ps2-at-keyboard-i8042.html">Here</a>
   
 #include <unistd.h>
#include <sys/io.h>

#define I8042_COMMAND_REG 0x64

int main(int argc, char *argv[]) {
  char data = 0xae; // enable keyboard

  ioperm(I8042_COMMAND_REG, 1, 1);

  if (argc == 2 && argv[1][0] == '0')
    data = 0xad; // disable keyboard
  outb(data, I8042_COMMAND_REG);
  return 0;
  }

</p><p>I have copied the C program from above link to a file named keyboard.c
</p>
<p>Following steps will help you compile the code and run it on boot time which eneables keyboard as soon as you start your computer.

<p>1. Compile the C program using #gcc -o keyboard keyboard.c</p>
<p>2. Copy the output file to your desired location (/home/superman/keyboard)
</p><p>3. To run the keyboard on startup we will set a init script in /etc/init.d/keyboard with following:</p>
``` #!/bin/sh -e
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
<p>4. Add following code to /etc/init/keyboard</p>

```  description "Fixing keyboard issue in lenovo ideapad"
     start on runlevel [2345]
     stop on runlevel [!2345]
     exec /etc/init.d/keyboard
```
<p> 5. Final step run the following command to update </p>
``` sudo update-rc.d keyboard defaults
``` 
#### From next boot your machine gets booted with keyboard enabled
