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


# Building
## default build
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

## building nRF Lighting app

{{< icon_button href="https://github.com/project-chip/connectedhomeip/blob/master/examples/lighting-app/nrfconnect/README.md" text="How to" icon="github" >}}





