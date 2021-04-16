---
title: "Thread SensorTag"
description: "A Low power SensorTag with High quality measures of light, Temperature, Humidity and Pressure. Based on nRF52840 which supports Thread and other protocols"
date: 2021-02-07T08:48:57+00:00
lastmod: 2021-02-14T08:00:00+00:00
images: ["/images/thread_sensortag/concept.png"]
weight: 2
toc: true
---
{{<load-svg-pan-zoom>}}
{{<load-model_viewer>}}
{{<load-photoswipe >}}

{{<icon_button relref="/docs/networks/thread/" text="Networks / Thread" >}}
{{<icon_button relref="/docs/frameworks/chip/" text="Frameworks / CHIP" >}}

{{<image src="/images/thread_sensortag/concept.png" >}}
{{<image src="/images/thread_sensortag/kicad_render.webp" width="300vw" >}}

# Current Version
* version 1.1
* update from v1.0 added RGB LED

Features
* `nRF52840` on a `MS88SF2` module.
* Light sensor `VEMLS6030`
* Temperature, Humidity and pressure`MS8607`
* Reset button `P0.18` and user button SW0 `P1.09`
* RGB LED `P0. 04 06 08`

{{<icon_button href="https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF52840" text="nRF52840" icon="new" >}}

{{<icon_button href="https://www.minew.com/products/nrf52840-module-ms88sf2.html" text="MS88SF2" icon="new" >}}

{{< icon_button href="https://www.mouser.com/datasheet/2/418/5/NG_DS_MS8607-02BA01_B3-1134999.pdf" text="MS86072" icon="new" >}}

{{< icon_button href="https://www.mouser.com/datasheet/2/427/veml6030-1767367.pdf" text="VEML6030" icon="new" >}}

## Manufacturing
* v 1.1 15.04.2021
* design files
{{<icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/tree/v1.1" text="tag v1.1" icon="github" >}}

* gerber files
{{<icon_button text="Gerber files v1.1" href="/data/manufacturing_v1.1.zip" icon="download" >}}

## Schematics

{{< svg-pan-zoom "/images/thread_sensortag/nrf52-sensor-tag.svg" "white" >}}

## Board

{{<image src="/images/thread_sensortag/board_top.webp" width="500px" >}}

## Power Consumption

{{<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vTjyTmBeA3KFeX2gRebiZoTsREhtoXCE8qZstHx1fQO6X83I-5LDIG0I0BbR6QdomUmvdfttt59Nt4Q/pubhtml?gid=0&amp;single=true&amp;widget=true&amp;headers=false" height="200" >}}

# development
## Firmware

{{<icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/tree/main/firmware" text="repo directory" icon="github" >}}

### 01 dongle nrfsdk mqttsn client

{{<hint info>}} Tested firmware with MQTT-SN gateway and Mosquitto broquer
{{</hint>}}

* example based on `nRFSDK for Thread and Zigbee v4.1.0` which path should be declared in an environment variable `THREAD_SDK_ROOT`
* Note that, as specified in the SDK file `components\toolchain\gcc\Makefile.windows` the used toolchain is `GNU Tools ARM Embedded/7 2018-q2-update`

### 04 dongle pio zephyr blink
* zephyr with platformio environment for easy install
* dongle led blink

### 06 tag zephyr hello
* hello world example using the `nrf52840_sensortag` custom board

zephyr examples usage
```bash
west build -b nrf52840_sensortag
west flash
```
### 07 tag zephyr button
* introducing user button interrupt
### 08 tag zephyr thread
* simplest example with thread stack running
* thread stack added through config only

## flashing
* SWD pogo pins adapter
* Pogo Pin P75-E2 Dia 1.3mm Length 16.5mm

{{<icon_button text="STL Model" href="/models/thread4_lower.stl" icon="download" >}}
{{< model_viewer "/models/thread4_lower.glb" "400" >}}
{{< gallery dir="/images/thread_sensortag/swd-pogo" />}}


# Produced versions
* v 1.1 15.04.2021

{{<icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/tree/v1.1" text="tag v1.1" icon="github" >}}

* v 1.0 10.02.2021

{{<icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/tree/v1.0" text="tag v1.0" icon="github" >}}
{{<gfigure src="/images/thread_sensortag/prototype_top.webp" width="200px" >}}
{{<gfigure src="/images/thread_sensortag/prototype_bottom.webp" width="200px" >}}


# FAQ - Discussion
* If you need support, want to ask a question or suggest a different answer, you can join the discussion on the discord server
{{<icon_button text="Discord - #thread-sensortag " href="https://discord.gg/SdKHaAfKN4" icon="discord" >}}

{{<faq>}}
Does this Thread SensorTag support MQTT ?
<--->
At the moment, a firmware is in preparation that support MQTT-SN which needs an MQTT-SN gateway to connect it to an MQTT broquer. Mode details in the [MQTT Sensors Node](/docs/networks/thread/#mqtt-sensors-node)
<===>
Is it possible to save energy by collecting many sensor samples over time and sending them together?
<--->
Yes, sure, the use cases could be slpit as follows :
* For short term buffering, it's possible to use the RAM retention feature, you pay as you go fine granular (per 4 kB block: 30nA)
* For long term buffering, we're no longer on the sensors network use case and rather on the logger tag use case, there you can use the Flash, 1 MB is huge.
In my use case, I don't use buffering in order to allow live update of measures on the user's apps, but combining like a level trigger to immediately send values on big changes with buffering and sending all the values cyclically could be quite smart, you will need a QoS through to ensure your data is really sent after a while, which will also consume RF transactions, I opted for sending every single value on an RF packet which is also used as an alive signal to know when the sensor is dead or out of reach, so no problem if once a packet is lost in a while, but practically, as I use a flood mesh, most measures arrive more than once to the server.
I think after all, such things should be configurable by the end users's app, and should not be hardcoded
{{</faq>}}


