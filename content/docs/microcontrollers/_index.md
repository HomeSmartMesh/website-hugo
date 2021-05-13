---
title: "Microcontrollers"
description: "collection of Microcontroller boards, ready made and custom made for ESP32, nRF52, STM32,..."
bookFlatSection: true
bookCollapseSection: true
bookToC: false
weight: 2
grid:
    filters:
    - All
    - Board
    - Display
    - Camera
    - Program
    - ESP32
    - RISC-V
    - NRF52
    - STM32
    slot: 300
    items:
    - name: "Ultra Wide Band DWM1001 dev"
      width: 300
      tags: [Board,NRF52]
    - name: "Thread SensorTag"
      width: 200
      tags: [Board,NRF52,Thread]
    - name: "Simple Mesh SensorTag"
      width: 200
      tags: [Board,NRF52,SimpleMesh]
    - name: "ttgo t-display"
      width: 200
      tags: [Board,ESP32,Display]
    - name: "m5 stick C Plus"
      width: 200
      tags: [Board,ESP32,Display]
    - name: "Timer-Cam 3MP"
      width: 200
      tags: [Board,ESP32,Camera]
    - name: "ttgo t5 e-paper"
      width: 200
      tags: [Board,ESP32,Display]
    - name: "Thermostat Control"
      width: 200
      tags: [Program,Display,Buttons]
    - name: "m5 stick V"
      width: 200
      tags: [Board,RISC-V,Camera,Display]
    - name: "Black pill"
      width: 200
      tags: [Board,STM32]
    - name: "Dev EBox F407"
      width: 200
      tags: [Board,STM32]
    - name: "nRF52 USB dongle"
      width: 200
      tags: [Board,NRF52]
    - name: "Blue pill"
      width: 200
      tags: [Board,STM32]
    - name: "nRF52832 UART"
      width: 200
      tags: [Board,NRF52]
---
{{<grid "esp32" >}}
{{< image src="/images/uwb/DWM1001 DevKit.png" href="/docs/microcontrollers/nrf52/dwm1001_dev" width="150px" >}}
* nRF52832
* UWB DWM1001C Module
{{< button relref="/docs/microcontrollers/nrf52/dwm1001_dev" >}}details...{{</button>}}
<--->
{{< image src="/images/thread_sensortag/concept.png" href="/docs/microcontrollers/nrf52/thread_sensortag" width="300px" >}}
{{< button relref="/docs/microcontrollers/nrf52/thread_sensortag" >}}details...{{</button>}}
<--->
{{< image src="/images/simplemesh_sensortag.png" href="/docs/microcontrollers/nrf52/simplemesh_sensortag" width=300 >}}
{{< button relref="/docs/microcontrollers/nrf52/simplemesh_sensortag" >}}details...{{</button>}}
<--->
{{< image src="/images/ttgo_t-display.png" width=200 >}}
* LCD ST7789V
* 1,14 Zoll
* 135x240
* Charger : TP4054
* [more details](/docs/microcontrollers/esp32/ttgo-t-display)
<--->
{{< image src="/images/m5_stick_plus.png" width=200 >}}
* 6-Axis IMU
* IR transmitter
* Microphone
* RTC
* LCD (1.14 inch)
* Lipo Battery
* Passive Buzzer
* [more details](/docs/microcontrollers/esp32/m5stick-plus)
<--->
{{< figure src="/images/fisheye-cam-ov36.png" width=200 >}}
* 3 Mega Pixels OV3660
* Fisheye lens DFOV 120°
* Ultra low power, sleep current 2μA
* 270mAh battery
* PSRAM 8MB
* [more details](/docs/microcontrollers/esp32/fisheye-cam-ov36)

<--->
{{< figure src="/images/ttgo_t5-epaper.png" width=300 >}}
* GxGDEW029Z10
* 2,9 Zoll
* White / Black / Red
* 296x128
* [more details](/docs/microcontrollers/esp32/ttgo-t5-epaper)

<--->
{{<image src="/images/esp32/display-control-app-all info.png" width=300 >}}
* Boards : [ttgo-t-display](/docs/microcontrollers/esp32/ttgo-t-display)
* Platformio
* MQTT
* Json
* Buttons Temperature up/down
* [more details](/docs/microcontrollers/esp32/ttgo-t-display/#thermostat-control)

<--->
{{< figure src="/images/m5stick_riscv.png" width=300 >}}
* [Kendryte K210](https://canaan.io/product/kendryteai)
    * RISC-V RV64IMAFDC Dual-Core 64-bit 400 MHz
    * KPU Neural Processor
* No wifi
* [Product page](https://m5stack.com/collections/m5-core/products/stickv)

<--->
{{<image src="/images/stm32_blackpill.png" height=200 >}}
* [STM32F411CE](https://www.st.com/en/microcontrollers-microprocessors/stm32f411ce.html)
* ARM M4 100 MHz
* 512 KB ROM - 128 KB RAM
* USB 2 Full Speed (device host otg)

<--->
{{< figure src="/images/STM32F407.png" width=300 >}}
* [STM32F407VG](https://www.st.com/en/microcontrollers-microprocessors/stm32f407vg.html)
* ARM M4 168 MHz
* 1024 KB ROM - 192 KB RAM
* USB 2 High Speed with DMA (device host otg)
* [Product Page](https://stm32-base.org/boards/STM32F407VGT6-STM32F4XX-M.html)

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
{{<image src="/images/stm32_bluepill.png" height=150 >}}
* [STM32F103C8](http://www.st.com/en/microcontrollers/stm32f103c8.html)
* ARM M3 72 MHz
* 64 KB ROM - 20 KB RAM
* USB 2 Full Speed

### Applications
* [Expansion boards Hackaday project](https://hackaday.io/project/21396-stm32-blue-pill-iot-expansion-boards)
* [MQTT 8 Channels dimmer](https://hackaday.io/project/28678-stm32-bluepill-x8-edison-bulbs-with-nrf-and-mqtt)

<--->


{{< figure src="/images/nRF52-uart.png" width=200 >}}
* [nRF52832](https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF52832)
* ARM M4 64 MHz
* 512 KB ROM - 64 KB RAM
* Bluetooth Low Energy, mesh
### Application
* [custom mesh to coordinator](https://github.com/nRFMesh/nRF52_Mesh#04-uart-dongle)
{{</grid>}}
