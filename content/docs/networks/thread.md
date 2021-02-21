---
title: "Thread"
description: ""
lead: ""
date: 2020-10-06T08:48:57+00:00
lastmod: 2020-10-06T08:48:57+00:00
draft: false
images: []
weight: 2
toc: true
---
# Relates to
{{<icon_button relref="/docs/frameworks/zephyr/" text="Zephyr RTOS" >}}
{{<icon_button relref="/docs/frameworks/chip/" text="Project CHIP" >}}

# Overview
{{< svg-pan-zoom "/images/openthread-environment-1.1.svg" >}}

* some elements have link to the corresponding websites


{{< icon_button href="https://openthread.io/" text="Open Thread" icon="new" >}}

{{< icon_button href="https://openthread.io/guides/thread-primer/ipv6-addressing" text="IPV6 adressing"  icon="new" >}}

{{< icon_button href="https://openthread.io/guides/thread-primer/network-discovery" text="Network discovery"  icon="new" >}}

{{< icon_button href="https://openthread.io/guides/border-router/docker/test-connectivity" text="test connectivity"  icon="new" >}}

## Cheat sheet
* IPv6 bit length : 128 bit = 8x 16 ibts => `00b1:00b2:00b3:00b4:00b5:00b6:00b7:00b8`
* Link-Local prefix : `fe80::/16`
* Mesh-Local prefix : `fd00::/8`
* Multicast Link-Local : `ff02::1`
* Multicast Mesh-Local : `ff03::1`
* RLOC16 : Routing locator in 16 bits `0x0401`
* RLOC16 Router mask : `0x0400`
* IID : Interface Identifier (includes RLOC16)
* RLOC = Mesh-Local Prefix + IID

## Specification

{{<icon_button href="https://www.threadgroup.org/ThreadSpec" text="request the specification..."  icon="new" >}}

## Tools

{{<icon_button href="https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Connect-for-desktop/Download#infotabs" text="nRF Connect for desktop..."  icon="new" >}}

{{<icon_button href="https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Command-Line-Tools/Download#infotabs" text="nRF Command Line Tools..."  icon="new" >}}

{{<icon_button href="https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads/7-2018-q2-update" text="GNU Arm Embedded Toolchain v7-2018-q2"  icon="new" >}}

{{<icon_button href="https://infocenter.nordicsemi.com/topic/struct_sdk/struct/sdk_thread_zigbee_latest.html" text="nRF SDK for Thread and Zigbee v4.1.0..." icon="new" >}}

{{<icon_button href="https://www.nordicsemi.com/Software-and-tools/Software/nRF5-SDK-for-Thread-and-Zigbee/Download#infotabs" text="download the SDK" icon="new" >}}

{{<icon_button href="http://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/gs_assistant.html" text="nRF Connect SDK installing..."  icon="new" >}}

* install `nRF Connect for deskop`
* From within nRF connect, install `Toolchain Manager`
* From the Toolchain manager, install the last `nRF Connect SDK`

## Thread Nodes
A Thread network setup contains the following Nodes

* network co.processor : dongle connected to the raspberry pi
* border router host : services running on a raspberry pi
* cli tester : command line dongle through serial monitor for testing
* mqtt-sn : end device with mqtt-sn such as sensors

* [Topology Monitor](https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Thread-topology-monitor) : GUI interface

* [Wireshark sniffer](https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Sniffer-for-Bluetooth-LE) : dongle that captures all frames and shows them on wireshark

# Radio Co-Processor (RCP)
This is the new version recommended by OpenThread for current and new versions aof border routers
##  Zephyr v2.5.99
* Zephyr version `2.5.99` has a `coprocessor` directory replacing the old `ncp`
* For details on Zephyr install see
{{<icon_button relref="/docs/frameworks/zephyr/" text="Zephyr RTOS" >}}

* add `CONFIG_BOARD_HAS_NRF5_BOOTLOADER=n` to `prj.conf`

