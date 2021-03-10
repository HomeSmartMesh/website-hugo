---
title: "Thread SensorTag"
description: "A Low power SensorTag with High quality measures of light, Temperature, Humidity and Pressure. Based on nRF52840 which supports Thread and other protocols"
date: 2021-02-07T08:48:57+00:00
lastmod: 2021-02-14T08:00:00+00:00
images: ["/images/thread_sensortag/concept.png"]
weight: 2
toc: true
---

{{<icon_button relref="/docs/networks/thread/" text="Networks / Thread" >}}
{{<icon_button relref="/docs/frameworks/chip/" text="Frameworks / CHIP" >}}

# v1.0 MS8607 VEML6030

{{<image src="/images/thread_sensortag/concept.png" >}}
{{<image src="/images/thread_sensortag/kicad_render.png" width="300vw" >}}


The Thread sensorTag is based on an `nRF52840` on a `MS88SF2` module. The used sensors are `VEMLS6030` for light and `MS8607` for Temperature, Humidity and Pressure.


{{< icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/commit/32a897a91b8c92f2495ef75408c987166ff7b465" text="commit" icon="github" >}}

{{< icon_button href="https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF52840" text="nRF52840" icon="new" >}}

{{< icon_button href="https://www.minew.com/products/nrf52840-module-ms88sf2.html" text="MS88SF2" icon="new" >}}

{{< icon_button href="https://www.mouser.com/datasheet/2/418/5/NG_DS_MS8607-02BA01_B3-1134999.pdf" text="MS86072" icon="new" >}}

{{< icon_button href="https://www.mouser.com/datasheet/2/427/veml6030-1767367.pdf" text="VEML6030" icon="new" >}}

## Schematics

{{< svg-pan-zoom "/images/thread_sensortag/nrf52-sensor-tag.svg" "white" >}}

## Board

{{<image src="/images/thread_sensortag/board_top.png" >}}

{{<image src="/images/thread_sensortag/board_bottom.png" >}}

## 3D Board model

{{< model_viewer "/models/nrf/sensortag_1.glb" "400" >}}


## Repo

{{< icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/commit/fe999911c088dd4ad4b96288b7cdaeced648661c" text="schematics 07/02/2021 commit fe9999"  icon="github" >}}

## Components

### Consumption

{{< iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vTjyTmBeA3KFeX2gRebiZoTsREhtoXCE8qZstHx1fQO6X83I-5LDIG0I0BbR6QdomUmvdfttt59Nt4Q/pubhtml?gid=0&amp;single=true&amp;widget=true&amp;headers=false" height="200" >}}

## Schematics

{{< svg-pan-zoom "/images/thread_sensortag/nrf52-sensor-tag_v0.svg" "white" >}}

## Board

{{< svg-pan-zoom "/images/thread_sensortag/nrf52-sensor-tag-brd_v0.svg" "white" >}}

## 3D Board model

{{< model_viewer "/models/nrf/sensortag_0.glb" "400" >}}

# Firmware
## Breadboard testing

{{<image src="/images/thread_sensortag/breadboard_test.png" >}}

# FAQ - Discussion
* If you need support, want to ask a question or suggest a different answer, you can join the discussion on the discord server
{{<icon_button text="Discord - #thread-sensortag " href="https://discord.gg/SdKHaAfKN4" icon="discord" >}}

{{<faq>}}
Is it possible to save energy by collecting many sensor samples over time and sending them together?
<--->
Yes, sure, the use cases could be slpit as follows :
* For short term buffering, it's possible to use the RAM retention feature, you pay as you go fine granular (per 4 kB block: 30nA)
* For long term buffering, we're no longer on the sensors network use case and rather on the logger tag use case, there you can use the Flash, 1 MB is huge.
In my use case, I don't use buffering in order to allow live update of measures on the user's apps, but combining like a level trigger to immediately send values on big changes with buffering and sending all the values cyclically could be quite smart, you will need a QoS through to ensure your data is really sent after a while, which will also consume RF transactions, I opted for sending every single value on an RF packet which is also used as an alive signal to know when the sensor is dead or out of reach, so no problem if once a packet is lost in a while, but practically, as I use a flood mesh, most measures arrive more than once to the server.
I think after all, such things should be configurable by the end users's app, and should not be hardcoded
{{</faq>}}


