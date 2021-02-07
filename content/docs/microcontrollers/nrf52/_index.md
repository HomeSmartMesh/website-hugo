---
weight: 4
bookFlatSection: true
bookCollapseSection: false
title: "nRF52"
grid:
    filters:
    - All
    - Board
    slot: 300
    items:
    - name: "nRF52840 Dongle"
      width: 200
      tags: [Board,USB]
    - name: "nRF52823 UART"
      width: 200
      tags: [Board,UART]
---

{{<grid "mygrid">}}

{{< image src="/images/nRF52840-Dongle.jpg" width=300 >}}

* [nRF52840](https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF52840)
* ARM M4 64 MHz
* 1024 KB ROM - 256 KB RAM
* Bluetooth 5, Mesh
* 802.15.4, Thread, Zigbee
* USB 2
* [Product Page](https://www.nordicsemi.com/Software-and-tools/Development-Kits/nRF52840-Dongle/GetStarted)
### Application

{{< button relref="/docs/networks/nrf#08-usb-dongle-nrf52840-dongle" >}}custom mesh coordinator{{</button>}}
<--->
{{< image src="/images/nRF52-uart.png" width=200 >}}

* [nRF52832](https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF52832)
* ARM M4 64 MHz
* 512 KB ROM - 64 KB RAM
* Bluetooth Low Energy, mesh
### Application

{{< button relref="/docs/networks/nrf#04-uart-dongle" >}}custom mesh coordinator{{</button>}}
{{< /grid >}}

# nRF52840 Dongle

## Serial wire debug pinout

{{< image src="/images/thread_sensortag/serial_wire_debug.png" >}}

## pogo pin adapter

{{< model_viewer "/models/nrf/pogo_adapter_swd.glb" "400" >}}

{{< new_button text="Download STL models zip" href="/models/nrf/nRF52840_usb_pogo_top.zip">}}

{{< image src="/images/thread_sensortag/pogo_pin_adapter.png" >}}

