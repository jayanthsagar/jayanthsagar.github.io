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

We need to add a port 8080 to the firewall and change the VirtualHost for phpMyadmin.

```html
nano /etc/sysconfig/iptables
```

<p>
Add the command line :
</p>
```html
-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
```
<p>
The same applies for IP6 firewall :
</p>
```html
nano /etc/sysconfig/ip6tables
```
<p>
Add the command line :
</p>
```html
-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
```
<p>
Restarting firewall service to allow the new port.
</p>
```html
/etc/init.d/iptables restart
/etc/init.d/ip6tables restart
```
<p>
Editing the VirtualHost file to run phpMyadmin on the port 8080
</p>
```html
nano /etc/httpd/conf/httpd.conf
```
<p>
Change the file content :
</p>
```html
<VirtualHost *:8080>
    DocumentRoot /usr/share/phpmyadmin/
    ServerName your_domain.com
</VirtualHost>
```
<p>
Next, add the command to allows listening on the port 8080 in the file "httpd.conf"
</p>
```html
nano /etc/httpd/conf/httpd.conf
```
<p>
Add the command line :
</p>
```html
Listen 8080
```
<p>
Save the file and exit, restarting the Apache service :
</p>
```html
service httpd restart
```
<p>
Now, phpMyadmin will run on the port 8080 at the address :
</p>
```html
http://your-domain:8080
```

#### Install Ruby
<p>
Ruby is a object-oriented programming language, capable of reflection. Syntax inherited from Ada and Perl with object-oriented features of Smalltalk, and also share some features with Python, Lisp, Dylan and CLU, Ruby is a single phase interpreter.

Ruby provides programming patterns, including functional programming, object-oriented, imperative, reflective, it uses dynamic variable and automatic memory management.

Install Ruby interpreter with version management program RVM.
</p>
```html
curl -L https://get.rvm.io | bash
```
### Install Rubygems
<p>
Rubygems is a Ruby's packages management program, very popular in applications written by Ruby language and the Ruby On Rails framework.
</p>
```html
yum -y install rubygems
```
#### Install Passenger
<p>
The full name of the Passenger is Phusion Passenger, known as mod_rails or mod_rack, it is a web application intergrate with Apache and it can operate as a standalone web server support for the Ruby On Rails applications.

Execute the following commands :
</p>
```html
gem install passenger
passenger-install-apache2-module
```
<p>
After completed, we copy a notification block in the window to create the configuration file in the next steps (select block notification and press C to copy).
</p>
```html
 LoadModule passenger_module /usr/local/rvm/gems/ruby-1.9.3-p551/gems/passenger-5.0.15/buildout/apache2/mod_passenger.so
   <IfModule mod_passenger.c>
     PassengerRoot /usr/local/rvm/gems/ruby-1.9.3-p551/gems/passenger-5.0.15
     PassengerDefaultRuby /usr/local/rvm/gems/ruby-1.9.3-p551/wrappers/ruby
   </IfModule>
```
<p>
Create a new virtual host file for Passenger :
</p>
```html
nano /etc/httpd/conf.d/passenger.conf
```
<p>
Paste the command blocks into the empty file and save it, then restart the Apache service.
</p>
```html
service httpd restart
```
<p>
Create Database for Redmine

Use MySQLAdmin to create an empty database for Redmine, saved password to fill in the configuration file in the next steps.
</p>
```html
mysql --user=root --password=root_password_mysql
create database redmine_db character set utf8;
create user 'redmine_admin'@'localhost' identified by 'your_new_password';
grant all privileges on redmine_db.* to 'redmine_admin'@'localhost';
quit;
```
#### Install Redmine
<p>
Redmine is a main program of the project management system, we will download and install the program from the website of Redmine.

Download Redmine version 2.6.x to directory "/var/www" on the Centos OS.
</p>
```html
cd /var/www
wget http://www.redmine.org/releases/redmine-2.6.6.tar.gz
```
<p>
Extract the folder and rename directory
</p>
```html
tar xvfz redmine-2.6.6.tar.gz
mv redmine-2.6.6 redmine
rm -rf redmine-2.6.6.tar.gz
```

#### Configuring the Database
<p>
The next, we need to configure the database was created from the above steps.
</p>
```html
cd /var/www/redmine/config
cp database.yml.example database.yml
nano database.yml
```
<p>
Enter name for database, enter username and password of the database. Press CTRL + O to save the file and CTRL + X to exit.
</p>
#### Setting up Rails
<p>
Install the package library support for Rails using the Bundle.
</p>
```html
cd /var/www/redmine
gem install bundler
bundle install
rake generate_secret_token
```
<p>
The next, we create the database table for the Redmine application.
</p>
```html
RAILS_ENV=production rake db:migrate
RAILS_ENV=production rake redmine:load_default_data
```
#### Activate FCGI

