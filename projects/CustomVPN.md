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

Generate a droplet
- ubuntu 22.04 LTS x64; SFO3; 0.004/month
- adduser tiffany
- sudo usermod -aG sudo tiffany

- A second server to act as the CA. Follow the same [Inital Server Set Up](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04) and [steps 1-3](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-a-certificate-authority-ca-on-ubuntu-20-04)
    - CA should only be on during use of authenticating, making, or revoking client certificates. It should be off when not in use to prevent it from being compromised. 

Generate another droplet for CA 
Step 1
- ubuntu 22.04 LTS x64; SFO3; 0.004/month
- adduser tiffany
- sudo usermod -aG sudo tiffany
Step 2
- sudo apt update
- sudo apt install easy-rsa 
Step 3 - PKI directory
- mkdir ~/easy-rsa
- ln -s /usr/share/easy-rsa/* ~/easy-rsa/
    - symlinks 
- chmod 700 /home/sammy/easy-rsa
    - change permissions only for owner
- cd ~/easy-rsa
- ./easyrsa init-pki
    - initialize PKI inside the directory
Step 4
- cd ~/easy-rsa
- nano vars
> ~/easy-rsa/vars
set_var EASYRSA_REQ_COUNTRY    "US"
set_var EASYRSA_REQ_PROVINCE   "NewYork"
set_var EASYRSA_REQ_CITY       "New York City"
set_var EASYRSA_REQ_ORG        "DigitalOcean"
set_var EASYRSA_REQ_EMAIL      "admin@example.com"
set_var EASYRSA_REQ_OU         "Community"
set_var EASYRSA_ALGO           "ec"
set_var EASYRSA_DIGEST         "sha512"
### Installing OpenVPN and Easy RSA
- sudo apt update
- sudo apt install openvpn easy-rsa
- mkdir ~/easy-rsa
    - make an easy-rsa directory
- ln -s /usr/share/easy-rsa/* ~/easy-rsa/
    - create a symlink from the easyrsa script that was installed to the directory
-sudo chown sammy ~/easy-rsa
-chmod 700 ~/easy-rsa
    - ensure the directory’s owner is your non-root sudo user and restrict access to that user

### Creating a PKI 

- cd ~/easy-rsa
- nano vars
    - in the file, paste:
    > set_var EASYRSA_ALGO "ec"
    > set_var EASYRSA_DIGEST "sha512"
    - this server is not the CA so these two lines is all.



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






