---
title: "UWB DWM3001 cdk"
description: "2nd Generation Ultra Wide Band development kit including the DW3110 transciever"
date: 2021-05-08T08:00:00+00:00
lastmod: 2021-05-08T08:00:00+00:00
images: ["/images/uwb/dwm3001-cdk.webp"]
weight: 2
toc: true
#{{<load-svg-pan-zoom>}}
#{{<svg-pan-zoom "/images/uwb/dwm1001-dev-schematic.svg" "white" >}}
---
{{<load-photoswipe >}}

{{<icon_button relref="/docs/networks/ultrawideband/" text="Networks / Ultra Wide Band RTLS" >}}

# Development Board
{{<icon_button text="product page" href="https://www.qorvo.com/products/p/DWM3001CDK" icon="new" >}}
{{<image src="/images/uwb/dwm3001-cdk.webp" width="150px">}}

* DWM3001C Module
* STM32 as J-Link OB debugger
* x2 micro USB for nRF Data and J-Link Program/serial
* battery conenctor
* Raspberry PI Header connector
* User Leds and buttons

## DWM3001C Module
{{<icon_button text="product page" href="https://www.qorvo.com/products/p/DWM3001C" icon="new" >}}

* DW3110 Transciever
* nRF52833 Thread, Zigbee and Bluetooth (Mesh, Direction finding)
* IMU : Inertial Motion Unit
* TWR and TDoA : Two Way Ranging and Time Difference of Arrival

{{<icon_button text="nRF52833" href="https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF52833" icon="new" >}}

## DW3110 Transciever
{{<icon_button text="product page" href="https://www.qorvo.com/products/p/DW3110" icon="new" >}}

* 2 RF bands : channel 5 (6.5 GHz) and channel 9 (8 GHz)
    * `IEEE 802.15.4z (BPRF mode)`, `IEEE802.15.4-2015 UWB`, `IEEE 802.15.4-2011 UWB`, `IEEE 802.15.8`
    * FiRa interoperability for PHY and MAC
    * Apple U1 chip interoperability in beta evaluation
* TWR, TDOA : Two Way Ranging, Time Difference Of Arrival
    * PDoA : Phase Difference of Arrival for DW3120 and DW3220 only
* Datarates : 850 kbps, 6.8 Mbps
* 1 Antenna : see DW3120/DW3220 for 2 antenna ports
* Backwards compatible with DW1000 for channel 5 6.8 Mbps and 850 kbps
* Location precision : 10 cm
    * Angle precision (for dual antenna) : +/- 10° accuracy, 5° std dev

{{<icon_button text="DW3000 datasheet" href="https://www.decawave.com/wp-content/uploads/2021/03/DW3000_Datasheet.pdf" icon="pdf" >}}
{{<icon_button text="DW3000 user manual" href="https://www.decawave.com/wp-content/uploads/2021/05/DW3000-User-Manual.pdf" icon="pdf" >}}

{{<icon_button text="FiRa" href="https://www.firaconsortium.org/news/press-releases/2020/05/fira-consortium-publishes-uwb-mac-technical-requirements-to-build-ultrawideband-interoperable-ecosystem" icon="new" >}}


# FAQ - Discussion
* Support is available on the [official forum](https://decaforum.decawave.com/). Reviews, experience exchange and ideas related to this page content can be discussed in the forum category
{{<icon_button text="Home Smart Mesh - UWB General" href="https://github.com/HomeSmartMesh/website/discussions" icon="github" >}}
{{<icon_button text="Home Smart Mesh - UWB nRF52 Zephyr" href="https://github.com/nRFMesh/sdk-uwb-zephyr/discussions" icon="github" >}}

{{<faq>}}
Is the DWM3001 compatible with the previous generation DWM1001 ?
<--->
Yes, but only when using channel 5, which might requires specific calibration. Programs designed for channel 9 cannot be directly reused on channel 5.
<===>
What is PDoA:Phase Difference of Arrival how does it compare to TDoA:Time Difference of Arrival ?
<--->
PDoA is more precise than TDoA when differentiating anchors close to each other, but as soon as they're far enough it becomes ambiguous so TDoA and PDoA can be used as complementary to each other.
<===>
Can the DWM3001 act as a TDoA anchor ? (not answered)
<--->
TBC
<===>
What is the difference between `IEEE 802.15.4z` and `IEEE 802.15.4-2011 UWB` ? (not answered)
<--->
TBC
{{</faq>}}
