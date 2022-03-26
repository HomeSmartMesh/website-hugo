---
title: "UWB DWM1001 dev"
description: "An Ultra Wide Band development kit including the DWM1001C Module"
date: 2021-05-08T08:00:00+00:00
lastmod: 2021-05-08T08:00:00+00:00
images: ["/images/uwb/DWM1001 DevKit.png"]
weight: 2
toc: true
#{{<load-svg-pan-zoom>}}
#{{<svg-pan-zoom "/images/uwb/dwm1001-dev-schematic.svg" "white" >}}
---
{{<load-photoswipe >}}

{{<icon_button relref="/docs/networks/ultrawideband/" text="Networks / Ultra Wide Band RTLS" >}}

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


# Application

{{<button relref="/docs/networks/ultrawideband#mesh-positioning-framework" text="Mesh Positioning Framework" >}}Mesh Positioning Framework{{</button>}}

# 3d printed box for 18650 battery
{{<image src="/images/uwb/devkit/08a UWB 18650 battery.png" width="400" >}}

{{< icon_button text="Fusion360 model" href="/models/DWM1001-Dev-3dprint-18650.f3d" icon="download" >}}
# Gallery

{{< gallery dir="/images/uwb/devkit" />}}

* `01 Unboxing` : A cable and x4 usb angles provided that can keep x4 Anchors up right.
* `06 Additional Passive Tag as serial listener` : Make sure that the listener is configured with the app to join the Network intended to be listened to

# FAQ - Discussion

* Support is available on the [official forum](https://decaforum.decawave.com/). Reviews, experience exchange and ideas related to this page content can be discussed in the forum category
{{<icon_button text="Home Smart Mesh - UWB General" href="https://github.com/HomeSmartMesh/website/discussions" icon="github" >}}
{{<icon_button text="Home Smart Mesh - UWB nRF52 Zephyr" href="https://github.com/nRFMesh/sdk-uwb-zephyr/discussions" icon="github" >}}



{{<faq>}}
How difficult is it to setup a demo ?
<--->
It's actually surprisingly easy. The hardest step is to install the App which I could not find on the appstore and Android prevents installing .apk by default, a trick was to long press "save link as" as simple click does not work, then in the settings the ip of the server has to be set as trusted. The rest is very well documented in the Quick Start guide.
<===>
What to do if the localization in the app is terribly wrong ?
<--->
By default the refresh rate of stationary tags is set to 10 sec, and the algorithm has an additional filter that prevents the tag position from moving to a new absolute measure and rather only improving by a given percentage. By incrinsing the "Stationary update rate" within the range of ~ 2 sec, it's possible to get an expected user experience with a tag visibly converging to the correct location.
<===>
Is it possible to communicate between DWM1001C and DMW1004C ?
<--->
Yes, given that they both use the same DW1000 tranceiver. But they have two different Host microcontrollers so algorith,s have to be ported on both targets nRF52 and STM32 and calibration has to be done accordingly.
{{</faq>}}
