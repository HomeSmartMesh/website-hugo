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



# Building for nRF52
## nRF Connect integration
{{<image src="/images/thread_sensortag/chip_nrfconnect_overview_simplified.svg" >}}

The above diagram taken from the CHIP repo link in the button below, shows the integration of a CHIP application on top of the nRF Connect SDK.

The MPSL is only required if needed to combine both bluetooth and Thread. Also the softDevice is only needed for bluetooth but not for Thread.
{{< icon_button href="https://github.com/project-chip/connectedhomeip/blob/master/docs/guides/nrfconnect_platform_overview.md" text="Reference in github CHIP repo" icon="github" >}}

## prerequisites
{{< icon_button href="https://github.com/project-chip/connectedhomeip/blob/master/docs/BUILDING.md" text="How to" icon="github" >}}

```bash
git clone --recurse-submodules https://github.com/project-chip/connectedhomeip.git
sudo apt-get install git gcc g++ python pkg-config libssl-dev libdbus-1-dev libglib2.0-dev libavahi-client-dev ninja-build python3-venv python3-dev unzip
cd connectedhomeip
source scripts/activate.sh
gn gen out/custom
gn args --list out/custom
```
important arguments :
* chip_device_platform : esp32, freertos, linux, nrfconnect,...
* chip_enable_openthread
* chip_enable_wifi
* target_cpu
* target_os

## install
* Step 1, install Zephyr
{{<icon_button relref="/docs/frameworks/zephyr/" text="Install Zephyr"  >}}

* Step 2, install the toolchain
{{<icon_button href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/gs_installing.html#installing-the-toolchain" text="install the toolchain" icon="new" >}}

```bash
nano ~/.zephyrrc
export ZEPHYR_TOOLCHAIN_VARIANT=gnuarmemb
export GNUARMEMB_TOOLCHAIN_PATH=~/gnuarmemb/gcc-arm-none-eabi-9-2019-q4-major/
```

* Step 2, install nRF Connect SDK
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

```bash
cd connectedhomeip/examples/lighting-app/nrfconnect
west build -b nrf52840dongle_nrf52840 -- -DCONF_FILE="prj.conf"
west build -b nrf52840dk_nrf52840 -- -DCONF_FILE="prj.conf"
west flash
nrfjprog --program build/zephyr/zephyr.hex -f NRF52
```
configure
```bash
west build -b nrf52840dongle_nrf52840 -t menuconfig
```
{{<details "Build Error">}}
```bash
CMake Error at /usr/share/cmake-3.16/Modules/ExternalProject.cmake:3182 (add_custom_target):
  add_custom_target cannot create target "chip-gn" because another target
  with the same name already exists.  The existing target is a custom target
  created in source directory
  "/home/wass/connectedhomeip/config/nrfconnect/chip-module".  See
  documentation for policy CMP0002 for more details.
Call Stack (most recent call first):
  /home/wass/ncs/modules/lib/connectedhomeip/config/nrfconnect/chip-module/CMakeLists.txt:244 (ExternalProject_Add)


CMake Error at /home/wass/ncs/zephyr/cmake/extensions.cmake:570 (add_library):
  add_library cannot create target "chip" because another target with the
  same name already exists.  The existing target is an interface library
  created in source directory
  "/home/wass/connectedhomeip/config/nrfconnect/chip-module".  See
  documentation for policy CMP0002 for more details.
Call Stack (most recent call first):
  /home/wass/ncs/modules/lib/connectedhomeip/config/nrfconnect/chip-module/CMakeLists.txt:268 (zephyr_interface_library_named)


CMake Warning at /home/wass/ncs/zephyr/CMakeLists.txt:1349 (message):
  __ASSERT() statements are globally ENABLED


CMake Warning at /home/wass/ncs/zephyr/CMakeLists.txt:1377 (message):


        The CMake build type was set to 'Debug', but the optimization flag was set to '-Os'.
        This may be intentional and the warning can be turned off by setting the CMake variable 'NO_BUILD_TYPE_WARNING'


-- Configuring incomplete, errors occurred!
See also "/home/wass/connectedhomeip/examples/lighting-app/nrfconnect/build/CMakeFiles/CMakeOutput.log".
See also "/home/wass/connectedhomeip/examples/lighting-app/nrfconnect/build/CMakeFiles/CMakeError.log".
FATAL ERROR: command exited with status 1: /usr/bin/cmake -DWEST_PYTHON=/usr/bin/python3 -B/home/wass/connectedhomeip/examples/lighting-app/nrfconnect/build -S/home/wass/connectedhomeip/examples/lighting-app/nrfconnect -GNinja -DBOARD=nrf52840dongle_nrf52840
```
{{</details>}}

# Building for ESP32

[all clusters app](https://github.com/project-chip/connectedhomeip/tree/master/examples/all-clusters-app/esp32) example
