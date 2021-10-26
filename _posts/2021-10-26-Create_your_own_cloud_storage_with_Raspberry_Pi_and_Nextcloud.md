---
layout: post
title: "Build your own personal cloud storage"
subtitle: 'Build your own personal cloud storage with Nextcloud and Raspberry Pi'
date:       2021-10-26
author: "Shadly"
category: Tutorial
categories: ['Tutorial', 'Nextcloud', 'Raspberry Pi']
header-img: "img/raspberry.jpg"
catalog: true
tags:
  - Nextcloud
  - Tutorial
  - Raspberry Pi
---

## What is Nextcloud
Nextcloud is a suite of client-server software for creating cloud file storage. It is basically a free and open source version of google drive. It offers some really nice features. After Google announced that Google Photos will no longer allow unlimited photo storage at high-quality from June 1, 2021, I decided to give Nextcloud a try. Here I will be sharing my experience on how I built my own personal cloud storage with Nextcloud and Raspberry Pi 3 Model B. 

## Right Image file for Raspberry Pi 3 Model B
Nextcloud have a nice project called NextcloudPi for Raspberry Pi devices. At first I tried their official images from [here](https://ownyourbits.com/downloads/) . But somehow they didn't boot on my Raspberry Pi 3 Model B. So grabbed the source code from [here](https://github.com/nextcloud/nextcloudpi) and built the img file for my device. 

## Build NextcloudPi for Raspberry Pi 3 Model B
``` bash
sudo apt-get update
git clone https://github.com/nextcloud/nextcloudpi.git
cd nextcloudpi
build/build-SD-rpi.sh
```
After the build is finished you will find the img file in the tmp folder. Write the image in your SD card. 

## Setting up Nextcloud on Raspberry Pi
To connect to wifi, enable SSH and do other setting just type in 
``` bash
sudo raspi-config
```
To install Nextcloud :
``` bash
curl -sSL https://raw.githubusercontent.com/nextcloud/nextcloudpi/master/install.sh | sudo bash
```
This will take some time to download and install everything. 
After it is installed get the IP of your Raspberry Pi device and check if the IP is accessible from your local network. 
``` bash
ifconfig
```
## Access through untrusted domain

![Untrusted Domain](/blog/img/untrusted_domain.png)

After you reach your device form your browser you will most likely get this error. To get through this, you need to add trusted domain in your config.php file. Edit this file and add your device's IP to the trusted domain list. 
``` bash
sudo cd ../../var/www/nextcloud/config
nano config.php
```
Now you can reach your Nextcloud. 

## Changing default password of the Nextcloud

There is a nice text based interface tool for configuring NextcloudPi. 
``` bash
sudo ncp-config
```
Here you can enable the web interface if it is not already enabled. Change the password for the default user 'ncp'. Now try to login to your cloud. If everything works fine you should be now able to login to your Nextcloud. 

## Add your External USB Storage

``` bash
sudo mkdir /media/USBdrive
```
Now you have to run the wizard from 

``` bash
https://your_raspi_ip:4443/wizard/
```
Navigate through the options and setup your external storage. 

## Enjoy Nextcloud 

Now enjoy your free unlimited cloud storage.

![Nextcloud](/blog/img/nextcloud.png)
