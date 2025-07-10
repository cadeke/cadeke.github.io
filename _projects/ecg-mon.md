---
layout: page
title: Wireless ECG-monitor
description: International project for making a DIY ECG-monitor
img: assets/img/ecg-mon/ecg-1.png
importance: 1
category: other
---

## Description 
This project was part of the fourth semester of my bachelor's degree. As part of a small exchange project, students from my university (in Belgium) and students from a partner university (in Finland) were tasked with creating a wireless ECG-monitor. For this project, we had to work together in an organized way, because the Finnish students had more knowledge about the hardware, whereas the Belgian students had to take care of the software side.

## Overview
For this project, we split up the tasks in order to combine our strengths. I was responsible for the backend of the application, and making sure the communication between all separate components went smoothly.

The Finnish students were able to create the monitor using an Adafruit Huzzah ESP8266 controller. This would send the measurements to a Raspberry Pi which acted as our server. After some processing the data could then be queried by a Flutter client in order to visualize the heartbeat.

For my part of the project, I experimented with Docker and NodeJS in order to get our server to work. I used three containers:
- InfluxDB: time-series database
- NodeRed: simulation of data points
- Telegraf: vizualizations and monitoring
These three containers where being orchestrated by a docker-compose file, which made the setup very portable, which was one of our main requirements.

In the following screenshot you can see the components used in the setup.

<div class="row">
    <div class="col-sm mt-2 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/ecg-mon/ecg-2.png" title="ducky hardware" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
   Architectural overview of all components used in the project. 
</div>

## Result
We were happy with the results because the whole project was quite a challenge. We had to work fully remotely on the different parts and then make sure all components would work together. In the end we reached our goal of making a DIY ECG-monitor, but some improvements could of course have been made.

## Credits
I would like to thank my teammates for their contributions to this project.

## References
- GitHub repository: [wireless-ecg-monitor](https://github.com/cadeke/wireless-ecg-monitor)
