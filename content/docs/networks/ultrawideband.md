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
{{<load-photoswipe>}}

{{<icon_button relref="/docs/microcontrollers/nrf52/dwm3001_cdk/" text="Microcontrollers / nRF52 / UWB DWM3001 cdk" >}}
{{<icon_button relref="/docs/microcontrollers/nrf52/dwm1001_dev/" text="Microcontrollers / nRF52 / UWB DWM1001 dev" >}}

An Ultra-Wide-Band networks can form a mesh network, in addition, the main feature is the RTLS: Real-Time-Locating-System capability.

# Mesh Positioning Framework
This framework developed as part of the `Home Smart Mesh` project is a takeover of the above mentioned [Zephyr Community](#zephyr-community) with refactoring introducing c++ and a higher level functional layer called `mp` for `mesh positioning` that wraps `dwt_` functions and adds an nRF52 custom RF mesh networking capability.
{{<icon_button href="https://github.com/nRFMesh/sdk-uwb-zephyr" text="sdk-uwb-zephyr" icon="github">}}
{{<hint info>}}Note that Zephyr imported dependencies have been restricted to the needed ones. It is possible to add by editing this [west.yml](https://github.com/nRFMesh/sdk-uwb-zephyr/blob/cd5dd2f1cc9851bccf66e5bdc4e4d3f7e06920e0/west.yml#L20) file e.g. all imports with `import: true`{{</hint>}}

## Nodes Preparation
### TWR Nodes
```bash
west init -m https://github.com/nRFMesh/sdk-uwb-zephyr --mr main
cd uwb/samples/06_uwb_node_sm
west build -b decawave_dwm1001_dev -- -DCONF_FILE=prj.conf
west flash
west flash --snr 760130093
west flash --snr 760130128
```
### Simpl Mesh CLI Node
The back-channel commands and configuration of uwb modes is based on `simplemesh`. Both boards are supported by the cli sample `nrf52840dongle_nrf52840` and `decawave_dwm1001_dev` but the `dwm1001` has a better responsible uart console.
{{<icon_button href="https://github.com/HomeSmartMesh/sdk-hsm-sensortag" text="sdk-uwb-zephyr" icon="github">}}

```bash
west init -m https://github.com/HomeSmartMesh/sdk-hsm-sensortag -mr main
cd hsm/samples/simplemesh_cli
west build -b decawave_dwm1001_dev -- -DCONF_FILE=prj-dwm.conf
west flash
west flash --snr 260103215
```
## Usage
The cli node can take user console input commands, send them through `simplemesh` network and outputs results coming from the network.
### DWT Config
request to node with uid, the node responds with the full dwt config

```shell
>sm/98501ED22B42EB41{"dwt_config":{"chan":5}}
>sm/98501ED22B42EB41{"chan":5,"dataRate":"DWT_BR_6M8","nsSFD":"NonStandard","phrMode":"DWT_PHRMODE_EXT","prf":"DWT_PRF_64M","rxCode":9,"rxPAC":"DWT_PAC8","sfdTO":129,"txCode":9,"txPreambLength":"DWT_PLEN_128"}
```

It is also possible to broadcast the config to all nodes with `sm{payload}` only at heading topic not `sm/uid{payload}`, in this case both nodes respond with the new full config
```shell
>sm{"dwt_config":{"chan":5}}
>sm/E8D81FEE52C283EB{"chan":5,"dataRate":"DWT_BR_6M8","nsSFD":"NonStandard","phrMode":"DWT_PHRMODE_EXT","prf":"DWT_PRF_64M","rxCode":9,"rxPAC":"DWT_PAC8","sfdTO":129,"txCode":9,"txPreambLength":"DWT_PLEN_128"}
sm/98501ED22B42EB41{"chan":5,"dataRate":"DWT_BR_6M8","nsSFD":"NonStandard","phrMode":"DWT_PHRMODE_EXT","prf":"DWT_PRF_64M","rxCode":9,"rxPAC":"DWT_PAC8","sfdTO":129,"txCode":9,"txPreambLength":"DWT_PLEN_128"}
```

### TWR command
* simple `Two Way Ranging` the below command broadcasts a request to all nodes, and each node identified by its short id, will execute the corresponding twr transaction as `initiator` or as `responder`.
```shell
sm{"uwb_cmd":"twr","initiator":0,"responder":1,"at_ms":100}
sm/C24FD51212E905F0{"initiator":0,"range":"0.375","responder":1,"seq":0,"uwb_cmd":"twr"}
```

{{<details "full example of cli input and output">}}
In this example, every simplemesh node starting reserves a short address. The first node starting checks the existance of a corrdinator by calling `node_id_get` if no responce is obtained, the node turns into a coordinator and assigns to itself the node id 0. The second node starting requests a node id and obtains the node id 1 from the coordinator. Each starting node broadcasts its dw1000 tranceiver config and its simplemesh node id.
```shell
>node_id_get:98501ED22B42EB41
node_id_get:98501ED22B42EB41
node_id_get:98501ED22B42EB41
sm/98501ED22B42EB41{"shortid":0}
sm/98501ED22B42EB41{"simplemesh":"started"}
sm/98501ED22B42EB41{"chan":5,"dataRate":"DWT_BR_6M8","nsSFD":"NonStandard","phrMode":"DWT_PHRMODE_EXT","prf":"DWT_PRF_64M","rxCode":9,"rxPAC":"DWT_PAC8","sfdTO":129,"txCode":9,"txPreambLength":"DWT_PLEN_128"}
node_id_get:E8D81FEE52C283EB
node_id_set:E8D81FEE52C283EB:01
sm/E8D81FEE52C283EB{"simplemesh":"started"}
sm/E8D81FEE52C283EB{"shortid":1}
sm/E8D81FEE52C283EB{"chan":5,"dataRate":"DWT_BR_6M8","nsSFD":"NonStandard","phrMode":"DWT_PHRMODE_EXT","prf":"DWT_PRF_64M","rxCode":9,"rxPAC":"DWT_PAC8","sfdTO":129,"txCode":9,"txPreambLength":"DWT_PLEN_128"}

>sm{"twr_command":{"initiator":0,"responder":1,"at_ms":100}}
>sm/E8D81FEE52C283EB{"at_ms":100,"initiator":0,"responder":1,"result":{"range":"0.211"}}
sm{"twr_command":{"initiator":0,"responder":1,"at_ms":100}}
>sm/E8D81FEE52C283EB{"at_ms":100,"initiator":0,"responder":1,"result":{"range":"0.220"}}
```
{{</details>}}

* sequence `Two Way Raning` allows to provide a list for each of the `initiators` or `responders`

Request:
```shell
sm{"uwb_cmd":"twr","initiator":4,"responders":[0,1,2,3],"at_ms":100,"step_ms":10,"count":3,"count_ms":50}
```

{{<details "sequence of the (4x3 = 12) TWR responses">}}
```shell
sm/90A971A3D1A1B648{"initiator":4,"range":"0.305","responder":0,"seq":0,"uwb_cmd":"twr"}
sm/C24FD51212E905F0{"initiator":4,"range":"0.614","responder":1,"seq":1,"uwb_cmd":"twr"}
sm/98501ED22B42EB41{"initiator":4,"range":"0.005","responder":2,"seq":2,"uwb_cmd":"twr"}
sm/E8D81FEE52C283EB{"initiator":4,"range":"0.178","responder":3,"seq":3,"uwb_cmd":"twr"}
sm/90A971A3D1A1B648{"initiator":4,"range":"0.310","responder":0,"seq":4,"uwb_cmd":"twr"}
sm/C24FD51212E905F0{"initiator":4,"range":"0.591","responder":1,"seq":5,"uwb_cmd":"twr"}
sm/98501ED22B42EB41{"initiator":4,"range":"0.038","responder":2,"seq":6,"uwb_cmd":"twr"}
sm/E8D81FEE52C283EB{"initiator":4,"range":"0.183","responder":3,"seq":7,"uwb_cmd":"twr"}
sm/90A971A3D1A1B648{"initiator":4,"range":"0.319","responder":0,"seq":8,"uwb_cmd":"twr"}
sm/C24FD51212E905F0{"initiator":4,"range":"0.577","responder":1,"seq":9,"uwb_cmd":"twr"}
sm/98501ED22B42EB41{"initiator":4,"range":"0.019","responder":2,"seq":10,"uwb_cmd":"twr"}
sm/E8D81FEE52C283EB{"initiator":4,"range":"0.169","responder":3,"seq":11,"uwb_cmd":"twr"}
```
{{</details>}}

### RF Mesh Diagnosis
* `ping` command and response. The goal is to evaluate the link between the cli and one uid node :
  * `rssi` 51 => -51 bBm
  * time in os ticks where 1 ms = 32 ticks
```shell
>sm/1CF6567337562176{"rf_diag":"ping"}
>sm/1CF6567337562176{"rf_diag":"pong","rssi":51,"time":197086}
```

* `ping_target` command and responses. The goal is to evaluate the link between two uid nodes :
  1. the user pastes and broadcasts the command from the cli on the pinger uid `1CF6567337562176` topic
  2. the pinger node `1CF6567337562176` recognises itself and broadcasts a `ping` command on the target uid `CBC216DC164B1DE8` topic
  3. the pinger provides a response where `rssi` and `time` relate to the cli -> pinger rx message
  4. The target of the `ping` request (2.) (`CBC216DC164B1DE8`) broadcasts a `pong` response where `rssi` and `time` relate to the ping rx message (`1CF6567337562176` -> `CBC216DC164B1DE8`)

```shell
1. >sm/1CF6567337562176{"rf_diag":"target_ping","target":"CBC216DC164B1DE8"}
2. >sm/CBC216DC164B1DE8{"rf_diag":"ping"}
3. sm/1CF6567337562176{"rf_diag":"pinger","rssi":52,"time":4200857}
4. sm/CBC216DC164B1DE8{"rf_diag":"pong","rssi":41,"time":3898403}
```
### UWB Diagnosis
* `ping` command and response. In this case, the ping request is a packet sent through `simplemesh` (2.4 GHz RF) to the `pinger` node, which then sends an UWB packet to the `target` node
* the response contains collected data from the DW1000 registers and sent as a `simplemesh` response packet. For details see `DW1000_Software_API_Guide_rev2p7.pdf dwt_readdiagnostics`
  * `fpAmp1` the leading edge signal from the LDE accumulator
  * `fpAmp2` same for index floor firstpath+2
  * `fpAmp3` same for index floor firstpath+3
  * `maxGrowthCIR` Channel Impulse Response Max Growth related to the receive signal power.
  * `maxNoise` peak maximum Noise
  * `rxPreamCount` number of symbols accumulated for the preamble
  * `stdNoise` standard deviation of the noise level
```shell
sm{"uwb_cmd":"ping", "pinger":0,"target":1,"at_ms":100}
sm/C24FD51212E905F0{"diag":{"fpAmp1":7196,"fpAmp2":7220,"fpAmp3":6969,"maxGrowthCIR":1995,"maxNoise":1495,"rxPreamCount":118,"stdNoise":64},"uwb_cmd":"ping"}
```

This command can also be requested to be run a `count` number of times each `count_ms` taken as period
```shell
sm{"uwb_cmd":"ping", "pinger":0,"target":1,"at_ms":100,"count":3,"count_ms":6}
sm/C24FD51212E905F0{"diag":{"fpAmp1":4771,"fpAmp2":7206,"fpAmp3":7313,"maxGrowthCIR":2091,"maxNoise":1516,"rxPreamCount":124,"stdNoise":68},"uwb_cmd":"ping"}
sm/C24FD51212E905F0{"diag":{"fpAmp1":7344,"fpAmp2":7710,"fpAmp3":7008,"maxGrowthCIR":2146,"maxNoise":1712,"rxPreamCount":118,"stdNoise":68},"uwb_cmd":"ping"}
sm/C24FD51212E905F0{"diag":{"fpAmp1":6346,"fpAmp2":6986,"fpAmp3":6808,"maxGrowthCIR":1976,"maxNoise":1339,"rxPreamCount":124,"stdNoise":60},"uwb_cmd":"ping"}
```




## Features
* Using west with a connected zephyr version dependency and a deca driver integration that can be enabled with the flag `CONFIG_DW1000=y`
* using the already available board in Zephyr `decawave_dwm1001_dev` instead of the locally declared board `nrf52_dwm1001`
* adding custom mesh network functionality. The issue is that testing UWB should have another independent communication and configuration channel. Normal Bluetooth cannot cover a big network so a mesh is needed. nRF52832 do not support openthread and Bluetooth Mesh is a big overhead compared to the [simplemesh](https://github.com/HomeSmartMesh/sdk-hsm-sensortag) 
* adding a higher level API to greatly simplify TWR applications and others by eliminating redundant blocks in the code and fitting the whole ranging sequence on a single screen function, the new API MP as `meshpositioning` includes `mp_receive()` that is overloaded with the structure type intended to be received, `mp_request()` when sending a struct and expecting a response . Also for when initiating a delayed transmission either expecting a response `mp_request_at()` or not `mp_send_at()`
* support for C++17 and standard library e.g. string, list, map. Now before the C / C++ skepticism takes over, please consider that C++ shall not be used in a real time system without knowing how C++ works. e.g. bad C++ is adding constructor without knowing when where are resources allocated. good C++ is using typed enums, any compile time typing verifications and dead simple iteration loops.
* using [nlohmann/json](https://github.com/nlohmann/json) JSON for Modern C++ for remote procedure call e.g. an uwb listener has an MQTT interface where an app throws a json config sent over simplemesh to use with dwt_configure()
verbose target log for status register thanks to a simple map flag to string

## TWR GPIO Profiling
* samples used `uwb\samples\twr_initiator` and `uwb\samples\twr_responder`
* Debug PIO used `[nRF52] P0.13` => `M_PIN17` => `J7 pin 8`
* Debug function calls `APP_SET;` and `APP_CLEAR;`

{{<gfigure src="/images/uwb/twr_profiling.png" width="600px" >}}

* in the screenshot below we can profile a Two Way Ranging cycle
* The initiator has two pulses 
  * 1) from first immediate TX until resp RX
  * 2) from intiating the delayed final TX until it is actually transmitted
* The responder has three pulses
  * 1) blocked on RX listening until the first poll message is received
  * 2) from initiating the delayed response transmittion including its actual transmission and then waiting till the reception of the final RX
  * 3) does not include RF transaction and is simply to monitor the time it takes to compute the double precision distance computation

