---
title: "nRF52840 USB dongle"
description: "Nordic's official dev kit PCA10059 in a form of a usb dongle for the nRF52840"
images: ["/images/nrf_usb_dongle.png"]
date: 2021-02-14T08:00:00+00:00
lastmod: 2021-02-14T08:00:00+00:00
weight: 3
toc: true
---
{{<load-model_viewer>}}
{{<load-photoswipe >}}

{{<icon_button text="Product Page" href="https://www.nordicsemi.com/Software-and-tools/Development-Kits/nRF52840-Dongle/" icon="new">}}

{{<gfigure src="/images/nrf_usb_dongle.png" >}}

* [nRF52840](https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF52840)
* ARM M4 64 MHz
* 1024 KB ROM - 256 KB RAM
* Bluetooth 5, Mesh
* 802.15.4, Thread, Zigbee
* USB 2
GetStarted)
### Applications
* [custom mesh coordinator](https://github.com/nRFMesh/nRF52_Mesh#08-usb-dongle-nrf52840-dongle)

# nRF52840 Dongle

## Pinout

### Serial Wire Debug
* the usb dongle is referred to as `PCA10059` which in projects usually shifts the app to offset `0x1000` as openbootloader usage is expected
* With serial wire debug, it's possible to flash applications at same address as the `PCA10056` which is at address 0x0000
* recovering the openbootloader mode would then require to reflash the MBR with any nRF SDK project `make flash_mbr`

{{< image src="/images/thread_sensortag/serial_wire_debug.png" >}}

### LEDS

* LED1 Green : `P0.06`
* LED2 Green : `P0.08`
* LED2 Green : `P1.09`
* LED2 Blue  : `P0.12`


## pogo pin adapter

{{< model_viewer "/models/nrf/pogo_adapter_swd.glb" "400" >}}

{{<icon_button text="STL models zip" href="/models/nrf/nRF52840_usb_pogo_top.zip" icon="download">}}

## micro python
{{<icon_button text="build micro python" relref="/docs/frameworks/upython/#config" >}}

## gallery
{{< image src="/images/thread_sensortag/pogo_pin_adapter.png" >}}

