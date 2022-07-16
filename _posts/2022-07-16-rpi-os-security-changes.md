---
title: "Default Raspberry Pi OS password deprecated"
categories:
  - Blog
tags:
  - computers
header:
  image: /assets/images/2022-07-16-header.png
  caption: Linux terminal
---

I just felt like I was beating my head against a brick wall for the past thirty minutes. I wanted to set up a headless Raspberry Pi with a fresh Raspberry Pi OS image and completely failed to SSH in with the time-honoured default username "pi" and password "raspberry".

Trying to search for the issue was useless because of the eleventy million pages proudly stating that the default Raspberry Pi OS password was, indeed, "raspberry".

Eventually, I found a [news post from Raspberry Pi discussing changes to the default OS security](https://www.raspberrypi.com/news/raspberry-pi-bullseye-update-april-2022/). I'm very happy that they've increased the default security, but it would have been lovely if they could have uploaded that information directly to my brain when it happened.

So from April 2022, you can use the [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to set up an account. At the same time you can add your WiFi defaults and localisation options.

![Raspberry Pi Imager options](/assets/images/2022-07-16-rpi-imager-options.png)

Hopefully blog posts like this will become the top result when people search for "raspberry pi default password"...