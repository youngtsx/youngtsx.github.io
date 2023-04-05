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
- One Ubuntu server with a sudo non-root user and a firewall enabled. [Initial Server Set Up](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04)

Generate a droplet
- ubuntu 22.04 LTS x64; SFO3; 0.04/month
- adduser tiffany
- sudo usermod -aG sudo tiffany

sudo apt install openssh-server openssh-client

- $ sudo vim /etc/ssh/sshd_config
    - Edit the file to change the port from 22 to 41235 or whatever port you want
- enable pubkey authentication

- mkdir ~/.ssh

- vim ~/.ssh/authorized_keys
    - append key created during ssh-keygen
    - cat /etc/ssh/id_rsa.pub

- ssh -p 41235 name@ip

- A second server to act as the CA. Follow the same [Initial Server Set Up](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04) and [steps 1-3](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-a-certificate-authority-ca-on-ubuntu-20-04)
    - CA should only be on during use of authenticating, making, or revoking client certificates. It should be off when not in use to prevent it from being compromised. 

Generate another droplet for CA 

Step 1
- ubuntu 22.04 LTS x64; SFO3; 0.04/month
- adduser tiffany
- sudo usermod -aG sudo tiffany

Step 2
- sudo apt update
- sudo apt install easy-rsa 

Step 3 - PKI directory
- mkdir ~/easy-rsa
- ln -s /usr/share/easy-rsa/* ~/easy-rsa/
    - symlinks 
- chmod 700 /home/tiffany/easy-rsa
    - change permissions only for owner
- cd ~/easy-rsa
- ./easyrsa init-pki
    - initialize PKI inside the directory

Step 4
- cd ~/easy-rsa
- nano vars

> ~/easy-rsa/vars

> set_var EASYRSA_REQ_COUNTRY    "US"

> set_var EASYRSA_REQ_PROVINCE   "Hawaii"

> set_var EASYRSA_REQ_CITY       "Honolulu"

> set_var EASYRSA_REQ_ORG        "DigitalOcean"

> set_var EASYRSA_REQ_EMAIL      "tyoung24@hawaii.edu"

> set_var EASYRSA_REQ_OU         "Community"

> set_var EASYRSA_ALGO           "ec"

> set_var EASYRSA_DIGEST         "sha512"

- ./easyrsa build-ca
    - enter password

Step 5 - Distributing your Certificate Authority’s Public Certificate
- cat ~/easy-rsa/pki/ca.crt
    - copy the output
- nano /tmp/ca.crt
    - paste 
- sudo cp /tmp/ca.crt /usr/local/share/ca-certificates/
- sudo update-ca-certificates
    - these import the certificates 

#### Public Keys

sudo apt install openssh-server openssh-client

- $ sudo vim /etc/ssh/sshd_config
    - Edit the file to change the port from 22 to 41235 or whatever port you want
- enable pubkey authentication

- mkdir ~/.ssh

- vim ~/.ssh/authorized_keys
    - append key created during ssh-keygen
    - cat /etc/ssh/id_rsa.pub

## OpenVPN 
### Installing OpenVPN and Easy RSA
- sudo apt update
- sudo apt install openvpn easy-rsa
- mkdir ~/easy-rsa
    - make an easy-rsa directory
- ln -s /usr/share/easy-rsa/* ~/easy-rsa/
    - create a symlink from the easyrsa script that was installed to the directory
-sudo chown tiffany ~/easy-rsa
-chmod 700 ~/easy-rsa
    - ensure the directory’s owner is your non-root sudo user and restrict access to that user

### Creating a PKI 

- cd ~/easy-rsa
- nano vars
    - in the file, paste:

    > set_var EASYRSA_ALGO "ec"

    > set_var EASYRSA_DIGEST "sha512"

    - this server is not the CA so these two lines is all.
- ./easyrsa init-pki

### Create a certificate request and private key
- cd ~/easy-rsa
- ./easyrsa gen-req server nopass
    - name the server, can leave blank
- sudo cp /home/tiffany/easy-rsa/pki/private/server.key /etc/openvpn/server/

### Signing the certificate
- scp /home/tiffany/easy-rsa/pki/reqs/server.req tiffany@your_ca_server_ip:/tmp
    - ensure the CA has vpn server's public key (ssh in the prereqs)

On CA server
- cd ~/easy-rsa
- ./easyrsa import-req /tmp/server.req server
    - this imports the server from the secure copy
- ./easyrsa sign-req server server
    - it will ask if this is from a trusted source, say yes and confirm
- scp pki/issued/server.crt tiffany@your_vpn_server_ip:/tmp
- scp pki/ca.crt tiffany@your_vpn_server_ip:/tmp
    - make sure vpn server has the CA server's public key

On VPN server
- sudo cp /tmp/{server.crt,ca.crt} /etc/openvpn/server
    copy the files from /tmp

### Configuring OpenVPN cryptographic material
- cd ~/easy-rsa
- openvpn --genkey secret ta.key
- sudo cp ta.key /etc/openvpn/server

### Generate client certificate and key pair
- mkdir -p ~/client-configs/keys
- chmod -R 700 ~/client-configs
- cd ~/easy-rsa
- ./easyrsa gen-req client1 nopass
    - create key
- cp pki/private/client1.key ~/client-configs/keys/
    - copy the key
- scp pki/reqs/client1.req tiffany@your_ca_server_ip:/tmp
    - transfer to the CA server

On CA server
- cd ~/easy-rsa
- ./easyrsa import-req /tmp/client1.req client1
- ./easyrsa sign-req client client1
    - prompted to sign
- scp pki/issued/client1.crt tiffany@your_server_ip:/tmp
    - copy the client certificate back to VPN server

On VPN Server
- cp /tmp/client1.crt ~/client-configs/keys/
    - copy the certificate to the client-configs/keys/ directory
- cp ~/easy-rsa/ta.key ~/client-configs/keys/
- sudo cp /etc/openvpn/server/ca.crt ~/client-configs/keys/
- sudo chown tiffany.tiffany ~/client-configs/keys/*

### Configure OpenVPN
- sudo cp /usr/share/doc/openvpn/examples/sample-config-files/server.conf.gz /etc/openvpn/server/
- sudo gunzip /etc/openvpn/server/server.conf.gz
- sudo nano /etc/openvpn/server/server.conf

`;`tls-auth ta.key 0 # This file is secret

`tls-crypt ta.key`







