---
layout: post
title: Installing Redmine2.6.6 on CentOS6.5
---

<p class="message">
CentOS is one of the most frequently chosen Linux operating systems for Linux based production environments. And Redmine is one of the best (if not THE best) open source issue tracking and project management applications, because of it is developed using Ruby on Rails, it feels like complex to deploy for anyone not familiar with the Ruby on Rails environment. The following steps will make the task simple as I am writing it side by doing all of the steps.
</p>

<p>
1.Login to Centos6.5 machine.
</p>
<p>
2.Update packages.
</p>
```html
yum update
```
<p>
3.Reboot the machine after update.
</p>
```html
reboot
```

## Install the dependencies packages.
<p>
4.These are the basic software packages for environment settings and utility tools to compile other packages in the next section.
</p>
```html
yum -y install nano zip unzip libyaml-devel zlib-devel curl-devel openssl-devel httpd-devel apr-devel apr-util-devel mysql-devel gcc ruby-devel gcc-c++ make postgresql-devel ImageMagick-devel sqlite-devel perl-LDAP mod_perl perl-Digest-SHA
```
