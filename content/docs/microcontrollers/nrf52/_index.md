---
weight: 4
bookFlatSection: true
bookCollapseSection: false
title: "nRF52"
grid:
    slot: 300
    filters:
    - All
    - nRF52840
    - nRF52832
    items:
    - name: "Thread SensorTag"
      width: 200
      tags: [Board,nRF52840,Thread]
    - name: "Simple Mesh SensorTag"
      width: 200
      tags: [Board,nRF52832,SimpleMesh]
    - name: "nRF52840 Dongle"
      width: 200
      tags: [Board,USB,nRF52840]
    - name: "nRF52823 UART"
      width: 200
      tags: [Board,USB,nRF52832]
---

{{<grid "mygrid">}}
{{< image src="/images/thread_sensortag/concept.png" href="/docs/microcontrollers/nrf52/thread_sensortag" width="300px" >}}
{{< button relref="/docs/microcontrollers/nrf52/thread_sensortag" >}}details...{{</button>}}

<--->
{{< image src="/images/simplemesh_sensortag.png" href="/docs/microcontrollers/nrf52/simplemesh_sensortag" width=300 >}}
{{< button relref="/docs/microcontrollers/nrf52/simplemesh_sensortag" >}}details...{{</button>}}

<--->
{{< image src="/images/nrf_usb_dongle.png" width=300 href="/docs/microcontrollers/nrf52/usb_dongle" >}}

* board codename : PCA10056
* nRF52840
* ARM M4 64 MHz
* 1024 KB ROM - 256 KB RAM
* Bluetooth 5, Mesh
* 802.15.4, Thread, Zigbee
* USB 2

{{< button relref="/docs/microcontrollers/nrf52/usb_dongle" >}}details...{{</button>}}
<--->
{{< image src="/images/nRF52-uart.png" width=200 >}}

* [nRF52832](https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF52832)
* ARM M4 64 MHz
* 512 KB ROM - 64 KB RAM
* Bluetooth Low Energy, mesh
### Application

{{< button relref="/docs/networks/nrf#04-uart-dongle" >}}custom mesh coordinator{{</button>}}
{{< /grid >}}
