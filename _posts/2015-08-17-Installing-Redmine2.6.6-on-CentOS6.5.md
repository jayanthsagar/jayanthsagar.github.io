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

#### Install the dependencies packages.
<p>
4.These are the basic software packages for environment settings and utility tools to compile other packages in the next section.
</p>
```html
yum -y install nano zip unzip libyaml-devel zlib-devel curl-devel openssl-devel httpd-devel apr-devel apr-util-devel mysql-devel gcc ruby-devel gcc-c++ make postgresql-devel ImageMagick-devel sqlite-devel perl-LDAP mod_perl perl-Digest-SHA
```
#### Install Apache and MySQL
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
<p>
Allow start services when OS boot :
</p>
```html
chkconfig httpd on
chkconfig mysqld on
service httpd start
service mysqld start
```
<p>
Set the password for MySQL
</p>
```html
/usr/bin/mysql_secure_installation
```
<p>
Because we not have a password for the root account so you press Enter to skip.
</p>
```html
Enter current password for root (enter for none):
```
<p>
Select Yes to set the password for the MySQL root account.
</p>
```html
Set root password? [Y/n] y
```
<p>
Enter and confirm your password, remove the anonymous user, select Yes
</p>
```html
Remove anonymous users? [Y/n] y
```
<p>
Allow remote login to MySQL as root account, select No.
</p>
```html
Disallow root login remotely? [Y/n] n
```
<p>
Delete the test database, select Yes
</p>
```html
Remove test database and access to it? [Y/n] y
```
<p>
Reload privilege tables, select Yes
</p>
```html
Reload privilege tables now? [Y/n] y
```
#### Turn off SELinux
<p>
SELinux is a security feature advanced for Linux operating system, when installing the system you need to turn off this feature to get the process done smoothly, after successful you can turn on back if you want.
</p>
```html
nano /etc/selinux/config
```
<p>
Change the file content :
</p>
```html
SELINUX=disabled
```
#### Set up the Hostname
<p>
By default when installing a new OS Centos not set the hostname, so we need to setting with the command :
</p>
```html
nano /etc/hosts
```
<p>
Add your domain name or host name that you set on both the command line, save the file and exit, the server name will be changed when restarting.
</p>
#### Configuring the Firewall
<p>
We do not want to turn off the firewall because it's quite important, so you need to add rules to allow port 80 for HTTP and port 443 for HTTPS.

In the Centos OS, you can configuration firewall by editing files iptables and ip6tables.
</p>
```html
nano /etc/sysconfig/iptables
```
<p>
Press Enter to create a new line after the line of port 22, copy the following two commands and right click on the window to the Paste command.
</p>
```html
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT
```
<p>
Press CTRL + O to save the file and press CTRL + X to exit.
</p>
<p>
The same applies for IP6 firewall :
</p>
```html
nano /etc/sysconfig/ip6tables
```
<p>
Add these lines to the file.
</p>
```html
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT
```
<p>
After you finish editing both files, run the commands to apply the new rules for firewall.
</p>
```html
/etc/init.d/iptables restart
/etc/init.d/ip6tables restart
```
<p>
Allow turn on the firewall when reboot the operating system.
</p>
```html
chkconfig iptables on
chkconfig ip6tables on
```
<p>
Finally, we need to restart the system to apply the changes to the SELinux and Hostname.
</p>
```html
reboot
```
#### Install PHP and phpMyAdmin
<p>
Because we use MySQL database management system, so we need to install phpMyAdmin program management.

phpMyAdmin is a free open source tool written by PHP language to manage MySQL database via a web browser.

It can create, modify or delete databases, tables, fields or records, perform SQL statements, or managing users and permissions.

The command to install PHP and the packages :
</p>
```html
yum -y install php php-mysql php-gd php-imap php-ldap php-mbstring php-odbc php-pear php-xml php-xmlrpc php-pecl-apc php-soap
```
<p>
Restarting the Apache service :
</p>
```html
service httpd restart
```
<p>
And install phpMyadmin :
</p>
```html
rpm --import http://dag.wieers.com/rpm/packages/RPM-GPG-KEY.dag.txt
yum install http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm
yum -y install phpmyadmin
```
<p>
Editing the virtual host file to allow remote login to the phpMyadmin.
</p>
```html
nano /etc/httpd/conf.d/phpmyadmin.conf
```
<p>
Replace text "Allow from 127.0.0.1" to "Allow from all", save the file and exit.
</p>
<p>
Editing the configuration file for the phpMyadmin
</p>
```html
nano /usr/share/phpmyadmin/config.inc.php
```
<p>
Replace text :
</p>
```html
$cfg['Servers'][$i]['auth_type'] = 'cookie';
```
<p>
To :
</p>
```html
$cfg['Servers'][$i]['auth_type'] = 'http';
```
<p>
Save the file and exit, restarting the Apache service :
</p>
```html
service httpd restart
```
<p>
After successfully installed phpMyadmin, you can check at the address :
</p>
```html
http://your-domain/phpmyadmin
```
<p>
Login with account : root / your_password

With Password has been set at step install MySQL database in the above.
</p>
*Note:* If you install the Redmine system on the PC or in a virtual machine which not on the dedicated server, we need to switch the application phpMyadmin to run on port 8080 because port 80 will be used for Redmine in the next steps.

We need add a port 8080 to the firewall and change the VirtualHost for phpMyadmin.
```html
nano /etc/sysconfig/iptables
```
