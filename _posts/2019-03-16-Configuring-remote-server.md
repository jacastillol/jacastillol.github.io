---
title: "Vagrant for LAMP learning"
date: 2019-03-15
tags: [linux, servers, LAMP]
header:
  image: "/images/fort point.png"
excerpt: "Linux, Servers, LAMP"
mathjax: "true"
---

# Configuring Ubuntu, Apache ...

## First step: 
Create a new user with sudo priviledges, and give a password.
```bash
root@hostname~# adduser username
``` 
Add the new user `username` as a root user by adding to `sudo` group,
```bash
root@hostname~# usermod -a -G sudo username
root@hostname~# exit
``` 
## Second step:
If you change your IP remote server, it is possible that `ssh` fail login by the hostname then you can user `ssh-keygen -R hostname`.
If the problem continue you must see in your VPS account the configuration details.

with the hostname
```bash
user@atwork~$ ssh username@hostname
``` 
or without
```bash
user@atwork~$ ssh username@ip-hostname
``` 
### Setting Apache and Firewall
After this step you can chech your server from a browser with http://hostname
```bash
username@hostname~$ sudo apt update
username@hostname~$ sudo apt upgrade
username@hostname~$ sudo apt install -y apache2
optional~$ sudo systemctl status apache2
optional~$ sudo ufw allow 80/tcp
optional~$ sudo ufw allow 443/tcp
optional~$ sudo ufw reload
``` 
### Setting PHP
With this step you can probe the following php file: `<?php infophp(); ?>` with the corresponding configurations.
```bash
username@hostname~$ sudo apt install -y php libapache2-mod-php
``` 
### Setting a SQL server
Install and configure a SQL server
#### MySQL
**N** and then **Y** to retain a simple password
```bash
username@hostname~$ sudo apt install -y mysql-server
optional~$ sudo systemctl status mysql
username@hostname~$ sudo mysql_secure_installation
``` 
#### MariaDB
```bash
username@hostname~$ sudo apt install -y mariadb-server mariadb-client
optional~$ sudo systemctl status mysql
username@hostname~$ sudo mysql_secure_installation
``` 
#### Removing MySQL
If you have to remove MySQL
```bash
sudo apt remove --purge mysql*
sudo apt remove --purge mysql-*
sudo apt autoremove
sudo apt remove dbconfig-mysql
``` 
