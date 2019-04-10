---
title: "Initial configuration for the Raspberry Pi and RFID throw SPI"
date: 2019-03-14
tags: [python, iot]
header:
  image: "/images/fort point.png"
excerpt: "Linux, Debian, Raspberry Pi, Python, IoT"
mathjax: "true"
---

# First Configuration for the Raspberry Pi

## Installing Raspian
1. Download from (here)[https://www.raspberrypi.org/downloads/raspbian/] the lastest version
1. You can use (Etcher)[https://www.balena.io/etcher/] to create a flash card or a USB.
1. Insert flash card on your Raspberry, and follow the instructions

## Setting the SPI module for devices
1. Select the __The fifth option: Interfacing options__ from the following command:
    ```bash
    $ sudo raspi-config
    ```
    and then enable __P4 SPI__ option, exit the `raspi-config` with `esc`. 
1. Reboot the system and check the SPI module is active with
    ```bash
    $ sudo reboot
    $ lsmod | grep spi
    ```
    you will see a line with `spi_bcm2835`.
If you have troubles see (here)[https://pimylifeup.com/raspberry-pi-rfid-rc522/].

## Setting Python 3
```bash
$ sudo apt update
$ sudo apt upgrade
$ sudo apt install python3-dev python3-pip
$ sudo pip3 install spidev
$ sudo pip3 install mfrc522
```

## Setting remote access from SSH for Raspberry Pi
1. Select __Interfacing Options__ from the following command and then enable __SSH__
    ```bash
    $ sudo raspi-config
    ```
    you can alternatively,
    ```bash
    $ sudo systemctl enable ssh
    $ sudo systemctl start ssh
    ```
    Remember to change the password for a remote access.


