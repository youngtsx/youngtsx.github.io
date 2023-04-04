---
layout: project
type: project
title: Custom VPN Walkthrough
date: 2023-04-16
labels:
  - ITM 684
  - Linux
---
# Custom VPN Server Installation

sudo apt install openssh-server openssh-client

##### > $ sudo vim /etc/ssh/sshd_config
Edit the file to change the port from 22 to 41235 or whatever port you want
