---
layout: page
title: RubberDucky & PicoDucky
description: Testing out the hotplug attack capabilities of the Hak5 USB Rubber Ducky
img: assets/img/ducky/ducky-1.webp
importance: 1
category: security
---
## Description <a href="#result">test</a>
This project served as the final assignment for my Security course from my bachelor's degree. I was tasked with researching and testing something from the cyber security realm and to present my findings to the lecturer.

## Overview
I chose to investigate the (in)famous Hak5 USB Rubber Ducky because I was very interested by their products and wanted to try them out for myself. I also wanted to test if I could create a DIY-version of the product with open-source software and a cheap microcontroller.

For my tests, I challenged myself with setting up two use-cases where this device could come in handy. The first one was in a security-awareness scenario where the attack would remind a user to always lock their computer when they leave their desk. In a second scenario, I wanted to establish a reverse shell to an attacking machine.

## Result
At the end of this project, I was able to accomplish both of my use-cases. The following screenshot shows the hardware that was used for this project.
<div class="row">
    <div class="col-sm mt-2 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/ducky/ducky-2.jpg" title="ducky hardware" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
   Hardware used for the Ducky project. 
</div>

### Security awareness
For the awareness case, I created a so called Ducky script that would download an image and set it as the wallpaper, then run some commands to max out the volume and lock it so it cannot be turned down, and then finally playing [the Internet's favorite song](https://www.youtube.com/watch?v=dQw4w9WgXcQ&ab_channel=RickAstley) (don't click). This worked quite well on unlocked computers, but was only a bit slow so I would optimize it so it would have a shorter running time.

### Reverse shell
For the reverse shell case, I created a second Ducky script that would disable Windows Defender and start a netcat listener. I could then connect to this listener on my attacker machine. In a real-life scenario, this would of course require a lot more steps, but it was still interesting to see it work in my small proof-of-concept.

### PicoDucky
I also was able to recreate the functionality of the RubberDucky using a Raspberry Pi Pico and the [pico-ducky libary](https://github.com/dbisu/pico-ducky). This process took some time to setup everything correctly, but I was able to run the same script. I found however, that it was a bit more difficult to use because of the loose cables, so I did most of my testing with the actual USB Rubber Ducky.

## Conclusion
The USB Rubber Ducky is a great tool for testing out some simple hotplug attacks. I also think I would use this tool in the future when some script has to be run manually and it cannot be done in a remotely automated way (e.g. using SSH or Ansible). It offers a simple scripting language which you can use to load custom payloads available online or written by yourself.

*Thumbnail source: [Hak5](https://hakshop.myshopify.com/products/usb-rubber-ducky?variant=353378649)*
