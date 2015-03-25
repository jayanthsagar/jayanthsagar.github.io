---
layout: post
title: Installing Sublime Text on Ubuntu14.04
---

<div class="message">
  This article includes steps to install Sublime Text on Ubuntu
</div>
* sudo add-apt-repository -y ppa:webupd8team/sublime-text-3
### If your machine is behind a proxy, we need to preserve existin environment variables using '-E'
* sudo -E add-apt-repository -y ppa:webupd8team/sublime-text-3
### Update and Install sublime:
* sudo apt-get update; sudo apt-get install -y sublime-text-installer