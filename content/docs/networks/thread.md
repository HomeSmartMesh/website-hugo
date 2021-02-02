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

# devices
* border router : dongle connected to the raspberry pi

* test cli rfserv : command line dongle through serial monitor

* mqtt-sn : test device with mqtt-sn

* [Topology Monitor](https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Thread-topology-monitor) : GUI interface

* [Wireshark sniffer](https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Sniffer-for-Bluetooth-LE) : dongle that captures all frames and shows them on wireshark

# Useful commands

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

