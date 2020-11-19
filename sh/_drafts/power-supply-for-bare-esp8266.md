---
title: Power supply for bare ESP 8266 units
description: 
layout: post
post-url: esp-power-supply
categories: smarthome iot esp8266 mcu
---
For battery powered ESP 8266 projects we would like to have a full control over electrical components that we use on board. This is because everything that draws power will also drain our battery, and this is something we want to avoid. The Wemos D1 Mini module seems to have the lowest memory consumption of them all, but we can still be better. In my battery powered projects I use ESP-12F modules and custom power circuit, that I would like to describe in this post. By the way, this approach will work with all other "bare" ESP modules like ESP-01, ESP-07, ESP-12S to mention just a few.

![ESP-12F and breakout board]({{  "/sh/img/esp-12f-breakout.jpg" | absolute_url }})

The ESP-12F module is a member of so-called castellated module family, that is it is not ready to be used with 2.54 raster boards or breadboards. Castellated modules are designed for mounting on top of other boards using SMD technique. Such modules we can find mounted on top of Wemos D1 Mini, NodeMCU v1, v2 or v3. Luckily it is possible to obtain dedicated breakout boards and solder ESP module on top of it. It is even possible to use regular soldering iron to do this, we just need a little practice in soldering.

### How to start a bare ESP 8266?
We all know that ESP 9266 chip requires 3.3V DC to operate. So it's enough to connect the "**+**" with `VCC` pin and the "**-**" with `GND` pin, and our chip will boot. We'll, not really. We use bare ESP chip which means that we have to take care of the proper pin configuration during startup, otherwise the chip does not boot, even though it is properly powered. Let's see the schematic.

![ESP-12F startup configuration]({{ "/sh/img/esp-12f-startup-configuration.png" | absolute_url }} )

For ESP-12F to start, the `CH_PD`, `GPIO4`, `GPIO0`, `GPIO2` pins must be **pulled up** and the `GPIO15` must be **pulled down**. As pull up/down we use 10kOhm resistors that are connected with `3.3V` and `GND`, respectively. Only with this configuration our chip properly boots up once powered.

Some breakout boards have some of these pull up/down resistors already mounted. In case of the board I use I have already `CH_PD` pulled up, and `GPIO15` pulled down, so I can remove two resistors from by project.

### Reset button
Our bare chip does not have a reset button. Because it is convenient to have one, it is good to mount it in our project. For reset button to work we need additional pull up resistor (again 10kOhm) connected to the `REST` pin, and a button (S1) which shorts `REST` line with `GND`. If we don't need that button, it would be a good idea to keep `REST` pin pulled up all the time.

### Voltage considerations
