---
layout: post
title: 解决 ubuntu apt-get TAB 键不能自动补全[转]
date: 2017-09-09 02:22
author: admin
comments: true
categories: [Uncategorized]
---
Add Bash Completion In Debian

ash completion is a useful tool for completion of file paths, commands etc. By default it is enabled on Ubuntu but not on Debian. With two simple steps it can also be enabled on Debian.

1. Install bash-completion

First of all we need the install the according package:

apt-get install bash-completion

2. Add it to the bash profile

Either edit the ~/.bash_profile file to enable it only for a given user or edit /etc/profile to add it system-wide. Add the following code:
if [ -f /etc/bash_completion ]; then
. /etc/bash_completion
fi
3. Try it

In order for it to work you have to log out and relogin and then you can make use of bash completion the usual way. E.g. issue:
apt-g

and then press the TAB key once and the command will be completed to apt-get. Or issue this:
apt

and then press TAB key twice. You can also try with
apt-get install apa

and then press TAB key once to complete as far as possible and a second time to list all options.
