---
layout: project
type: project
title: Docker Install
date: 2023-04-8
labels:
  - ITM 684
  - Linux
  - VPN
---
### Create a digital ocean server with a non-root user with sudo permissions

Generate a droplet
- ubuntu 22.04 LTS x64; SFO3; 0.04/month
- adduser tiffany
- sudo usermod -aG sudo tiffany

[install docker](https://thematrix.dev/install-docker-and-docker-compose-on-ubuntu-20-04/)
- follow this and install docker


![](dockerimages/docker_compose_yml.png)

- copy and paste 
- change serverlurl to DO server ip
- edit the peers as needed

add tunnel to wireguard
- sftp the .conf file to client machine
- locate the file on wireguard tunnel