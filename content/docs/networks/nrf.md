---
title: "Simple Mesh"
description: "A simpler, more efficient alternative to Z-x, BL-x, Thread, standard RF protocols. Open source SW is provided for custom and off the shelf nRF SoC tags and dongles."
lead: "A simpler, more efficient alternative to Z-x, BL-x, Thread, standard RF protocols. Open source SW is provided for custom and off the shelf nRF SoC tags and dongles."
date: 2020-10-06T08:48:57+00:00
lastmod: 2021-02-14T08:00:00+00:00
draft: false
images: ["/images/nRF52-features.png"]
weight: 3
toc: true
---
{{<load-svg-pan-zoom>}}
{{<load-photoswipe >}}
# Simple Mesh Overview
A simpler, more efficient alternative to Z-x, BL-x, Thread, standard RF protocols. Open source SW is provided for custom and off the shelf nRF SoC tags and dongles.

{{<icon_button relref="/docs/microcontrollers/nrf52/simplemesh_sensortag" text="Simple Mesh SensorTag" >}}

{{<icon_button href="https://github.com/HomeSmartMesh/sdk-hsm-sensortag" text="Simple Mesh and thread on Zephyr" icon="github" >}}

{{<icon_button href="https://github.com/nRFMesh/nRF52_Mesh" text="Simple Mesh Repo" icon="github" >}}

## Features
{{< gfigure src="/images/nRF52-features.png" width="100%" >}}

* 2.4 GHz 2 Mbps Mesh Network
  * tested with 300 packets / second
* Flash time automated database config
* Zero install time configuration
  * No association, No pairing, No factory resets
* Router = Coordinator = Repeater = Sniffer = CLI
* text serial line protocol
* Multiple Network endpoints
  * No single point of failure
* Server, Dongles, Sensors
  * Full Stack Open Source (no softdevices required)
  * HW On the shelves
  * optional DIY HW
* Alive trace routing
  * a heartbeat message that gives info about the age and RF tx/rx hops through the route

