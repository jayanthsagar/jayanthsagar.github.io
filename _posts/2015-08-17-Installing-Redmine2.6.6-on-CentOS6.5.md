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

### Install the dependencies packages.
<p>
4.These are the basic software packages for environment settings and utility tools to compile other packages in the next section.
</p>
```html
yum -y install nano zip unzip libyaml-devel zlib-devel curl-devel openssl-devel httpd-devel apr-devel apr-util-devel mysql-devel gcc ruby-devel gcc-c++ make postgresql-devel ImageMagick-devel sqlite-devel perl-LDAP mod_perl perl-Digest-SHA
```
### Install Apache and MySQL
<p>
5.Apache is a server application for communicating over the HTTP protocol. Apache runs on operating systems such as Unix, Linux, Microsoft Windows, and other operating systems.

Apache play an important role in the development of the internet and the world wide web.

MySQL is the database management free open source most popular on the world, MySQL has high speed, stability and ease of use, portability, operating on multiple operating systems offer a large system is very powerful utility functions.

With the speed and high security, MySQL is well suited for applications that access databases on the internet.

Use the following command to install :
</p>
```html

yum -y install httpd mysql mysql-server
```
Allow start services when OS boot :
```html
chkconfig httpd on
chkconfig mysqld on
service httpd start
service mysqld start
```
Set the password for MySQL
```html
/usr/bin/mysql_secure_installation
```
Because we not have a password for the root account so you press Enter to skip.
```html
Enter current password for root (enter for none):
```
Select Yes to set the password for the MySQL root account.
```html
Set root password? [Y/n] y
```
Enter and confirm your password, remove the anonymous user, select Yes
```html
Remove anonymous users? [Y/n] y
```
Allow remote login to MySQL as root account, select No.
```html
Disallow root login remotely? [Y/n] n
```
Delete the test database, select Yes
```html
Remove test database and access to it? [Y/n] y
```
Reload privilege tables, select Yes
```html
Reload privilege tables now? [Y/n] y
```