{{<gfigure src="/images/uwb/twr_init_resp_timing.webp" width="800px" >}}
the screenshot was made from the below commit which is a separate branch not including functional refactoring although the main line refactoring showed similar timings
{{<icon_button href="https://github.com/nRFMesh/sdk-uwb-zephyr/commit/28bb91069e5c51336cb47bf0d38d4b5f21947ed9" text="branch twr_functional 28bb910" icon="github">}}


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

### Zephyr with Bluetooth

{{<icon_button href="https://github.com/RT-LOC/zephyr-dwm1001/" text="RT-LOC/zephyr-dwm1001" icon="github">}}



### Arduino
{{<hint danger>}}Although mentioned for completeness Arduino Framework and libraries are not recommended for production development due to Arduino debug and real time limitations and the unofficial (non-tested / incomplete) character of the available libraries as discussed in the [Decawave forum](https://decaforum.decawave.com/t/how-to-read-dws1000-on-arduino-uno/9428/2){{</hint>}}

{{<icon_button href="https://github.com/thotro/arduino-dw1000" text="arduino-dw1000" icon="github">}}

* No longer maintained
* not supporting all functions, e.g. antenna calibration
* License : Apache 2.0
* TWR

{{<icon_button href="https://github.com/F-Army/arduino-dw1000-ng" text="arduino-dw1000-ng" icon="github">}}

* Fork of `arduino-dw1000`
* License : MIT
* TWR


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


# FAQ - Discussion
* Support should be available from the [official forum](https://decaforum.decawave.com/). Reviews, experience exchange and ideas related to this page content can be discussed in the forum category
{{<icon_button text="Discourse - #ultra-wide-band " href="https://homesmartmesh.discourse.group/c/networks/ultrawideband" icon="discourse" >}}

{{<faq>}}
What is Ultra Wide Band ?
<--->
It's a radio technology that is not based on classical frequency or amplitude modulation rather uses a rich signal pulses that occupies a large spectrum band.
<===>
What does Ultra Wide Band mean for home automation ?
<--->
Very low energy level allows small battery operated tags to achieve functions like precise indoors positioning, which so far could only be achieved with heavier equipment like cameras that need high processing power localisation algorithms.
{{</faq>}}