see below the [Drivers & Custom Mesh protocol](#drivers--custom-mesh-protocol) section for the RF Protocol details

## Description
* 2 Mbps (e.g. Zgibee is at 250 Kbps to have a higher range), lowering the bitrate does not improve the range enough to get rid of repeaters, and once you need repeaters these are continuous listeners and require permanent power anyway, so the strategy is to have a repeater for every slot of the house and those repeaters can have power amplifiers and the range problem is solved keeping the high bitrate feature.
* successfull stress tests including up to 300 packets / second.
* This [nodes.json](./nodes.json) configuration file contains the short id for device unique identifier (uid64) and other configurations such as the channel, the sleep time, the required function,... Note that a python script using jLink API automate the flashing process as the uid is read every time from the attached device and matches the parameters to be flashed from the config file.
* As the short id (8 bits) is flashed, there is no association process required and all sensor devices can act as a pure beacons (no listening required) which considerably increases battery life time. We can keep arguing about the need of an acknowledged protocol for sensors logging, I do not think it is required as the feedback of battery level and rssi are both logged as well so the user knows if the sensor is in a healthy position or not.
* Router = Coordinator = Repeater = Sniffer = CLI : actually, a configuration activates the repeat functionality in case someone wants a completely stealth sniffer and this config does not require a different FW. There is nothing to coordinate as the mesh is fully dynamic with a flood and time to live concept. And every dongle has a CLI in text mode to report heard packets and takes commands as text input.
* A consequence of the simplified design allows to have multiple dongles in different locations listening to the network and reporting it to mqtt which avoids any single point of failure. By design there is no id 0 privileged coordinator and end devices do not require to know any particular associated address, they just wake up, broadcast and sleep.
* As visible in the gif animations below, Open Source SW is provided for all steps, devices firmware and server python code.



## Static Demo

* [UART : dongle firmware - directory](https://github.com/nRFMesh/nRF52_Mesh/tree/master/applications/04_uart_dongle)
* [USB : dongle firmware - directory](https://github.com/nRFMesh/nRF52_Mesh/tree/master/applications/08_usb_dongle)

    cu -l /dev/ttyACM0 -s 460800 

{{< figure src="/images/nrf_serial.gif" width="100%" >}}

subscribe to topic

    mosquitto_sub -t 'nrf/#' -v | ts

start nrf_mesh [py/nrf_mesh](https://github.com/HomeSmartMesh/raspi/tree/master/py/nrf_mesh) from [raspi](https://github.com/HomeSmartMesh/raspi/) Project


    python3 nrf_mesh.py

{{< figure src="/images/nrf_mqtt.gif" width="100%" >}}

This custom sensors RF mesh can expand to cover a home area. Onces connected to services provided in the [raspi](https://github.com/HomeSmartMesh/raspi) project, including python scripts, influxdb and grafana, the end result dashboard looks like this

{{< figure src="/images/grafana-dashboard.png" width="100%" >}}

---

# Mesh Nodes

## sensor tag
    application/01_sensortag> make flash
    application/01_sensortag> make paramw

{{< figure src="/images/nRF52832_sensortag.png" width="300" >}}

* Status : deployed > 1 year
* nRF52832 module
* Temperature, Humidity, Pressure : BME280
* Light : MAX44009
* Smooth graphana logs with cyclic broadcast ~ 30 sec => battery life ~ 6 month on CR2032
* `make paramw` detects the device uid and writes the config parameters available in [nodes.json](./nodes.json)

{{<icon_button relref="/docs/microcontrollers/nrf52/simplemesh_sensortag" text="Simple Mesh SensorTag details..." >}}

## uart dongle
    application/04_uart_dongle> make flash
    application/04_uart_dongle> make paramw

{{<image src="/images/dongle.png" width="200" >}}

* Status : deployed > 1 year
* buy : "nRF52832 BLE USB UART dongle"
* custom firmware and pogo-pins jtag adapter see below
* RF Mesh repeater + RF Mesh to Host interface
* `make paramw` detects the device uid and writes the config parameters available in [nodes.json](./nodes.json)

{{<icon_button relref="/docs/microcontrollers/nrf52/uart_dongle" text="UART Dongle details..." >}}

## usb dongle (nRF52840)
    application/08_usb_dongle> make flash
    application/08_usb_dongle> make paramw

{{< figure src="/images/nRF52840-dongle-debug.png" width="100%" >}}

* buy : "nRF52840 usb dongle"
* custom firmware for RF mesh
* RF Mesh repeater + RF Mesh to Host interface
* buffered USB CDC tx and rx
* `make paramw` detects the device uid and writes the config parameters available in [nodes.json](./nodes.json)
* pogo-pins jtag adapter 3dprint : 
  * [Version two CAD Model](https://a360.co/3jr91PD)
  * [Version one Fusion360 CAD model](https://a360.co/2CDqeTA)

{{< figure src="/images/pogo-versions.jpg" width="100%" >}}

---

# Protocol


The directory `./drivers/` contains the custom drivers for this project from which the [mesh.c](./drivers/mesh.c) a Simple Mesh Protocol connecting all the devices using a custom RF protocol (without softdevice)
* Sleepy nodes (low power) and router nodes (always listening)
* single layer ultra simple rpotocol. App into mac with unique ids to small ids mapping
* A simple alternative to the Bluetooth Mesh and Zigbee Thread IPV6

{{<figure src="/images/mesh.png" >}}

* Low power node : Mesh node that can be a sensor Tag that wakes up on RTC or on sensor value interrupt to log a parameter. Can also be a button that wakes up on a press PIO interrupt.
* Router node : Mesh node that cooperates with the Low power nodes, the router has to be a device that keeps listening permanently on the same RF channel, this might require ~ 13 mA. Example router devices are light controllers, server’s RF dongle or USB powered sensors and status devices.
* Server : A Raspberry pi running linux. The RF signals come through the serial port or with a USB adapter, then published on MQTT, from which Python scripts inject signals on the Database, connected to Grafana web server. Note that there can be multiple router dongles connected to servers.

## Overview

This project introduce an RF Mesh protocol which is a complete stack implementation to allow devices to communicate in a Mesh Network Topology. This protocol implementation is using the nRF SDK 15 it is using the Radio module through a modified Enhanced Shock-burst custom protocol. Implementations range from nRF24L01+ with an STM8L and STM32 to the nRF51 and nRF52 families that add features such as RSSI and bigger packets sizes.

## Description

* 100% Open source Mesh RF Stack
* Ported on STM8L, STM32, nRF51 and nRF52
* Concept designed for Low Power Nodes and Router Nodes
* Same radio channel configured for the whole network
* Single channel allows maximum power save with unlimited sleep periods, and random wake up with button press or sensor event.
* Mesh with flooding broadcast, where every message has its own configurable time to live. This brings multiple advantages of usage simplicity.
* Short addresses allow efficient packets sizes. Possibility for extensions with short address reuse and exception addresses to allow unlimited network size.
* For simplicity purpose, security is a matter of application payload content.
* The stack is so small that the RAM and ROM are left for the application logic, not requiring a network co-processor.

### Mesh Broadcast
* A simple flood mechanism is implemented where every router repeats the signal heard decrementing the time to live to avoid infinite propagation.
* Future extensions can extend the routing by gathering statistics and deciding of the optimal path in a decentralized fashion. Note that small installations in a home do not bring any significant advantage, thus the interest for a simple protocol for such use case.

### Directed messages with ack
* This type of communication uses a source and destination addresses
* Only the node recognizing itself as destination will send a packet as acknowledge
* The acknowledge packet is propagated the same way, until it reaches the source, which make it an end to end acknowledge
* If the source did not got an acknowledge back while it was requested, it can retry the transmission again.

### Packet Format

custom stack vs softdevices : This protocol is using the direct Radio module capabilities and does not require the sofdevices. The softdevices are Nordic firmware that are closed source and distributed as binaries, they have higher privileges than the user application. Softdevices guarantee that the RF functions keep always working as the user application is untrusted. This comes at a big price to pay, as the rest of the SoC cannot be properly used, no free configuration of interrupts for example, they have also a big memory footprint that prevents safe double paged firmware updates.

{{< figure src="/images/packet.png" width="100%" >}}

The configuration has discarded the usage of S0 and S1 which are intended for compatibility with other protocols which require a certain number of bits on the air that do not map to 8 aligned to form bytes in memory.

{{< figure src="/images/protocol.png" width="100%" >}}

the RF Payload Format contains another software header. That is the header used to manage the packets in the mesh network.


* control: bit 7 defines whether a packet is to be broadcasted or has a target destination. Bits 6 and 5 allow safe communication with acknowledge, and a request response type. That allows the sw stack to handle the acknowledge and re-transmission. bit 3 allows streaming of messages without acknowledge.
* pid: is defining the application function which maps to a particular payload serialization
* Source and dest: are the Node Id, and are unique within every mesh. Although 8 bits, exceptions allow further reserved sizes.
* Time to live : used by the routers to know when to drop the packet.

### Alive trace routing

{{< figure src="/images/alive-trace-route.png" width="100%" >}}

* All Nodes including the low power, send a periodic “alive” packet
* The initiator includes a counter that informs about the live cycle count
* Every retransmission appends information about
  * Received RSSI (Radio Signal Strength Indicator)
  * The Node Id of the Router performing the re transmission
  * Transmission power with which the packet has been re transmitted

{{< figure src="/images/alive-json.png" width="100%" >}}

* The “alive” packet that reaches the server’s dongle has the full route information
* Multiple paths can reach the server for the same original broadcast
* A single “alive” packet can provide link status of the complete mesh netwrok
* The signal route information is packed in a json structure
* json structures are broadcasted on the jNodes topics path

## Mesh Stack

Functions call graph. Fifos dataflow between interrupts and main loop.

{{< svg-pan-zoom "/images/nrf_mesh.svg" >}}

---

# Tools
## ./nRF5_SDK_15.0.0_prf/
This is a submodule that minifies the nRF15 SDK for the content required by this repository.
When compiling the nRF applications firmware don't forget to call the submodule update command, it is also possible to do that without git by going to the repository link, downloading it and placing it under the same path. Although if you do not use git command line at this stage, you might want to consider following a git tutorial.

    git submodule update --init

## ./tools/
Contains the Makefile extensions that allow :
* switch power on and off using a Segger jlink
* read and write parameters matching the device's unique identifier read from the HW registers

## installation

* For the Raspberry pi tools and scripts see full installation steps in the [Installation](https://www.homesmartmesh.com/install/) website page
* For the nRF52 firmare :
    * GNU Tools ARM Embedded version 6 2017-q2-update, referenced from the SDK Makefile.windows
    * Python [jlink wrapper](https://github.com/square/pylink). Used in Makefile and tools of this repo, more on [pylink.readthedocs](http://pylink.readthedocs.io/en/latest/pylink.html). Note that it has to be used with a 32 bit python version referenced in the tools scripts as **C:\Python27\python.exe**. wrapper scripts already availabe in the tools directory

## Details about the nRF52 Frimware environment

{{< figure src="/images/pylink.png" width="100%" >}}

* `nodes.json` is provided locally in this repo and the docker command [nodes-server.sh](./nodes-server.sh) can also serve it at http://hostname:8080/nodes.json

{{< figure src="/images/nodes.json.png" width="100%" >}}

The parameters here are used by external tools to have a consistent undersanding of the mesh network. From simple id to name translation, to location display according to coordinates down to user data flashing.

## Configuration
### sdk config tool
Once in the application directory just use ```make conf``` to call a cmsis [configuration wizard](http://helmpcb.com/software/cmsis-configuration-wizard), as provided in the SDK. Note that it was here extended to make the user drivers shared and configurable as well, e.g. the I²C frequency of the **Application** in the screenshot

{{< figure src="/images/cmsis-wizard.png" width="100%" >}}

### Automated mesh devices configuration
 User data flashing is done with Pylink which reads in [uicr.py](tools\uicr.py) the registers of the attached device, look it up in the **config** file, retrives which parameters should be flashed, the mapping of parameters to CUSTOMER_X registers come from "uicr_map.json".

## simple board switch
The repo contais a directory for boards declaration "boards/" and a directory for applications "applications/". Although every application targets a particular board, it is possible to use any application for any other board. In the makefile a sinlge line has to be edited

    USED_BOARD := BOARD_NRF52_SENSOR_TAG

## PPI automation
The programmable peripheral interconnect is used for "real" real-time operations rf time synch,...

{{< figure src="/images/ppi.svg" width="100%" >}}


# Raspi Host Controller Interface

## HCI MQTT Test vector

### Commands
get mesh node id of attached dongle

    mosquitto_pub -t 'cmd/request/get_node_id' -m ''

Response

    cmd/response/get_node_id {"cmd": "get_node_id", "node_id": "73"}

get channel of attached dongle

    mosquitto_pub -t 'cmd/request/get_channel' -m ''

Response

    cmd/response/get_channel {"cmd": "get_channel", "channel": "2"}

### Remote Commands
Request to set remote node 74 on RF channel 2

    mosquitto_pub -t 'remote_cmd/request/set_channel' -m '{"channel":2,"remote":74}'

Response

    remote_cmd/response/set_channel {"pid": "236", "ctrl": "0x02", "src": "74", "dest": "73", "cmd": "set_channel", "set": "2", "get": "2"}

### Actions
Ping node 74

    mosquitto_pub -t 'Nodes/79/ping' -m '{"remote":"74"}'

Aknowledge

    Nodes/79/ack 1

# Other nRF52 Applications
    cd ./applications/

## 02 acell tag
    application/02_accell_tag> make flash

{{< figure src="/images/motion_tag.png" width="300" >}}

* Status : experimental, low battery life
* nRF52832 module
* MPU-6050 module
* interrupt pio from MPU-6050

## 03 buttons
    application/03_buttons> make flash

{{< figure src="/images/buttons.png" width="300" >}}

* Status : experiemntal, low battery life
* nRF52832 module
* x6 buttons

## 05 rotary decoder
    application/05_rotary_decoder> make flash

{{< figure src="/images/rf_encoder.png" width="100%" >}}

* buy : "600 pulses Optical Rotary encoder"
* custom wiring +5V, encoder pullups to +3.3 V
* rf timestamp synchronisation
* HW capture of timestamp
* RF Mesh log of modulo 600 position with timestamp
* 4 ms status on change, otherwise 4 s reminder


## 06 bldc 52832
    application/06_bldc_52832> make flash

{{< figure src="/images/bldc_control.png" width="100%" >}}

* Status : RF magnetic angle control OK, rest is in development
* nRF52832 sensor tag board used
* buy : "L6234d breakout"
* custom wiring power supply 9.6 V / nRF powered with 3.3 V separately
* RF bldc control : magnetic angle, voltage ratio, speed, absolute angle


## 07 bldc 52840
    application/07_usb_dongle> make flash

{{< figure src="/images/nrf52840-usb-dongle.png" width="300" >}}

* Status : preparation
* previous application "03 bldc 52832" ported to the nRF52840-dongle

# FAQ - Discussion
If you need support, want to ask a question or suggest an idea, you can join the discussion on the forum
{{<icon_button text="Home Smart Mesh - sdk-hsm-sensortag" href="https://github.com/HomeSmartMesh/sdk-hsm-sensortag/discussions" icon="github" >}}

{{<faq>}}
What are the alternatives to Simple-Mesh ?
<--->
Simple Mesh is a playground for Mesh experimentation but does not intend to cover production grade features, therefore when it comes to 2.4 GHz mesh solutions, Standards such as [Zigbee](/docs/networks/zigbee/) and [Thread](/docs/networks/thread/) exist, provide all needed features and are more affordable as the documentation is open and the development is also open source.
{{</faq>}}
