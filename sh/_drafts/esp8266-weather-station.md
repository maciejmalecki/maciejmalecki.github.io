---
title: Weather Station with ESP 8266
description: 
layout: post
post-url: weather-station
categories: smarthome iot esp8266 mcu
---
With ESP 8266 WiFi module I built several devices very quickly. All of them were powered with 230V socket via power adapter.

Theoretically each ESP module with following pins exposed is suitable. With some concession it should be also doable to implement this device with ESP-01 module.

The device is designed to be powered from battery. The ESP module itself is well known of being power hungry, especially when handling WiFi connection thus it is not very suitable for operating on battery. The specifics of the weather station is, however, that it just wakes up every n minutes, gather measurements and sends it to the hub. This assumption helps us greatly, because we can use so-called deep sleep mode of ESP8266 to reduce current from around 80mA to 10uA.

The ESP8266 itself operates in 3.0-3.6V area and nominal voltage should be around 3.3V. I have seen it working in 2.5-3.0V area as well, however stability problems were occasionally visible. Theoretically a bare ESP module (like ESP-12F or ESP-01) can be powered directly from a pack of 2xAA batteries, but we shouldn't expect a long running time, because the 3.10V initial voltage drops below limits rather quickly.