```bash
cd zephyrproject/zephyr/samples/net/openthread/coprocessor
west build -b nrf52840dongle_nrf52840 -- -DCONF_FILE="prj.conf overlay-rcp.conf overlay-usb-nrf-br.conf"
```
* current status `Failed`

{{<details "error details...">}}
```bash
FAILED: zephyr/CMakeFiles/zephyr.dir/drivers/ieee802154/ieee802154_nrf5.c.obj
In file included from D:/Projects/zephyrproject/zephyr/drivers/ieee802154/ieee802154_nrf5.c:42:
D:/Projects/zephyrproject/zephyr/drivers/ieee802154/ieee802154_nrf5.h:11:10: fatal error: ieee802154_radio.h: No such file or directory
   11 | #include "ieee802154_radio.h"
      |          ^~~~~~~~~~~~~~~~~~~~
compilation terminated.
[103/355] Building C object zephyr/subsys/net/CMakeFiles/subsys__net.dir/buf.c.obj
ninja: build stopped: subcommand failed.
FATAL ERROR: command exited with status 1: 'C:\Program Files\CMake\bin\cmake.EXE' --build 'D:\Projects\zephyrproject\zephyr\build'
```
{{</details>}}



# Network Co-Processor (NCP)
Note, see the RCP version above for recent border routers. Given the open source nature of OpenThread, it is quite challenging to find the right build instructions and version match to the right border router. Different NCP versions are listed here
## Nordic - nRF 4.1.0
* successfull match between the SDK and Rpi image, both available from the link below
  * nRFSDK for Thread and Zigbee v4.1.0
  * RaspPi OT Border Router Demo v4.1.0-1.alpha
{{<icon_button href="https://www.nordicsemi.com/Software-and-tools/Software/nRF5-SDK-for-Thread-and-Zigbee/Download" text="nRF SDK Download" icon="new" >}}


build and flash the ncp example
```bash
>cd "examples\thread\ncp\ftd\usb\pca10059\mbr\armgcc\Makefile"
>make
>make flash_mbr
>make flash
```

## Zephyr v2.4.99
* This section is about the `nRF Connect SDK` using the `zephyr` directory
* successfully built and tested with `v1.4.99-dev1` which contains Zephyr version `2.4.99`
* recognised as USB device
{{<icon_button href="http://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/gs_assistant.html" text="nRF Connect SDK installing..."  icon="new" >}}

* add `CONFIG_BOARD_HAS_NRF5_BOOTLOADER=n` to `prj.conf`

```bash
cd nrf/v1.4.99-dev1/zephyr/samples/net/openthread/ncp
west build -b nrf52840dongle_nrf52840 -- -DCONF_FILE="prj.conf overlay-usb-nrf-br.conf"
```

## Nordic - nRF Connect
* This section is about the `nRF Connect SDK` using the `nrf` directory
* Not yet successfull with the nRF52 dongle, likely due to the missing usb overlay
{{<icon_button href="http://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/gs_assistant.html" text="nRF Connect SDK installing..."  icon="new" >}}

* add `CONFIG_BOARD_HAS_NRF5_BOOTLOADER=n` to `prj.conf`

```bash
cd nrf/v1.4.99-dev1/nrf/samples/openthread/ncp
west build -b nrf52840dongle_nrf52840 -- -DCONF_FILE="prj.conf overlay-logging.conf"
```

## Open Thread

