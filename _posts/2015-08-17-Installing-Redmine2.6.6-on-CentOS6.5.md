---
layout: post
title: Installing Redmine2.6.6 on CentOS6.5
---

<p>
1.Login to Centos6.5 machine.
</p>
<p>
2.Update packages.
```html
yum update
```
</p>
<p>
3.Reboot the machine after update.
```html
reboot
```
</p>
<p>
4.Install the dependencies packages

These are the basic software packages for environment settings and utility tools to compile other packages in the next section.
```html
yum -y install nano zip unzip libyaml-devel zlib-devel curl-devel openssl-devel httpd-devel apr-devel apr-util-devel mysql-devel gcc ruby-devel gcc-c++ make postgresql-devel ImageMagick-devel sqlite-devel perl-LDAP mod_perl perl-Digest-SHA
```
</p>
