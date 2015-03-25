---
layout: post
title: Installing Sublime Text on Ubuntu14.04
---

<div class="message">
  This article includes steps to install Sublime Text on Ubuntu
</div>
<h6>Run the following commands on your terminal</h6>
* sudo add-apt-repository -y ppa:webupd8team/sublime-text-3.

<h6> If your machine is behind a proxy, we need to preserve existin environment variables using '-E' </h6>
* sudo -E add-apt-repository -y ppa:webupd8team/sublime-text-3.

<h6>Update and Install sublime:</h6>
* sudo apt-get update; sudo apt-get install -y sublime-text-installer