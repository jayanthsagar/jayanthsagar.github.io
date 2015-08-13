---
layout: post
title: Know about SSH Forced command
---
<p class="message">
Sometimes System Admins will come across a situation to add users public keys to a production server, but want to restrict them executing only few commands. SSH Forced Command helps in such case.
</p>


What is a SSH Forced Command?
   Forced command is used if the users can only connect through ssh. Essentially, whenever the user connects through ssh with a certain key or a certain username, you force him to execute a command (or a script) you determined in the .ssh/authorized_keys. Other commands issued by the users will be ignored.
```html
   # in .ssh/authorized_keys 
   command="cd /root/ && service.py arg1 arg2 arg3",no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty ssh-rsa public_key
```
*Note:*  you need these options to secure a forced command: 
"no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty"
