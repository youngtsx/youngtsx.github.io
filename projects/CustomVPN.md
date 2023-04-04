---
layout: project
type: project
title: Custom VPN Walkthrough
date: 2023-04-16
labels:
  - ITM 684
  - Linux
---
## Prerequisites 
- One Ubuntu server with a sudo non-root user and a firewall enabled. [Inital Server Set Up](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04)
-A second server to act as the CA. Follow the same [Inital Server Set Up](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04) and [`steps 1-3`](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-a-certificate-authority-ca-on-ubuntu-20-04)

sudo apt install openssh-server openssh-client

##### > $ sudo vim /etc/ssh/sshd_config
Edit the file to change the port from 22 to 41235 or whatever port you want
