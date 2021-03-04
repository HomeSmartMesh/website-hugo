---
title: "Project CHIP"
description: "Connected Home Over IP"
date: 2021-02-12T08:00:00+00:00
lastmod: 2021-02-12T08:00:00+00:00
weight: 2
toc: true
---
# Relates to
{{<icon_button relref="/docs/frameworks/zephyr/" text="Zephyr RTOS" >}}
{{<icon_button relref="/docs/networks/thread/" text="Thread Protocol" >}}
# Overview
## Definition

We can consider the project CHIP `Connected Home Over IP` definition as in two folds :
* a takeover of the Zigbee Cluster Library to be fit over the IP layer.
* The abstraction of all different transport protocols with IP

This will avoid that each transport (e.g. BT, Zigbee) have a custom app layer. It is interesting to notice that the `Zigbee Alliance` is driving this project whereas Zigbee itself does not map to an IP layer, and therefore `Thread` is the natural transition from Zigbee as both are 802.15.4 compliant at the bottom and both will keep the same logic of the ZCL that devices are familiar with.

## Links

{{< icon_button href="https://www.connectedhomeip.com/" text="CHIP website" icon="new" >}}

{{< icon_button href="https://github.com/project-chip/connectedhomeip" text="Github Repo" icon="github" >}}

## Tools

**ZAP** : ZCL Advaned Platform

**ZCL** : Zigbee Cluster Library

{{< icon_button href="https://github.com/project-chip/zap" text="ZAP Repo" icon="github" >}}

