---
title: "Project CHIP"
description: "Connected Home Over IP"
date: 2021-02-12T08:00:00+00:00
lastmod: 2021-02-12T08:00:00+00:00
weight: 2
toc: true
---
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

# nRF Connect integration
{{<image src="/images/thread_sensortag/chip_nrfconnect_overview_simplified.svg" >}}

The above diagram taken from the CHIP repo link in the button below, shows the integration of a CHIP application on top of the nRF Connect SDK.

The MPSL is only required if needed to combine both bluetooth and Thread. Also the softDevice is only needed for bluetooth but not for Thread.
{{< icon_button href="https://github.com/project-chip/connectedhomeip/blob/master/docs/guides/nrfconnect_platform_overview.md" text="Reference in github CHIP repo" icon="github" >}}


# Linux
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

## build lighting app

```bash
cd connectedhomeip/examples/lighting-app/nrfconnect
west build -b nrf52840dongle_nrf52840
west flash
nrfjprog --program build/zephyr/zephyr.hex -f NRF52
```
configure
```bash
west build -b nrf52840dongle_nrf52840 -t menuconfig
```

## build default
{{< icon_button href="https://github.com/project-chip/connectedhomeip/blob/master/docs/BUILDING.md" text="How to" icon="github" >}}

```bash
git clone --recurse-submodules https://github.com/project-chip/connectedhomeip.git
sudo apt-get install git gcc g++ python pkg-config libssl-dev libdbus-1-dev libglib2.0-dev libavahi-client-dev ninja-build python3-venv python3-dev unzip
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

# Windows

{{< icon_button href="https://github.com/project-chip/connectedhomeip/blob/master/examples/lighting-app/nrfconnect/README.md" text="How to" icon="github" >}}

* clone connectedhomeip repo
* install nRF Connect SDK v1.4.0
* configure the toolchain
  * `ZEPHYR_TOOLCHAIN_VARIANT` to `gnuarmemb`
  * `GNUARMEMB_TOOLCHAIN_PATH` to `D:\tools\gnu_arm_embedded\9_2020-q2-update`

```cmd
cd connectedhomeip\examples\lighting-app\nrfconnect
"D:\tools\nrf\v1.4.0\zephyr\zephyr-env.cmd"
west build -b nrf52840dongle_nrf52840

```
{{<details "build error on windows">}}
```cmd
D:\Projects\connectedhomeip\examples\lighting-app\nrfconnect>west build -b nrf52840dongle_nrf52840
-- west build: generating a build system
Including boilerplate (Zephyr base): D:/tools/nrf/v1.4.0/zephyr/cmake/app/boilerplate.cmake
-- Application: D:/Projects/connectedhomeip/examples/lighting-app/nrfconnect
-- Using NCS Toolchain 1.4.0 for building. (D:/tools/nrf/v1.4.99-dev1/toolchain/cmake)
-- Zephyr version: 2.4.0 (D:/tools/nrf/v1.4.0/zephyr)
-- Found Python3: D:/tools/nrf/v1.4.99-dev1/toolchain/opt/bin/python.exe (found suitable exact version "3.8.2") found components: Interpreter
-- Found west (found suitable version "0.7.2", minimum required is "0.7.1")
CMake Error at D:/tools/nrf/v1.4.0/zephyr/cmake/zephyr_module.cmake:48 (message):

  D:/Projects/connectedhomeip/examples/lighting-app/nrfconnect/third_party/connectedhomeip/config/nrfconnect/chip-module,
  given in ZEPHYR_EXTRA_MODULES, is not a valid zephyr module

Call Stack (most recent call first):
  D:/tools/nrf/v1.4.0/zephyr/cmake/app/boilerplate.cmake:150 (include)
  D:/tools/nrf/v1.4.0/zephyr/share/zephyr-package/cmake/ZephyrConfig.cmake:24 (include)
  D:/tools/nrf/v1.4.0/zephyr/share/zephyr-package/cmake/ZephyrConfig.cmake:35 (include_boilerplate)
  CMakeLists.txt:26 (find_package)


-- Configuring incomplete, errors occurred!
FATAL ERROR: command exited with status 1: 'C:\Program Files\CMake\bin\cmake.EXE' '-DWEST_PYTHON=c:\users\user\appdata\local\programs\python\python39\python.exe' '-BD:\Projects\connectedhomeip\examples\lighting-app\nrfconnect\build' '-SD:\Projects\connectedhomeip\examples\lighting-app\nrfconnect' -GNinja -DBOARD=nrf52840dongle_nrf52840
```
{{</details>}}