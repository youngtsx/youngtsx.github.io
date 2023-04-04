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
- A second server to act as the CA. Follow the same [Inital Server Set Up](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04) and [steps 1-3](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-a-certificate-authority-ca-on-ubuntu-20-04)
    - CA should only be on during use of authenticating, making, or revoking client certificates. It should be off when not in use to prevent it from being compromised. 

### Configuring the VPN server
We first need a sudo non root user
- sudo adduser tiffany
- sudo usermod -aG sudo tiffany

#### Installing OpenVPN and Easy RSA

- sudo apt update
- sudo apt install openvpn easy-rsa
- mkdir ~/easy-rsa
    - make an easy-rsa directory
- ln -s /usr/share/easy-rsa/* ~/easy-rsa/
    - create a symlink from the easyrsa script that was installed to the directory
-sudo chown sammy ~/easy-rsa
-chmod 700 ~/easy-rsa
    - ensure the directory’s owner is your non-root sudo user and restrict access to that user

#### Creating a PKI 

- cd ~/easy-rsa
- nano vars
    - in the file, paste:
    > set_var EASYRSA_ALGO "ec"
    > set_var EASYRSA_DIGEST "sha512"




sudo apt install openssh-server openssh-client

- $ sudo vim /etc/ssh/sshd_config
    - Edit the file to change the port from 22 to 41235 or whatever port you want
- enable pubkey authentication

- mkdir ~/.ssh

- vim ~/.ssh/authorized_keys
    - append key created during ssh-keygen
    - cat /etc/ssh/id_rsa.pub
    - 

- ssh -p 41235 tiffany@147.182.232.139






