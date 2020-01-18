---
layout: post
title: "Mosquitto in Docker"
subtitle: 'Run MQTT Broker in Docker'
date:       2020-01-18
author: "Shadly"
category: Library
categories: ['Tutorial', 'Docker']
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
  - Docker
  - MQTT
  - Tutorial
  - Mosquitto
  - Broker
---

## What is Docker?
> Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package. By doing so, thanks to the container, the developer can rest assured that the application will run on any other Linux machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code.
> 
> In a way, Docker is a bit like a virtual machine. But unlike a virtual machine, rather than creating a whole virtual operating system, Docker allows applications to use the same Linux kernel as the system that they're running on and only requires applications be shipped with things not already running on the host computer. This gives a significant performance boost and reduces the size of the application.
> 
> -- <cite>[https://opensource.com/resources/what-docker](https://opensource.com/resources/what-docker)</cite>

## What is MQTT?
> MQTT stands for MQ Telemetry Transport. It is a publish/subscribe, extremely simple and lightweight messaging protocol, designed for constrained devices and low-bandwidth, high-latency or unreliable networks. The design principles are to minimise network bandwidth and device resource requirements whilst also attempting to ensure reliability and some degree of assurance of delivery. These principles also turn out to make the protocol ideal of the emerging “machine-to-machine” (M2M) or “Internet of Things” world of connected devices, and for mobile applications where bandwidth and battery power are at a premium.
> 
> -- <cite>[http://mqtt.org/faq](http://mqtt.org/faq)</cite>

To learn more about MQTT, you can checkout [this](https://www.hivemq.com/mqtt-essentials/) nice blog from HiveMQ.

## MQTT Basics (HiveMQ)

-   [Part 1: Introducing MQTT](https://www.hivemq.com/blog/mqtt-essentials-part-1-introducing-mqtt/)
-   [Part 2: Publish & Subscribe Basics](https://www.hivemq.com/blog/mqtt-essentials-part2-publish-subscribe/)
-   [Part 3: Client, Broker and Connection Establishment](https://www.hivemq.com/blog/mqtt-essentials-part-3-client-broker-connection-establishment/)
-   [Part 4: Publish, Subscribe & Unsubscribe](https://www.hivemq.com/blog/mqtt-essentials-part-4-mqtt-publish-subscribe-unsubscribe/)
-   [Part 5: Topics & Best Practices](https://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices/)

## What is MQTT Broker?
> An **MQTT broker** is a **server** that receives all messages from the clients and then routes the messages to the appropriate destination clients. An **MQTT** client is any device (from a micro controller up to a full-fledged **server**) that runs an **MQTT** library and connects to an **MQTT broker** over a network.
> 
> --<cite>[https://en.wikipedia.org/wiki/MQTT](https://en.wikipedia.org/wiki/MQTT)</cite>

## What is Eclipse Mosquitto?
> Eclipse Mosquitto is an open source (EPL/EDL licensed) message broker that implements the MQTT protocol versions 5.0, 3.1.1 and 3.1. Mosquitto is lightweight and is suitable for use on all devices from low power single board computers to full servers.
> 
> --<cite>[https://mosquitto.org/](https://mosquitto.org/)</cite>

## Create docker container with mosquitto
### Using Command Line :
Pull official Eclipse Mosquitto image from using the following command
 ```bash
 docker pull eclipse-mosquitto
 ```

Create a folder named mosquitto. Inside this folder, create another folder named config. Now create a file here with name mosquitto.conf. This is the configuration file for the broker. Put the below configuration in mosquitto.conf file and save.
```
persistence true 
persistence_location /mosquitto/data/ 
log_dest file /mosquitto/log/mosquitto.log
```
Now run the following command to create and run the container named "mosquitto". Do not forget to replace *"Your Full Path"* with your directory.
```bash
docker run --name=mosquitto --restart=always  -p 1883:1883 -p 9001:9001 -v /Your Full Path/mosquitto/:/mosquitto/ -d eclipse-mosquitto
```

Now your docker container with mosquitto should be up and running !

### Using Dockerfile : 

Create a folder named src. Now create a file here with name mosquitto.conf. This is the configuration file for the broker. Put the below configuration in mosquitto.conf file and save.
```
persistence true 
persistence_location /mosquitto/data/ 
log_dest file /mosquitto/log/mosquitto.log
```

Create a file named *"Dockerfile"* and paste the following lines
```
FROM eclipse-mosquitto
COPY src/mosquitto.conf ./mosquitto/config/
EXPOSE 1883
```
Now build the image with the following command
```bash
sudo docker build -t mosquitto .
```
Now run the container with the following command
```bash
sudo docker run -p 1883:1883 mosquitto
```

## Useful Docker Commands :
**docker run** – Runs a command in a new container  
**docker start** – Starts one or more stopped containers  
**docker stop** – Stops one or more running containers  
**docker build** – Builds an image form a Docker file  
**docker rm** – Removes an image  
**docker build** – Builds an image form a Docker file  
**docker container ls** - Lists running containers  