```html
cd /var/www/redmine/public
mkdir plugin_assets
cp dispatch.fcgi.example dispatch.fcgi
cp htaccess.fcgi.example .htaccess
```

#### Setting up Apache and FastCGI

```html
cd /var/www/
rpm --import https://fedoraproject.org/static/0608B895.txt
wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
rpm -ivh epel-release-6-8.noarch.rpm
yum -y install mod_fcgid
rm -rf epel-release-6-8.noarch.rpm
```

#### Creating Files Directory
<p>
This directory contains data files generated during the operation of Redmine as document or image file, we create a new directory in the "/opt".
</p>

```html
mkdir -p /opt/redmine/files
chown -R apache:apache /opt/redmine
cd /var/www/redmine/config
cp configuration.yml.example configuration.yml
nano configuration.yml
```

<p>
Enter the directory path containing the data files you just created in the previous step into the line "attachments_storage_path".
</p>
*Note:* You must add a space at the begin of the path "/opt/redmine/files" after character ":"
#### Configuring Email
<p>
Another very important function of Redmine is using email to notify members when the contents of each project changes, Redmine can use many different methods to send email that is Sendmail, SMTP, GMail ...

To configure the email we will edit the configuration file.
</p>

```html
nano /var/www/redmine/config/configuration.yml
```

<p>
The simplest is you use features of the default SendMail in the Centos OS by settings :
</p>

```html
  email_delivery:
   delivery_method: :sendmail
```

*Note :* Do not use the Tab key to indent when editing the configuration file, you need to use the space bar on the keyboard.
<p>
If you use GMail's SMTP, you need to register an email account with the login methods used password normal and disable two-step authentication by smart phone.
</p>
<p>
Enter your Gmail account as below :
</p>

```html
  email_delivery:
   delivery_method: :smtp
   smtp_settings:
        enable_starttls_auto: true
        address: "smtp.gmail.com" 
        port: 587
        domain: "smtp.gmail.com" 
        authentication: :plain
        user_name: "your_email@gmail.com" 
        password: "your_password" 
```

<p>Save and exit</p>
#### Configuring SMTP
<p>
    Configure sendmail on the machine so that it can send emails. In this particular case sendmail is configured to use mail.foo.com as SMART_HOST for sending emails outside.
</p>

```html

cp {REDMINE_ROOT}/config/configuration.yml.example {REDMINE_ROOT}/config/configuration.yml

```

<p>
    Edit the config/configuration.yml file and near the end where production: is present use:
</p>

```html
            production:
               email_delivery:
                 delivery_method: :sendmail
```

-    Login as administrator in redmine and go to "Administration -> Settings -> Email notifications" and choose appropriate defaults. In current configuration settings are:
-        Emission email address:redmine@issues.foo.com
-        Blind carbon copy for receipients
-        Default notification: Only for things I am involved in
-        Email notifications should be sent for issue added or issue updated (note, status, priority) 
<p> </p>
#### LDAP authentication

- Login as administrator
- Navigate to Administration > LDAP authentication
- Click on "New authentication mode"
- Enter the below values in corresponding fields

```html
        Host: ldap.virtual-labs.ac.in 
        Port: 389 
        Base DN: dc=virtual-labs,dc=ac,dc=in 
        Login: uid 
        "On-the-fly user creation" checkbox 
```

-    Save and Test the connection
-    Login with an LDAP account
-    Provide the additional details when prompted (name, email, etc.)
-    Logout and login as administrator
-    Navigate to Administration > Users
-    Click on the user name used in last step
-    Go to the Projects tab
-    Choose a project name and select a role
-   Click on Add to give the LDAP user access to the redmine project 

#### Create Virtual Host for Redmine

Create an Apache configuration file for the Redmine application at the port 80.

nano /etc/httpd/conf.d/redmine.conf

Copy the text below and paste into the editor window, note the information to change your domain name.

```html

<VirtualHost *:80>
        ServerName your_domain
        ServerAdmin your_domain@domain.com
        DocumentRoot /var/www/redmine/public/
        ErrorLog logs/redmine_error_log
        <Directory "/var/www/redmine/public/">
                Options Indexes ExecCGI FollowSymLinks
                Order allow,deny
                Allow from all
                AllowOverride all
        </Directory>
</VirtualHost>

```

Save the file configuration and exit.
#### Running Redmine

Before execute Redmine in the first time, we must permission for the directory installed Redmine and restart Apache service.

```html

cd /var/www
chown -R apache:apache redmine
chmod -R 755 redmine
service httpd restart
```
Redmine will run at the following address URL :

```html

http://your-domain

```

Login to system with an administrator account : admin / admin

You can change your password after successful login.

We can see Redmine has running but very primitive, in the next steps we will install the support plugins and customized Redmine to use professional.
