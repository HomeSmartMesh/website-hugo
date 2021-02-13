---
title: "Thread"
description: ""
lead: ""
date: 2020-10-06T08:48:57+00:00
lastmod: 2020-10-06T08:48:57+00:00
draft: false
images: []
menu: 
  docs:
    parent: "networks"
weight: 3
toc: true
---

{{< svg-pan-zoom "/images/openthread-environment-1.1.svg" >}}

* some elements have link to the corresponding websites

# Concepts

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

# Documentation

{{<icon_button href="https://www.threadgroup.org/ThreadSpec" text="request the specification..."  icon="new" >}}

{{<icon_button href="https://infocenter.nordicsemi.com/topic/struct_sdk/struct/sdk_thread_zigbee_latest.html" text="nRF SDK for Thread and Zigbee v4.1.0..." icon="new" >}}

{{<icon_button href="https://www.nordicsemi.com/Software-and-tools/Software/nRF5-SDK-for-Thread-and-Zigbee/Download#infotabs" text="download the SDK" icon="new" >}}

# Tools

{{<icon_button href="https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Connect-for-desktop/Download#infotabs" text="nRF Connect for desktop..."  icon="new" >}}

{{<icon_button href="https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Command-Line-Tools/Download#infotabs" text="nRF Command Line Tools..."  icon="new" >}}

{{<icon_button href="https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads/7-2018-q2-update" text="GNU Arm Embedded Toolchain v7-2018-q2"  icon="new" >}}

# devices
* border router : dongle connected to the raspberry pi

* test cli rfserv : command line dongle through serial monitor

* mqtt-sn : test device with mqtt-sn

* [Topology Monitor](https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Thread-topology-monitor) : GUI interface

* [Wireshark sniffer](https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Sniffer-for-Bluetooth-LE) : dongle that captures all frames and shows them on wireshark

# Boarder Router
## Nordic - Firmware

build and flash the ncp example
```bash
>cd "examples\thread\ncp\ftd\usb\pca10059\mbr\armgcc\Makefile"
>make
>make flash_mbr
>make flash
```
## Nordic - Raspi ready image

* download a ready raspberry pi image

{{<icon_button href="https://www.nordicsemi.com/Software-and-tools/Software/nRF5-SDK-for-Thread-and-Zigbee/Download#infotabs" text="Download the nRF5 SDK for Thread and Zigbee..."  icon="new" >}}

## Open Thread - Firmware

Available pre-build firmware from [this page](https://openthread.io/platforms/co-processor/firmware) is the old version no longer compatible and results in this error :
```bash
a82cab962603 otbr-agent[196]: [CRIT]-PLAT----: Init() at ../../third_party/openthread/repo/src/lib/spinel/radio_spinel_impl.hpp:255: RadioSpinelIncompatible
```

{{<icon_button href="https://openthread.io/platforms/co-processor/firmware#download_nrf52840_firmware_image" text="nRF52840 ncp firmare..."  icon="new" >}}

  nrfjprog -f nrf52 --program ot-ncp-ftd-gd81d769e-nrf52840.hex --sectorerase --verify

therefore, as explained in [this ticket](https://github.com/openthread/ot-br-posix/issues/642), rcp not ncp has to be built and used 

{{< hint warning >}}I could not find build instructions, e.g. missing tools 'aclocal',... the 'OpenThread' BR is at the moment not yet tested
{{</ hint >}}

## OpenThread - Raspi docker

```bash
docker run --sysctl "net.ipv6.conf.all.disable_ipv6=0 \
        net.ipv4.conf.all.forwarding=1 net.ipv6.conf.all.forwarding=1" \
        -p 8080:80 --dns=127.0.0.1 -it --volume \
        /dev/ttyACM0:/dev/ttyACM0 --privileged openthread/otbr \
        --radio-url spinel+hdlc+uart:///dev/ttyACM0
```

{{<icon_button href="https://openthread.io/guides/border-router/docker/run" text="More details on 'Run OTBR Docker'..."  icon="new" >}}

## nRF Connect
{{<icon_button href="http://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/gs_assistant.html" text="nRF Connect SDK installing..."  icon="new" >}}

* install `nRF Connect for deskop`
* From within nRF connect, install `Toolchain Manager`
* From the Toolchain manager, install the last `nRF Connect SDK`

## Form a network

Upon success you should be able to connect on the raspberry pi url `http://10.0.0.41/`

{{<image src="/images/thread_sensortag/OTBR_server.png">}}

* on the left menu click on `Join` then select the required paramters (e.g. Channel,...) then click on `FORM` 
## wireshark sniffing

  git clone https://github.com/NordicSemiconductor/nRF-Sniffer-for-802.15.4

* Flash the `nrf802154_sniffer_dongle.hex`

When Forming a network, some Pakets can be sniffed including advertisment

{{<image src="/images/thread_sensortag/wireshark_startup.png">}}

## test with cli

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

## MQTT-SN
* MQTT-SN actually has nothing to do with `Thread`, it is rather a protcol for a bridge that allows clients to interact with an MQTT Broquer with minimal payload and packets transfers, therefore, it is well adapted to ip over low power mesh networks such as Thread.

Firmware example from Nordic SDK `examples\thread\mqttsn_sleepy_publisher`

{{<icon_button href="https://www.oasis-open.org/committees/download.php/66091/MQTT-SN_spec_v1.2.pdf" text="MQTT-SN_spec_v1.2.pdf"  icon="new" >}}

useful commands
```bash
sudo systemctl status paho-mqttsn-gateway.service
sudo /usr/sbin/MQTT-SNGateway -f /etc/paho-mqtt-sn-gateway.conf
sudo nano /etc/paho-mqtt-sn-gateway.conf
```
Test vector
```bash
#send (Length 3)(MsgType SEARCHGW=1)(Radius 1)
echo -n -e "030101" | xxd -r -p | nc -u fd11:1111:1122:0:98bf:60c7:9431:ee90 47193
```

### MQTT-SN Gateway data flow

{{< svg-pan-zoom "/images/thread_sensortag/mqtt-sn gateway.svg" >}}


# Useful commands

## flash

```cmd
nrfjprog -f NRF52 --chiperase --program <path to the hex file> --reset
```

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

