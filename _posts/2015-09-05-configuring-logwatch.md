 Get a daily server overview via email with logwatch

Keeping up with activity on your Linux server can be a full-time job if you allow it to be (as well it should be, it can easily be argued).  There are many services that can ease the burden by providing you with a detailed overview of what’s going on at the moment (things like Monit, Munin, Cloudpassage, and a host of others).

Something that can augment this nicely is the 30,000-foot view that logwatch provides.

Logwatch is a series of Perl scripts packaged together that will detect, parse, and summarize a wide variety of log file types, including Apache, sshd, postfix, iptables, and a host of others.  Simply tell it who to email reports to, and schedule it to run at least once a day to receive a regular email digest of server activity you can quickly scan for issues.

To install, you can either grab the files via the project website at http://sourceforge.net/projects/logwatch/files/, or via your server’s package manager.  On flavors of Ubuntu, this would look like:

sudo apt-get install logwatch
yum install logwatch

Installation will also install a few required libraries if they’re not already on your server (libdate-manip-perl and libyaml-syck-perl).

At this point, you can run logwatch from the commandline and you’ll see some text output with information about what’s happened in the last day.  It’ll list out things like the what packages have been installed/removed, the 404s and 500s Apache has served, how much drive space is left, any segfaults detected, and what services like postfix and sshd have been up to.

To customize the configuration, you’ll want to edit the config file found in the slightly out-of-the-way location of

/usr/share/logwatch/default.conf/logwatch.conf

In here you’ll want to change the email address from root to your own email address.  Logwatch can send your email as text or HTML – select whichever you prefer.

Output = mail #change from stdout
Format = text
MailTo = you@example.com

There’s a detail level you can set as well – by default it’s set to the lowest level (0), which itself sends a fair amount of info.  If you find that you want to receive more, increase it as needed, up to 10.  It also supports text values of Low, Medium and High, which translate to 0, 5, and 10:

Detail = Low

By default, logwatch will recursively look for logs in /var/log, due to the configuration directive:

LogDir = /var/log

If you have additional logs in other locations, you can specify them by simply adding other LogDir lines:

LogDir = /var/log
LogDir = /var/www/example.com/logs/

Finally, schedule it by adding it to crontab – in this example, we schedule it to run at midnight by running crontab -e and adding the line:

0 0 * * * /usr/sbin/logwatch

…now sit back and wait for handy overviews to appear nightly in your inbox!

