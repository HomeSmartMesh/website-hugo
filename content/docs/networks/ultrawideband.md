---
title: "Ultra Wide Band"
description: "Transcievers, modules and Applications for Ultra Wide Band. Focus on Real Time Localisation System"
lead: ""
date: 2021-05-06T08:48:57+00:00
lastmod: 2021-05-15T08:00:00+00:00
draft: false
images: ["/images/uwb/DRTLS.webp"]
weight: 2
toc: true
---
{{<icon_button relref="/docs/microcontrollers/nrf52/dwm1001_dev/" text="Microcontrollers / nRF52 / UWB DWM1001 dev" >}}

An Ultra-Wide-Band networks can form a mesh network, in addition, the main feature is the Real-Time-Locating-System capability.
# Real Time Locating System

{{<image src="/images/uwb/DRTLS.webp">}}

* DRTLS : DWM1001 Two-Way-Ranging RTLS
* RTLS : Real Time Locating System
* PANS : Positioning and Netowrking Stack
* [eCos](https://ecos.sourceware.org/) : Free Open source Real-Time Operating-System

## Hardware
UWM Chips available from Decawave, now Qorvo. Qorvo is also the supplier of Apple UWB chips.

More details about the UWB DWM1001 development kit :
{{<image src="/images/uwb/DWM1001 DevKit.png" width="150px" >}}
{{<icon_button relref="/docs/microcontrollers/nrf52/dwm1001_dev/" text="Microcontrollers / nRF52 / UWB DWM1001 dev" >}}

## nRF52832 Firmware
* Production Firmware : available as precompiled binary `DWM1001_PANS_R2.0.hex` in order to recover the Module for production grade certified applications
* Open Source Firmware : multiple examples are provided to kick start development

### From the Product Page
{{<icon_button href="https://www.decawave.com/product/dwm1001-development-board/" text="Product Page software development kit" icon="new">}}

* available `.emProject` project files for [Segger Studio](https://www.segger.com/products/development-tools/embedded-studio/)
* based on `Nordik SDK` version `12.1.0`
* Used `GNU Tools ARM Embedded` version `5.4 2016q3`
* dwm code provided as binary files `dwm.o`, `libdwm.a`,...
* OS functions are wrapped by the dwm library through functions `dwm_thread_xxx`

### On Decawave Github
{{<icon_button href="https://github.com/Decawave/uwb-apps" text="mynewt examples" icon="github">}}

* many examples including sniffer, OTA, TWR, TDOA,...
* running over Apache's [myNewt](https://mynewt.apache.org/) Operating System which comes with code for BT, network,...
* referencing the Decawave [uwb-core](https://github.com/Decawave/uwb-core) repo

{{<icon_button href="https://github.com/Decawave/dwm1001-examples" text="nRF SDK examples" icon="github">}}

* DWM1001 board definition (leds, buttons,...)
* Two way ranging and accelerometer examples
* based on `Nordik SDK` version `14.2.0`
* source code of the dw1000 spi driver


### Zephyr
{{<icon_button href="https://docs.zephyrproject.org/2.4.0/boards/arm/decawave_dwm1001_dev/doc/index.html" text="Zephyr Port" icon="new">}}

* module port as `decawave_dwm1001_dev` board with `.dts`
* transciever driver `decawave,dw1000` integrated in the spi dts entry
* no uwb specific positioning code

Community contribution of Zephyr based examples with decadriver

{{<icon_button href="https://github.com/foldedtoad/dwm1001" text="Zephyr with decadriver" icon="github">}}

# Standard

{{<image src="/images/uwb/uwb_standard.webp" width="600px" >}}

{{<icon_button text="802.15.4z" href="https://www.ieee802.org/15/pub/TG4z.html" icon="new" >}}


# FAQ

{{<faq>}}
What is Ultra Wide Band ?
<--->
It's a radio technology that is not based on classical frequency or amplitude modulation rather uses a rich signal pulses that occupies a large spectrum band.
<===>
What does Ultra Wide Band mean for home automation ?
<--->
Very low energy level allows small battery operated tags to achieve functions like precise indoors positioning, which so far could only be achieved with heavier equipment like cameras that need high processing power localisation algorithms.
{{</faq>}}