on windows install with node `v14.15.1` failed, errors after manuall install of GTK filed an [issue](https://github.com/project-chip/zap/issues/101)

on linux the following commands succeed using node `v14.15.5` :

```bash
  npm uninstall node-pre-gyp --save
  npm install @mapbox/node-pre-gyp --save

  npm install
  npm run zap
```



# Running on nRF52
## nRF Connect integration
{{<image src="/images/thread_sensortag/chip_nrfconnect_overview_simplified.svg" >}}

The above diagram taken from the CHIP repo link in the button below, shows the integration of a CHIP application on top of the nRF Connect SDK.

The MPSL is only required if needed to combine both bluetooth and Thread. Also the softDevice is only needed for bluetooth but not for Thread.
{{< icon_button href="https://github.com/project-chip/connectedhomeip/blob/master/docs/guides/nrfconnect_platform_overview.md" text="Reference in github CHIP repo" icon="github" >}}

{{<hint info>}}
Note that `zephyr` and `connectedhomeip` will be managed as ncs dependencies with west from `nrf/west.yml`
{{</hint>}}

## install
* install nRF Connect SDK
{{<icon_button href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/gs_installing.html" text="install nRF Connect SDK" icon="new" >}}

```bash
mkdir ~/ncs && cd ~/ncs
west init -m https://github.com/nrfconnect/sdk-nrf --mr v1.4.0
west update
west zephyr-export
pip3 install --user -r zephyr/scripts/requirements.txt
pip3 install --user -r nrf/scripts/requirements.txt
pip3 install --user -r bootloader/mcuboot/scripts/requirements.txt
```
then on every new session
```bash
cd ncs
source zephyr/zephyr-env.sh
```
* step 3, from the same link above install gn

## build the lighting app

* Update `prj.conf` params to match the thread border router parametrs (e.g. channel,...)
* add CONFIG_BOARD_HAS_NRF5_BOOTLOADER=n

```bash
cd ncs/modules/lib/connectedhomeip/examples/lighting-app/nrfconnect/
west build -b nrf52840dongle_nrf52840 -t menuconfig
west build -b nrf52840dongle_nrf52840 -- -DCONF_FILE="prj.conf"
west flash
nrfjprog -f nrf52 --program zephyr.hex --sectorerase --verify
```
* in the config below, the RTT logging is enabled to get the log through the SWD Segger J-Link interface, see `# Logging` section
* please note that the show `CONFIG_OPENTHREAD_MASTERKEY` is a dummy key and has to be kept secret in a production environment
{{<details "prj.conf">}}
```conf
#
#    Copyright (c) 2020 Project CHIP Authors
#
#    Licensed under the Apache License, Version 2.0 (the "License");
#    you may not use this file except in compliance with the License.
#    You may obtain a copy of the License at
#
#        http://www.apache.org/licenses/LICENSE-2.0
#
#    Unless required by applicable law or agreed to in writing, software
#    distributed under the License is distributed on an "AS IS" BASIS,
#    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#    See the License for the specific language governing permissions and
#    limitations under the License.
#

# This sample uses sample-defaults.conf to set options common for all
# samples. This file should contain only options specific for this sample
# or overrides of default values.

# Add support for LEDs and buttons on Nordic development kits
CONFIG_DK_LIBRARY=y
CONFIG_PWM=y

# Default OpenThread network settings
CONFIG_OPENTHREAD_PANID=4660
CONFIG_OPENTHREAD_CHANNEL=13
CONFIG_OPENTHREAD_NETWORK_NAME="OpenThreadDemo"
CONFIG_OPENTHREAD_XPANID="11:11:11:11:22:22:22:22"
CONFIG_OPENTHREAD_MASTERKEY="00112233445566778899aabbccddeeff"

# Bluetooth overrides
CONFIG_BT_DEVICE_NAME="ChipLight"

# Additional configs for debbugging experience.
CONFIG_THREAD_NAME=y
CONFIG_MPU_STACK_GUARD=y

# CHIP configuration
CONFIG_CHIP_PROJECT_CONFIG="main/include/CHIPProjectConfig.h"
CONFIG_CHIP_OPENTHREAD_CONFIG="../../platform/nrfconnect/project_include/OpenThreadConfig.h"

CONFIG_BOARD_HAS_NRF5_BOOTLOADER=n

# Logging
CONFIG_SERIAL=n
CONFIG_LOG=y
CONFIG_LOG_BACKEND_RTT=y
CONFIG_LOG_BACKEND_UART=n
CONFIG_BOOT_BANNER=y
CONFIG_USE_SEGGER_RTT=y
CONFIG_CONSOLE=y
CONFIG_UART_CONSOLE=n
CONFIG_RTT_CONSOLE=y
```
{{</details>}}
The log output is then as follows
{{<details "RTT Log output" >}}
```log
I: nRF5 802154 radio initialized
I: 4 Sectors of 4096 bytes
I: alloc wra: 0, ec0
I: data wra: 0, 227
*** Booting Zephyr OS build v2.4.99-ncs1-rc1  ***
I: Init CHIP stack
I: SoftDevice Controller build revision: 
I: e5 c7 9c d9 91 00 1d 66 |.......f
I: ea fb 6e 7b 98 2f 42 0d |..n{./B.
I: f1 60 93 c8             |.`..    
I: Starting CHIP task
I: Init Thread stack
D: 593[DL] CHIP task running
D: 594[DL] In DriveBLEState
I: 614[DL] CHIPoBLE advertising disabled because device is fully provisioned
D: 615[DL] OpenThread State Changed (Flags: 0x0100103d)
D: 616[DL]    Device Role: DETACHED
D: 617[DL]    Thread Unicast Addresses:
D: 618[DL]         fdb0:f51f:154a:11be:0:ff:fe00:f002/64 valid preferred rloc
D: 620[DL]         fdb0:f51f:154a:11be:2294:a147:3648:185d/64 valid preferred
D: 621[DL]         fe80::4b5:84fc:320f:d2/64 valid preferred
I: 830[DL] OpenThread ifconfig up and thread start
I: 832[DL] OpenThread started: OK
I: 832[DL] Setting OpenThread device type to MINIMAL END DEVICE
I: 834[ZCL] Using ZAP con
```
{{</details>}}

# Running on ESP32

[all clusters app](https://github.com/project-chip/connectedhomeip/tree/master/examples/all-clusters-app/esp32) example