Available pre-build firmware from [this page](https://openthread.io/platforms/co-processor/firmware) is `ncp` not `rcp`therefore results in thsi error
```bash
a82cab962603 otbr-agent[196]: [CRIT]-PLAT----: Init() at ../../third_party/openthread/repo/src/lib/spinel/radio_spinel_impl.hpp:255: RadioSpinelIncompatible
```

{{<icon_button href="https://openthread.io/platforms/co-processor/firmware#download_nrf52840_firmware_image" text="nRF52840 ncp firmare..."  icon="new" >}}

```cmd
nrfjprog -f nrf52 --program ot-ncp-ftd-gd81d769e-nrf52840.hex --sectorerase --verify
```

As explained in [this ticket](https://github.com/openthread/ot-br-posix/issues/642), rcp not ncp has to be built and used 

{{< hint warning >}}I could not find build instructions, e.g. missing tools 'aclocal',... the 'OpenThread' BR is at the moment not yet tested
{{</ hint >}}

# Border Router
* Border router solutions available from `Nordic` and from `Open Thread`.
* A border router requires 
  * a USB dongle as `Network Co-Processor`
  * a `server` in our case on a raspberry pi

#### Manual Build
{{<icon_button href="https://openthread.io/guides/build#how_to_build_openthread" text="How to build"  icon="new" >}}

```bash
  git clone https://github.com/openthread/openthread
  sudo ./script/bootstrap
  ./bootstrap
  ./configure --enable-cli --enable-radio-only
  make
```

## Server Setup
### Nordic - sdcard image

* download a ready raspberry pi image
* successfully tested with the Nordif Firmware from the SDK v4.1.0

{{<icon_button href="https://www.nordicsemi.com/Software-and-tools/Software/nRF5-SDK-for-Thread-and-Zigbee/Download#infotabs" text="Download the nRF5 SDK for Thread and Zigbee..."  icon="new" >}}

### OpenThread - docker
* Not successfull yet, even with the pre-built binaries available on the OpenThread website

```bash
docker run --sysctl "net.ipv6.conf.all.disable_ipv6=0 \
        net.ipv4.conf.all.forwarding=1 net.ipv6.conf.all.forwarding=1" \
        -p 8080:80 --dns=127.0.0.1 -it --volume \
        /dev/ttyACM0:/dev/ttyACM0 --privileged openthread/otbr \
        --radio-url spinel+hdlc+uart:///dev/ttyACM0
```

{{<icon_button href="https://openthread.io/guides/border-router/docker/run" text="More details on 'Run OTBR Docker'..."  icon="new" >}}

## Form a network

Upon success you should be able to connect on the raspberry pi url `http://10.0.0.41/`

{{<image src="/images/thread_sensortag/OTBR_server.png">}}

* on the left menu click on `Join` then select the required paramters (e.g. Channel,...) then click on `FORM` 
## wireshark sniffing

  git clone https://github.com/NordicSemiconductor/nRF-Sniffer-for-802.15.4

* Flash the `nrf802154_sniffer_dongle.hex`

When Forming a network, some Pakets can be sniffed including advertisment

{{<image src="/images/thread_sensortag/wireshark_startup.png">}}


# MQTT Sensors Node
* MQTT-SN is a protocol not requiring `Thread` necessarily, it is rather a protcol for a bridge that allows clients to interact with an MQTT Broquer with minimal payload and packets transfers, therefore, it is well adapted to ip over low power mesh networks such as `Thread`.

* Example firmware `mqttsn_sleepy_publisher` and `mqttsn_client_publisher` are a port of the nRFSDK example from PCA10056 which was the only supported board to the PCA10059, the nRF52840 USB dongle, therefore the search gateway, connect, publish sequence have been merged in on button and run cyclically on each new press.

{{<icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/tree/main/firmware/mqttsn_sleepy_publisher" text="mqttsn_sleepy_publisher"  icon="github" >}}

{{<icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/tree/main/firmware/mqttsn_client_publisher" text="mqttsn_client_publisher"  icon="github" >}}

{{<icon_button href="https://www.oasis-open.org/committees/download.php/66091/MQTT-SN_spec_v1.2.pdf" text="MQTT-SN_spec_v1.2.pdf"  icon="new" >}}

useful commands
```bash
sudo systemctl status paho-mqttsn-gateway.service
sudo /usr/sbin/MQTT-SNGateway -f /etc/paho-mqtt-sn-gateway.conf
sudo nano /etc/paho-mqtt-sn-gateway.conf
```
{{<details "paho-mqtt-sn-gateway.conf" >}}
```conf
#**************************************************************************
# Copyright (c) 2016-2019, Tomoaki Yamaguchi
#
# All rights reserved. This program and the accompanying materials
# are made available under the terms of the Eclipse Public License v1.0
# and Eclipse Distribution License v1.0 which accompany this distribution.
#
# The Eclipse Public License is available at
#    http://www.eclipse.org/legal/epl-v10.html
# and the Eclipse Distribution License is available at
#   http://www.eclipse.org/org/documents/edl-v10.php.
#***************************************************************************
#
# config file of MQTT-SN Gateway
#

# IPv4 Address of iot.eclipse.org
BrokerName=10.0.0.42
BrokerPortNo=1883
BrokerSecurePortNo=8883

#
# When AggregatingGateway=YES or ClientAuthentication=YES,
# All clients must be specified by the ClientList File
#

ClientAuthentication=NO
AggregatingGateway=NO
QoS-1=NO
Forwarder=NO

#ClientsList=/path/to/your_clients.conf

PredefinedTopic=NO
#PredefinedTopicList=/path/to/your_predefinedTopic.conf

#RootCAfile=/etc/ssl/certs/ca-certificates.crt
#RootCApath=/etc/ssl/certs/
#CertsFile=/path/to/certKey.pem
#PrivateKey=/path/to/privateKey.pem

GatewayID=1
GatewayName=PahoGateway-01
KeepAlive=900
#LoginID=your_ID
#Password=your_Password


# UDP
GatewayPortNo=10000
MulticastIP=225.1.1.1
MulticastPortNo=1883

# GatewayUDP6Broadcast address is set to all Thread devices address
# in order to enable Thread Sleepy Devices to receive multicast messages
# sent from the gateway.
# UDP6
GatewayUDP6Port = 47193
GatewayUDP6Broadcast = ff03::1
GatewayUDP6If = wpan0

# XBee
Baudrate=38400
SerialDevice=/dev/ttyUSB0
ApiMode=2

# LOG
ShearedMemory=NO;
```
{{</details>}}

Test vector for sending a `SEARCHGW` message to the MQTT-SQ gateway service
```bash
#send (Length 3)(MsgType SEARCHGW=1)(Radius 1)
echo -n -e "030101" | xxd -r -p | nc -u fd11:1111:1122:0:98bf:60c7:9431:ee90 47193
```

## Firmware flow diagram
running the firmware example `mqtt_client_publisher` results in the following logs

{{<details "MQTT-SN gateway log" >}}
```shell
pi@raspberrypi:~ $ sudo /usr/sbin/MQTT-SNGateway -f /etc/paho-mqtt-sn-gateway.conf
ClientList can not open the Predefined Topic List.     /etc/predefinedTopic.conf

 ***************************************************************************
 * MQTT-SN Transparent Gateway
 * Part of Project Paho in Eclipse
 * (http://git.eclipse.org/c/paho/org.eclipse.paho.mqtt-sn.embedded-c.git/)
 *
 * Author : Tomoaki YAMAGUCHI
 * Version: 1.3.1
 ***************************************************************************

20210213 194529.539 PahoGateway-01 has been started.

 ConfigFile: /etc/paho-mqtt-sn-gateway.conf
 SensorN/W:   Gateway Port: 47193 Broadcast Address: ff03::1 Interface: wpan0
 Broker:     10.0.0.42 : 1883, 8883
 RootCApath: (null)
 RootCAfile: (null)
 CertKey:    (null)
 PrivateKey: (null)



20210213 194535.931   SEARCHGW          <---  Client                              03 01 01
20210213 194535.932   GWINFO            --->  Clients                             03 02 01

20210213 194541.989   CONNECT           <---  nRF52840_publisher                  18 04 04 01 00 3C 6E 52 46 35 32 38 34 30 5F 70 75 62 6C 69 73 68 65 72
20210213 194541.997   CONNECT           ===>  nRF52840_publisher                  10 1E 00 04 4D 51 54 54 04 02 00 3C 00 12 6E 52 46 35 32 38 34 30 5F 70 75 62 6C 69 73 68 65 72
20210213 194542.042   CONNACK           <===  nRF52840_publisher                  20 02 00 00
20210213 194542.042   CONNACK           --->  nRF52840_publisher                  03 05 00

20210213 194542.055   REGISTER    0001  <---  nRF52840_publisher                  1D 0A 00 00 00 01 6E 52 46 35 32 38 34 30 5F 72 65 73 6F 75 72 63 65 73 2F 6C 65 64 33
20210213 194542.056   REGACK      0001  --->  nRF52840_publisher                  07 0B 00 01 00 01 00

20210213 194550.331   PUBLISH     0002  <---  nRF52840_publisher                  08 0C 20 00 01 00 02 01
20210213 194550.331   PUBLISH     0002  ===>  nRF52840_publisher                  32 1C 00 17 6E 52 46 35 32 38 34 30 5F 72 65 73 6F 75 72 63 65 73 2F 6C 65 64 33 00 02 01
20210213 194550.338   PUBACK      0002  <===  nRF52840_publisher                  40 02 00 02
20210213 194550.338   PUBACK      0002  --->  nRF52840_publisher                  07 0D 00 01 00 02 00

20210213 194557.499   PUBLISH     0003  <---  nRF52840_publisher                  08 0C 20 00 01 00 03 00
20210213 194557.500   PUBLISH     0003  ===>  nRF52840_publisher                  32 1C 00 17 6E 52 46 35 32 38 34 30 5F 72 65 73 6F 75 72 63 65 73 2F 6C 65 64 33 00 03 00
20210213 194557.515   PUBACK      0003  <===  nRF52840_publisher                  40 02 00 03
20210213 194557.516   PUBACK      0003  --->  nRF52840_publisher                  07 0D 00 01 00 03 00

20210213 194602.541   PUBLISH     0004  <---  nRF52840_publisher                  08 0C 20 00 01 00 04 01
20210213 194602.542   PUBLISH     0004  ===>  nRF52840_publisher                  32 1C 00 17 6E 52 46 35 32 38 34 30 5F 72 65 73 6F 75 72 63 65 73 2F 6C 65 64 33 00 04 01
20210213 194602.547   PUBACK      0004  <===  nRF52840_publisher                  40 02 00 04
20210213 194602.547   PUBACK      0004  --->  nRF52840_publisher                  07 0D 00 01 00 04 00
```
{{</details>}}

{{<details "Firmware J-Link RTT log">}}
```shell
<info> app_timer: RTC: initialized.
<info> app: Thread version   : OPENTHREAD/20191113-00534-gc6a258e3; NRF52840; Apr  5 2020 21:53:43
<info> app: Network name     : OpenThreadDemo
<info> app: Thread interface has been enabled.
<info> app: 802.15.4 Channel : 13
<info> app: 802.15.4 PAN ID  : 0x1234
<info> app: Radio mode       : rx-on-when-idle
<warning> app_timer: RTC instance already initialized.
<info> app: State changed! Flags: 0x0117D33D Current role: 1
<info> app: State changed! <info> app: (0)
<info> app: MQTT-SN event: Client has found an active gateway.
<info> app: MQTT-SN event: Client has found an active gateway.
<info> app: MQTT-SN event: Gateway discovery procedure has finished.
<info> app: MQTT-SN event: Gateway discovery result: 0x0.
<info> app: (1)
<info> app: Wake up first time
<info> app: MQTT-SN event: Client connected.
<info> app: MQTT-SN event: Client registered topic.
<info> app: MQTT-SN event: Topic has been registered with ID: 1.
<info> app: (2)
<info> app: Wake up next times
<info> app: MQTT-SN event: Client has successfully published content.
<info> app: (2)
<info> app: Wake up next times
<info> app: MQTT-SN event: Client has successfully published content.
<info> app: (2)
<info> app: Wake up next times
<info> app: MQTT-SN event: Client has successfully published content.
```
{{</details>}}

## Firmware search gateway

{{< svg-pan-zoom "/images/thread_sensortag/mqtt-sn gateway.svg" >}}



# CHIP Node
As the `CHIP Protocol` can run over `Thread`, it is possible to connect CHIP applications.

{{<icon_button relref="/docs/frameworks/chip/" text="More about Project CHIP" >}}

# Test and Debug

## with server command line
* using `wpanctl`

{{< details "host wpantund wpanctl. Click to expand..." >}}
```shell
sudo nano /etc/wpantund.conf

sudo wpanctl status

sudo wpanctl scan

sudo wpanctl leave

sudo wpanctl setprop w

sudo wpanctl setprop Channel: 15

sudo wpanctl setprop Network:PANID 0x1234

sudo wpanctl setprop Network:Key 00112233445566778899aabbccddeeff

sudo wpanctl config-gateway -d "fd11:22::"

sudo wpanctl form "nRF52840thread"

sudo wpanctl permit-join 300

w join -n "MeasureFall" -c 15 -k 00112233445566778899aabbccddeeff -p 0x1234
```
{{</ details>}}

## cli firmware
* A dongle flashed with `cli` firmware can be attached to the serial console and offer a command line interpreter that helps with testing

build and flash the cli example
```bash
>cd "examples\thread\cli\ftd\usb\pca10059\mbr\armgcc\Makefile"
>make
>make flash_mbr
>make flash
```
on the cli

```bash
>panid 0x1234
>panid
>channel 13
>channel
>networkname OpenThreadDemo
>networkname
>extpanid 1111111122222222
>masterkey
>ifconfig up
>thread start
>state
```
Note `masterkey` command is for the `Network Key`

at this stage the `state` command should log `child`

wireshark should log the join as follows

{{<image src="/images/thread_sensortag/child_join.png">}}

* it's possible to ping with the router ip

```bash
>ping fe80::c85c:d0c4:1103:31d5
Done
> 16 bytes from fe80:0:0:0:c85c:d0c4:1103:31d5: icmp_seq=2 hlim=64 time=12ms
```

a good exercice is to commission a device through CLI

{{<icon_button href="https://openthread.io/guides/border-router/external-commissioning" text="External commitioning..."  icon="new" >}}

## cli reference
{{<icon_button href="https://github.com/openthread/openthread/blob/master/src/cli/README.md" text="thread cli reference"  icon="github" >}}

{{< details "ncp cli. Click to expand..." >}}
```shell
cu -l /dev/ttyACM0 -s 115200
panid 0xabcd
ifconfig up
thread start
state
ping fd00:0064:0123:4567::0808:0808
router list
```

adresses
```shell
panid
rloc16
eui64
```

{{</ details>}}

# Thread over Zephyr

for installing Zephyr see 
{{<icon_button relref="/docs/frameworks/zephyr/" text="Installing Zephyr"  >}}

## building cli
* add `CONFIG_BOARD_HAS_NRF5_BOOTLOADER=n` to `overlay-minimal_singleprotocol.conf`
* Not yet successfull with the nRF52 dongle due to the missing usb overlay

```bash
source ~/ncs/zephyr/zephyr-env.sh
cd ~/ncs/nrf/samples/openthread/cli
west build -b nrf52840dongle_nrf52840 -- -DCONF_FILE="prj.conf overlay-minimal_singleprotocol.conf"
```

