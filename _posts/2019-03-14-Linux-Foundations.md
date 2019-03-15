---
title: "Linux Foundations"
date: 2019-03-14
tags: [linux, servers, programming]
header:
  image: "/images/fort point.png"
excerpt: "Linux, Servers, LAMP, Programming"
mathjax: "true"
---

# Towards a Linux Web Sever

## Requirements and setting tools
For this configuration you will need `virtualbox` and `vagrant` installed on your Linux box. Depending on the VirtualBox you most choose between

```bash
$ sudo apt install vagrant
```
For versions greater than 5.2 of VirtualBox, and if you have problems with ruby's gems then do the following:
```bash
$ sudo dpkg -i vagrant*.deb
```
Create a virtual box, remember to select VirtualBox as the option
```bash
$ vagrant box add generic/ubuntu1804
$ mkdir ~/ubuntu1804
$ cd ~/ubuntu1804
$ nano VagrantFile
```
and edit the folowing VagrantFile
```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'

Vagrant.configure("2") do |config|
  ##### DEFINE VM #####
  config.vm.define "ubuntu-01" do |config|
  config.vm.hostname = "ubuntu-01"
  config.vm.box = "generic/ubuntu1804"
  config.vm.box_check_update = false
  config.vm.network "private_network", ip: "192.168.2.10"
  end
end
```
Choose a valid IP and now you can work from `ssh`
```bash
$ vagrant up
$ vagrant ssh
```

## Add a new user `student` and using ssh with a key
```bash
vagrant-$ sudo adduser student
vagrant-$ sudo usermod -a -G sudo student
```
now student  have a better access
```bash
$ ssh student@127.0.0.1 -p 2222
```
You can force the student user to change the password for a better security
```bash
vagrant-$ sudo passwd -e student
```
but is better if you create a pair of keys with student user.
Student in the local must create a key with ssh-keygen
```bash
$ ssh-keygen
```
This command generates two files `id_rsa` and `id_rsa.pub` which use the RSA encryption algorithm. Now on the server system you must copy only the public key present inside the `id_rsa.pub` file. Next, in a new file inside the home directory of the `student` user save the key as a hidden file named  `.ssh/authorized_keys`.
```bash
student-$ mkdir .ssh
student-$ touch .ssh/authorized_keys
student-$ nano .ssh/authorized_keys
```
Furthermore the right permissions on the .ssh directory content must be set up.
```bash
student-$ chmod 700 .ssh
student-$ chmod 644 .ssh/authorized_keys
```
And Finally from a remote `ssh` the `student` user can access the server securely just having the right `id_rsa` file.
```bash
$ ssh student@127.0.0.1 -p 2200 -i ~/.ssh/id_rsa
```
### Forcing Key based authentication
In the file `/etc/ssh/sshd_config` line `PasswordAuthentication` switch `yes` to `no`, and restart ssh service
```bash
student-$ sudo nano /etc/ssh/sshd_config
student-$ sudo service ssh restart
```

## Set up firewall

## Configure :8080 port 
Or other for the virtual and host machine
```bash
student-$ sudo apt update
student-$ sudo apt install apache2
```
In the host of the virtual you can access http://localhost:8080, to see the Apache2 Ubuntu Default page. 
