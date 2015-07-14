---
layout: post
title: Controlling your Dell laptop fan speed using i8kmon
---
<p class="message">
Generally most of the Dell laptops with ubuntu installed have problem with temperature issues.
</p>

To overcome the issue, there is package i8kmon.

* sudo apt-get install i8kmon

configure your values in /usr/bin/i8kmon

change the Default array set config to

```html

    array set config {
       sysconfig   /etc/i8kmon.conf
       userconfig  ~/.i8kmon
       i8kfan      /usr/bin/i8kfan
       acpi    "acpi"
       geometry    {}
       use_conf    1
       auto        1
       daemon      1
       verbose     0
       timeout     5
       unit        C
       t_high      80
       min_speed   2000
       0           {\{0 0}  -1  45  -1  45}
       1           {\{1 1}  40  60  45  65}
       2           {\{1 1}  50  70  55  75}
       3           {\{2 2}  60 128  65 128}
    }

    Description: generally a laptop temperature varies between 40-50 degrees.     I have set 45 -50 a level so that whenever the temperature crosses 45 it     activates fan and exhausts out the heat. Whenever the temperature goes     down 45, the fan automatically stops. playing between 45 and 50.

    Note: Dell laptops comes with only one fan so we can change the config to

    If we have only right fan in our laptop use "-" for left to notify only     give command for right fan to work

       0           {\{- 0}  -1  45  -1  45}
       1           {\{- 1}  40  60  45  65}
       2           {\{- 1}  50  70  55  75}
       3           {\{- 2}  60 128  65 128}
    To check your temperature and fan status use i8kctl

    A sample watch of i8kctl of my laptop

    $ i8kctl
    1.0 A11 78QGMV1 46 -1 1 -1 84000 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 43 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 43 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 43 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 43 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 44 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 45 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 45 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 46 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 46 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 46 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 46 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 46 -1 1 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 47 -1 1 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 44 -1 1 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 44 -1 1 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 44 -1 1 -1 84000 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 44 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 44 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 44 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 44 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 45 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 45 -1 0 -1 0 -1 -1
    $ i8kctl
    1.0 A11 78QGMV1 45 -1 0 -1 0 -1 -1
    1.0 A11 78QGMV1 45 -1 0 -1 0 -1 -1
    $ i8kctl
    $ i8kctl
    1.0 A11 78QGMV1 45 -1 0 -1 0 -1 -1

```
