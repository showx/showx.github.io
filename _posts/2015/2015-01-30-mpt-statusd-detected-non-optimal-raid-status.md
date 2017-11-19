---
layout: post
title: MPT-STATUSD: DETECTED NON-OPTIMAL RAID STATUS
date: 2015-01-30 17:26
author: admin
comments: true
categories: []
---
I have noticed that mpt-status gets installed by default in Debian 7 Wheezy when running on VMware. Since the virtual machine does not use RAID mpt-statusd reports “non-optimal” RAID status in the log every 10 minutes.
<pre>mpt-statusd: detected non-optimal RAID status</pre>
The mpt-status package is used to query the status of LSI SCSI HBAs so unless your machine is using such HBA cards the mpt-status package should be safe to remove.
<pre>sudo service mpt-statusd stop sudo apt-get purge mpt-status</pre>
