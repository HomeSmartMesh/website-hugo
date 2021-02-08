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

{{< my_button href="https://openthread.io/" text="Open Thread" >}}

{{< svg-pan-zoom "/images/openthread-environment-1.1.svg" >}}

* some elements have link to the corresponding websites

# Concepts

{{< new_button href="https://openthread.io/guides/thread-primer/ipv6-addressing" text="IPV6 adressing" >}}

{{< new_button href="https://openthread.io/guides/thread-primer/network-discovery" text="Network discovery" >}}

{{< new_button href="https://openthread.io/guides/border-router/docker/test-connectivity" text="test connectivity" >}}

# Documentation

{{<new_button href="https://www.threadgroup.org/ThreadSpec" text="request the specification..." >}}

{{<new_button href="https://infocenter.nordicsemi.com/topic/struct_sdk/struct/sdk_thread_zigbee_latest.html" text="nRF SDK for Thread and Zigbee v4.1.0..." >}}

{{<new_button href="https://www.nordicsemi.com/Software-and-tools/Software/nRF5-SDK-for-Thread-and-Zigbee/Download#infotabs" text="download the SDK">}}

# Tools

{{<new_button href="https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Connect-for-desktop/Download#infotabs" text="nRF Connect for desktop..." >}}

{{<new_button href="https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Command-Line-Tools/Download#infotabs" text="nRF Command Line Tools..." >}}

{{<new_button href="https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads/7-2018-q2-update" text="GNU Arm Embedded Toolchain v7-2018-q2" >}}

# devices
* border router : dongle connected to the raspberry pi

* test cli rfserv : command line dongle through serial monitor

* mqtt-sn : test device with mqtt-sn

* [Topology Monitor](https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Thread-topology-monitor) : GUI interface

* [Wireshark sniffer](https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Sniffer-for-Bluetooth-LE) : dongle that captures all frames and shows them on wireshark

# Boarder router

## ncp for nRF service
{{<new_button href="https://www.nordicsemi.com/Software-and-tools/Software/nRF5-SDK-for-Thread-and-Zigbee/Download#infotabs" text="Download the nRF5 SDK for Thread and Zigbee..." >}}

build and flash the ncp example
```bash
>cd "examples\thread\ncp\ftd\usb\pca10059\mbr\armgcc\Makefile"
>make
>make flash
```
* connect the USB dongle with the ncp firmware on the raspberry pi
* on the raspberry pi run

## ncp for openthread service

{{<new_button href="https://openthread.io/platforms/co-processor/firmware#download_nrf52840_firmware_image" text="nRF52840 ncp firmare..." >}}

```bash
docker run --sysctl "net.ipv6.conf.all.disable_ipv6=0 \
        net.ipv4.conf.all.forwarding=1 net.ipv6.conf.all.forwarding=1" \
        -p 8080:80 --dns=127.0.0.1 -it --volume \
        /dev/ttyACM0:/dev/ttyACM0 --privileged openthread/otbr \
        --radio-url spinel+hdlc+uart:///dev/ttyACM0
```

{{<new_button href="https://openthread.io/guides/border-router/docker/run" text="More details on 'Run OTBR Docker'..." >}}

Upon success you should be able to connect on the raspberry pi url `http://10.0.0.53:8080/`

{{<image src="/images/thread_sensortag/OTBR_server.png">}}

After trying forming a network and on not success such error might appear

  otbr-agent[196]: [CRIT]-PLAT----: Init() at ../../third_party/openthread/repo/src/lib/spinel/radio_spinel_impl.hpp:255: RadioSpinelIncompatible

## wireshark sniffing

  git clone https://github.com/NordicSemiconductor/nRF-Sniffer-for-802.15.4

* Flash the `nrf802154_sniffer_dongle.hex`



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
{{< new_button href="https://github.com/openthread/openthread/blob/master/src/cli/README.md" text="thread cli reference" >}}

{{< details "ncp cli. Click to expand..." >}}
```shell
cu -l /dev/ttyACM0 -s 115200

panid 0xabcd

ifconfig up

thread start

state

ping fd00:0064:0123:4567::0808:0808
```
{{</ details>}}

