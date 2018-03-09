---
layout:     post
title:      Wifi issue with Realtek RTL8723be
date:       2018-03-10 01:58:00 - 5:30:00
summary:    Wifi issue with Realtek RTL8723be
categories: unix
thumbnail: jekyll
tags:
 - linux
 - wifi
 - realtek
---

# WIFI issues with Realtek and Manjaro Linux

### The issue

Wifi just doesn't work consistenly. Annoying stuff includes
* Not connecting to home wifi at all
* Connecting to office wifi, but working at snail's speed
* Connecting okay-ish to mobile wifi
* not detecting few other (non-major) wifi

### Details

```
[rudra@rudra-pc ~]$ inxi -N
Resuming in non X mode: xrandr not found. For package install advice run: inxi --recommends
Network:   Card-1: Realtek RTL8723BE PCIe Wireless Network Adapter driver: rtl8723be
           Card-2: Realtek RTL8101/2/6E PCI Express Fast/Gigabit Ethernet controller driver: r8169

```

### The solution

As mentioned [here](https://forum.manjaro.org/t/solved-wifi-problem-rtl8723be/34872/7),

> "HP seems to like to connect the wireless antenna to the aux connection, and not the primary."

* Create a file named `rtl8723.be.conf` in `etc/modprobe.d`
* Add the following to it:
 ```
 options rtl8723be fwlps=n ant_sel=2
 ```

