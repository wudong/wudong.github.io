---
layout: post
title:  "A successful hackintosh build with Gigabyte GA-H87N-WIFI"
date:   2014-07-07 22:06:26
categories: "personal"
disqus: true
keywords:
- Hackintosh
- Gigabyte
- GA-H87N-WIFI
- Apple
excerpt: "How I build a hackintosh with Gigabyte GA-H87N-WIFI motherboard."
tags:
- personal
- hardware
- hackintosh
- Apple
---

The last time of me trying hackintosh was close to disaster. That was on a Asus
Z67 Sandy bridge motherboard with a i7-2600k CPU about a year ago, just couldn't
make it passing the dread white Apple logo.

Determined to make it work this time (partly because of the lagging I have
been feeling recently on my 2012 Macbook Pro 15), I did a quite a lot research
before commit to buying components. Based on various successful stories surfaced
on the Internet, I bought a Gigabyte GA-H77N-Wifi Ivy bridge motherboard to
work with my old Intel i7-2600k.

At the first attempt, I had some problem of installing Maverick into a
USB disk using [UniBeast](http://http://www.unibeast.com/);
it keeps saying installing failing at the very last step, and looking at the
installing log didn't reveal any obvious reason. After lots of searching and
head scratching, almost by accident, I found that it is the Mac OS's disk
partition type caused the problem. On my system it is set to
Mac OS journaled case sensitive, while UniBeast apparently can only work with
case insensitive partition.

Figured I cannot change the case-sensitivity of a partition easily, I had
to use my another Mac (which luckily have a case insensitive partition) to finish
the UniBeast process.

Installation using the UniBeast disk was smooth enough. But then, to my surprise,
it failed to reboot, still stopped at the dread white Apple logo. Adding '-x',
'-f' to the UniBeast boot options did not help; fiddling with BIOS settings did
not bring me anywhere either. This is a strange cause the GA-H77N-Wifi is
featured by many Hackintosh websites and is root of many successful build reported.

Then finally, after some millions google, I found an forum post mentioned that
there are some compatibility issued between Ivy Bridge and Sandy Bridge. If mixing
a Ivy Bridge CPU with a Sandy Bridge motherboard, or the vice verse, 
