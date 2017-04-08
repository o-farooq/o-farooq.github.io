---
layout: post
title: "Raspberry PI garage door opener for Merlin 230T using AWS IoT and AWS Lambda"
date: 2017-04-04 -0800
comments: true
tags: [iot, garage opener, merlin iot, garage iot, merlin 230T, door iot, aws garage opener]
---

Couple of days ago when I came back home I found out that I forgot to take the main door keys. And it took
more than 2 hours before my wife was back home and I could go in. while waiting for her to come I was wishing
if I could open my garage door via phone :) . So I decided to setup some basic stuff so that I can open my garage door
remotely

I have a Merlin 230T garage door opener, almost all the garage doors have manual open option. Did a simple test with a wire 
by connecting the manual endpoints. 
All I needed in hardware was a raspberry pi, a relay and some wire connectors.

Software solution is really simple, Raspberry has a node.js app running which listens to a topic hosted on AWS IoT. There is an API endpoint in AWS which triggers a 
lambda function which publishes to the same AWS IoT topic. Once a message is received in AWS IoT it notifies the node.js app and that
app then triggers the garage opening circuit to toggle the garage state

![AWS IoT Diagram]({{sie.url}}/images/20170408-merlin-garage-opener-iot.png)
 
Raspberry PI circuit, just a relay connected to GPIO 

![Raspberry PI Garage Opener]({{sie.url}}/images/20170408-merlin-garage-opener-iot-raspberry-pi.png)
 
Here is how the final solution looks like, I will post the code to my github account
 
![Raspberry PI Merlin Garage Opener]({{sie.url}}/images/20170408-merlin-garage-opener-iot-connected.png)
 
Yay now I can open my garage door remotely from anywhere