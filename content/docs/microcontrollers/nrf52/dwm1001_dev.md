---
title: "UWB DWM1001 dev"
description: "An Ultra Wide Band development kit including the DWM1001C Module"
date: 2021-05-08T08:00:00+00:00
lastmod: 2021-05-08T08:00:00+00:00
images: ["/images/thread_sensortag/sensortag_v1.1.webp"]
weight: 2
toc: true
#{{<load-svg-pan-zoom>}}
#{{<svg-pan-zoom "/images/uwb/dwm1001-dev-schematic.svg" "white" >}}
---

{{<icon_button relref="/docs/networks/ultrawideband/" text="Networks / Ultra Wide Band" >}}

# Development Board
{{<image src="/images/uwb/DWM1001 DevKit.png" width="250px" >}}

* DWM1001C Module
* STM32F072 as J-Link OB debugger
* USB and battery conenctors
* Raspberry PI Header connector
* User Leds buttons

{{<icon_button text="product page" href="https://www.decawave.com/product/dwm1001-development-board/" icon="new" >}}
{{<icon_button text="datasheet" href="https://www.mouser.de/datasheet/2/412/DWM1001_DEV_Data_Sheet-1950462.pdf" icon="pdf" >}}
{{<icon_button text="user manual" href="https://eu.mouser.com/datasheet/2/412/MDEK1001_System_User_Manual-1.1-1878639.pdf" icon="pdf" >}}



## DWM1001C Module
{{<image src="/images/uwb/dwm1001 module.webp" width="250px" >}}

* DW1000 Transciever
* TWR-RTLS : Two Way Ranging - Real Time Location System
* nRF52832 for a Bluetooth API
* LIS2DH12TR STM 3-Axis Motion Detector

The product page includes datasheets, schematics, firmware,...
{{<icon_button text="product page" href="https://www.decawave.com/product/dwm1001-module/" icon="new" >}}

## DW1000 Transciever
{{<image src="/images/uwb/dw1000.webp" width="150" >}}

* IEEE 802.15.4-2011 UWB
* 6 RF bands : 3.5 GHz - 6.5 GHz
* Datarates : 110 kbps, 850 kbps, 6.8 Mbps
* Voltage 2.8 V - 3.6 V
* sleep 1 uA - deep sleep 50 nA
* TWR, TDOA : Two Way Ranging, Time Difference Of Arrival
* Location precision : 10 cm
* Range : 290 m

{{<icon_button text="datasheet" href="https://www.mouser.de/datasheet/2/412/DW1000_Data_Sheet-1950775.pdf" icon="pdf" >}}


# Software
{{<icon_button text="Decaware" href="https://github.com/Decawave" icon="github" >}}

{{<icon_button href="https://docs.zephyrproject.org/latest/boards/arm/decawave_dwm1001_dev/doc/index.html" text="Zephyr support" >}}
