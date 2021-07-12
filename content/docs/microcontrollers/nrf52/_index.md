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
    - uwb
    items:
    - name: "Ultra Wide Band DWM1001 dev"
      width: 200
      tags: [Board,nRF52832,uwb]
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
    - name: "Trick-Tracker"
      width: 200
      tags: [Board,USB,nRF52840]
---

{{<grid "mygrid">}}
{{< image src="/images/uwb/DWM1001 DevKit.png" href="/docs/microcontrollers/nrf52/dwm1001_dev" width="300px" >}}
* nRF52832
* UWB DWM1001C Module
{{< button relref="/docs/microcontrollers/nrf52/dwm1001_dev" >}}details...{{</button>}}
<--->
{{< image src="/images/thread_sensortag/concept.png" href="/docs/microcontrollers/nrf52/thread_sensortag" width="300px" >}}
* nRF52840
* openthread
* Zephyr
* Light, Temperature, Humidity, Pressure

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

<--->
{{< image src="/images/trick_tracker/title_image.jpg" width=200 href="/docs/microcontrollers/nrf52/trick_tracker">}}

* nRF52840
* ARM M4 64 MHz
* 1024 KB ROM - 256 KB RAM
* Bluetooth
* USB-C
* Accelerometer, Gyroscope, Magnetoscope, Pressure
{{< button relref="/docs/microcontrollers/nrf52/trick_tracker" >}}details...{{</button>}}
{{< /grid >}}
