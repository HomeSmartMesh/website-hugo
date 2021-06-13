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

{{<icon_button relref="/docs/microcontrollers/nrf52/dwm3001_cdk/" text="Microcontrollers / nRF52 / UWB DWM3001 cdk" >}}
{{<icon_button relref="/docs/microcontrollers/nrf52/dwm1001_dev/" text="Microcontrollers / nRF52 / UWB DWM1001 dev" >}}

An Ultra-Wide-Band networks can form a mesh network, in addition, the main feature is the RTLS: Real-Time-Locating-System capability.
# RTLS Gen 2
{{<icon_button relref="/docs/microcontrollers/nrf52/dwm3001_cdk/" text="Microcontrollers / nRF52 / UWB DWM3001 cdk" >}}
{{<image src="/images/uwb/dwm3001-cdk.webp" width="150px">}}

* DW3000 IC family `IEEE 802.15.4z`, `IEEE802.15.4-2015 UWB`,...
* using nRF52833 as host capable of Thread and Bluetooth Mesh and Direction finding

# RTLS Gen 1

{{<image src="/images/uwb/DRTLS.webp">}}

* DRTLS : DWM1001 Two-Way-Ranging RTLS
* RTLS : Real Time Locating System
* PANS : Positioning and Netowrking Stack
* [eCos](https://ecos.sourceware.org/) : Free Open source Real-Time Operating-System

Decawave provides a ready to use localisation system based on a preflashed `DWM1001_PANS_R2.0.hex` firmware. The functional concept is described in `DWM1001 System Overview and Performance.pdf` including the time division protocol and the format of all the 802.15.4 Frames.
## Hardware
UWM Chips available from Decawave, now Qorvo. Qorvo is also the supplier of Apple UWB chips.

More details about the UWB DWM1001 development kit :
{{<image src="/images/uwb/DWM1001 DevKit.png" width="150px" >}}
{{<icon_button relref="/docs/microcontrollers/nrf52/dwm1001_dev/" text="Microcontrollers / nRF52 / UWB DWM1001 dev" >}}

## Module Firmware
* Production Firmware : available as precompiled binary `DWM1001_PANS_R2.0.hex` in order to recover the Module for production grade certified applications
* PANS library : All the main functions performed by the module (TWR, RTLS, Bluetooth, 802.15.4) are provided as a library on top of which a user nRF52832 application can be built.
* PANS library API : described in `DWM1001-Firmware-User-Guide.pdf` and `DWM1001-API-Guide.pdf` 
* Open Source Firmware : multiple examples are provided to kick start development

out of the box config
{{<details "system information log">}}
```conf
 DWM1001 TWR Real Time Location System

 Copyright :  2016-2019 LEAPS and Decawave
 License   :  Please visit https://decawave.com/dwm1001_license
 Compiled  :  Jun  7 2019 18:00:03

 Help      :  ? or help

dwm> si
[000042.580 INF] sys: fw2 fw_ver=x01030001 cfg_ver=x00010700
[000042.580 INF] uwb0: panid=x0000 addr=xDECA50BFBF5001D2
[000042.590 INF] mode: tn (pasv,twr,np,le)
[000042.590 INF] uwbmac: disconnected
[000042.590 INF] uwbmac: bh disconnected
[000042.600 INF] cfg: sync=0 fwup=0 ble=1 leds=1 le=1 lp=0 stat_det=1 (sens=1) m                                   ode=0 upd_rate_norm=1 upd_rate_stat=100 label=DW01D2
[000042.610 INF] enc: off
[000042.610 INF] ble: addr=E0:69:AF:07:D2:94
```
{{</details>}}

* `DWM1001_PANS_R2.0.hex` has the same firmware version but compiled on a different date `Compiled  :  Mar 27 2019 03:35:59`

Firmware recovery
```bash
nrfjprog -f nrf52 --program DWM1001_PANS_R2.0.hex --sectorerase --verify
```

### Product Page
PANS binary library for nRF52832 module DWM1001

{{<icon_button href="https://www.decawave.com/product/dwm1001-development-board/" text="v9_dwm_on_board_package" icon="new">}}


* Path `v9_dwm_on_board_package/examples` : dwm-api-test, dwm-range-iot, dwm-simple, dwm-sniffer, dwm-timer, dwm-uart
* API 2.2, Firmware 1.3.0 : `DWM1001 PANS Library Version 1.3.0`
* Transciever API not exposed, used by the library as dwm_ functions provided as binary files `dwm.o`, `libdwm.a`,...
* available `.emProject` project files for [Segger Studio](https://www.segger.com/products/development-tools/embedded-studio/)
* based on `Nordik SDK` version `12.1.0`
* Used `GNU Tools ARM Embedded` version `5.4 2016q3`

{{<hint warning>}}Although reporting the same firmware version, two UWB commands are missing from the PANS lib `utpg, utps` (transmit power get and set){{</hint>}}

### Decawave Github
Transciever driver DW1000 for nRF52

{{<icon_button href="https://github.com/Decawave/dwm1001-examples" text="dwm1001-examples" icon="github">}}


* Path `dwm1001-examples/examples/` : ss_twr_init, ss_twr_init_int, ss_twr_resp, twi_accel
* DWM1001 board definition (leds, buttons,...)
* based on `Nordik SDK` version `14.2.0`
* decadriver : `DW1000 Device Driver Version 04.00.06`

{{<hint warning>}}with Embedded studio V5.40, it is required to enable the updated #ifdef for `__putchar` in `retarget.c` line 100{{</hint>}}

### Github mynewt
Core source code for MAC layers and ranging
{{<icon_button href="https://github.com/Decawave/uwb-core" text="mynewt uwb-core" icon="github">}}

using the core functions and adding TDMA, Clock calib,...
{{<icon_button href="https://github.com/Decawave/uwb-apps" text="mynewt uwb-apps" icon="github">}}

* examples : sniffer, OTA, TWR, TDOA,...
* running over Apache's [myNewt](https://mynewt.apache.org/) Operating System which comes with code for BT, network,...
### Zephyr Official
{{<icon_button href="https://docs.zephyrproject.org/2.4.0/boards/arm/decawave_dwm1001_dev/doc/index.html" text="Zephyr Port" icon="new">}}

* module port as `decawave_dwm1001_dev` board with `.dts`
* transciever driver `decawave,dw1000` integrated in the spi dts entry
* no uwb specific positioning code

### Zephyr Community

Community contribution of Zephyr based examples with decadriver
{{<icon_button href="https://github.com/foldedtoad/dwm1001" text="dwm1001" icon="github">}}

* using custom board `nrf52_dwm1001`
* Path `dwm1001/examples`
* ex_01 ... ex_14 (simple, sleep, sniff, twr, low power, accell,...)
* decadriver `DW1000 Device Driver Version 05.01.00`
* logs through RTT and serila
{{<hint warning>}}GNU GLP v3{{</hint>}}
{{<hint warning>}}using Zephyr 2.5 config Patches for double floats print `%lf` and `CONFIG_NEWLIB_LIBC=y`
`CONFIG_NEWLIB_LIBC_FLOAT_PRINTF=y`, also sprintf string overflow needs size increase {{</hint>}}


## DW Tranceiver
available in a Decawave webpage

{{<icon_button href="https://www.decawave.com/dw1000/usermanual/" text="dw1000 user manual" icon="new" >}}

* User Manual : latest version 2.18 as of May 2021
* Transmission, Reception, MAC features, Calibration,...

application note and datasheet from forum post (note the datasheet on the product page is version 1.8)
{{<icon_button href="https://decaforum.decawave.com/uploads/short-url/bDBtllhpvFtLXlS6U1OW19f7Li6.pdf" text="APS017 max range in DW1000 systems v1.1" icon="pdf" >}}
{{<icon_button href="https://www.decawave.com/sites/default/files/resources/dw1000-datasheet-v2.09.pdf" text="dw1000 datasheet v2.09" icon="pdf" >}}

SDK available from Decawave uploads as zip

{{<icon_button href="https://www.decawave.com/wp-content/uploads/2019/01/dw1000_api_rev2p14.zip" text="dw1000_api_rev2p14.zip" icon="download" >}}

* document User Guide :  `dw1000_api_rev2p14\dw1000_api_rev2p14_stsw\DW1000_Software_API_Guide_rev2p7.pdf`
* source code decadriver : `DW1000 Device Driver Version 05.01.00`
* ported to STM32 platforms `coocox` and `stsw`

## Raspberry pi software
* DWM Daemon
* DWM Proxy
* webapp http server
* dwm device on `/dev/dwm0`

check that your raspberry pi is a model 3b not b+ with 100 Mbit only with `sudo ethtool eth0 | grep Speed`, which is important for the real time operation of the services.

### DWM Daemon
* service : `dwm1001.service`
* log : `/var/log/dwm-daemon.log`
* config : `/etc/dwm1001/dwm1001.config`
### DWM Proxy
* service : `dwm1001-proxy.service`
* config : `/etc/dwm1001/dwm1001-proxy.config`

### Decawave Webapp
* the webapp is provided on the raspberry pi on the path `/var/www/html/`.
* The webapp is based on [three.js](https://threejs.org/) which is a viable choice for 3d gemoetry calculations of anchors and tag coordinates.
* The view is limited to an orthogonal projection which makes the app 2d only. Although the created tags and anchros are actual 3d meshes, only 2d shapes appear on the screen.
* The down side of 3d is the need for ray casting to create click events on objects. But on the upside it's a frameowrk that can easily extend to 3D models. Maybe for the future, Decawave will get inspired by the [smart home 3d webapp](/docs/applications/home3d/) project.
* The menus are handled with injected jquery html with classes.
* All in all, this webapp is an amazing user experience and a reference for open design, that's how all mdoern apps should be

An easy upload function allows to customize the view with an own floor map. The offset and scale is not easy but manageable

{{<image src="/images/uwb/floormap.webp" width="600px" >}}

### MQTT Config
The decawave webapp communicates with all devices through the 802.15.4-uwb mesh network to send and receive configuration. For that purpose, the IOT Data message is used as config request and response packets.

* Details of the IoT Data message are provided in `DWM1001_System_Overview.pdf` section `9.1.12 IOT Data message`
* config can be updated by sending a request
    * topic `dwm/node/<nodeid>/downlink/config`
    * 802.15.4-uwb downlink message id `UWBMAC_FRM_TYPE_DL_IOT_DATA`
* config is received by a response
    * topic `dwm/node/<nodeid>/uplink/config`
    * 802.15.4-uwb uplink message id `UWBMAC_FRM_TYPE_UL_IOT_DATA`

given that the response is a confirmation it will have a payload similar as the request

{{<details "example Anchor config downlink">}}
```json
{
  "configuration": {
    "label": "RearRight",
    "nodeType": "ANCHOR",
    "uwbFirmwareUpdate": false,
    "leds": false,
    "ble": false,
    "anchor": {
      "routingConfig": "ROUTING_CFG_OFF",
      "initiator": false,
      "position": {
        "x": 3.5500002,
        "y": 1.1,
        "z": 0,
        "quality": 100
      }
    }
  }
}
```
{{</details>}}

{{<details "example Tag config downling">}}
```json
{
  "configuration": {
    "label": "BatteryTag",
    "nodeType": "TAG",
    "uwbFirmwareUpdate": false,
    "leds": false,
    "ble": false,
    "tag": {
      "locationEngine": true,
      "responsive": true,
      "stationaryDetection": true,
      "nomUpdateRate": 100,
      "statUpdateRate": 2000
    }
  }
}
```
{{</details>}}



# Standard

{{<image src="/images/uwb/uwb_standard.webp" width="600px" >}}

{{<icon_button text="802.15.4z" href="https://www.ieee802.org/15/pub/TG4z.html" icon="new" >}}

# References

{{<icon_button text="ACCURATE POSITIONING IN ULTRA-WIDEBAND SYSTEMS" href="http://repository.bilkent.edu.tr/bitstream/handle/11693/13380/10.1109-MWC.2011.5751292.pdf?sequence=1" icon="pdf" >}}

{{<icon_button text="AN ACCURATE UWB RANGING FOR PRECISION ASSET LOCATION" href="http://vlsicad.eecs.umich.edu/BK/Slots/cache/www.ee.vt.edu/~ha/research/publications/uwbst03b.pdf" icon="pdf" >}}


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
