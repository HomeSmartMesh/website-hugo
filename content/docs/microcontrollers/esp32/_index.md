---
weight: 1
bookFlatSection: true
bookCollapseSection: true
BookToC: false
title: "esp32"
grid:
    filters:
    - All
    - Board
    - Display
    - Camera
    - LowPower
    - Program
    slot: 250
    items:
    - name: "ttgo t-display"
      width: 200
      tags: [Board,Display]
    - name: "m5 stick C Plus"
      width: 200
      tags: [Board,Display]
    - name: "fisheye cam OV3660"
      width: 200
      tags: [Board,Camera,LowPower]
    - name: "FireBeetle"
      width: 200
      tags: [Board,LowPower]
    - name: "m5 atom lite"
      width: 200
      tags: [Board]
    - name: "ttgo t5 e-paper"
      width: 200
      tags: [Board,Display]
    - name: "Thermostat Control"
      width: 200
      tags: [Program,Display,Buttons]
---
{{<grid "esp32" >}}
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
{{<image src="/images/esp32/timer-cam-3mp.png" href="/docs/microcontrollers/esp32/fisheye-cam-ov36" width=200 >}}
* 3 Mega Pixels OV3660
* Fisheye lens DFOV 120°
* Ultra low power, sleep current 2μA
* 270mAh battery
* PSRAM 8MB
* [more details](/docs/microcontrollers/esp32/fisheye-cam-ov36)
<--->
{{< image src="/images/esp32/firebeetle_board.png" width=200 >}}
* 10 uA deep sleep current
* USB and external DC
* TP4056 Li-Ion battery charger
* [more details](/docs/microcontrollers/esp32/fire-beetle)

<--->
{{< image src="/images/esp32/M5-Atom-Lite.webp" href="/docs/microcontrollers/esp32/m5stack-atom" width=150 >}}
* Tiny 24 x 24 x 10 mm
* User Button + Reset
* RGB Led SK6812
* IR Led
* [more details](/docs/microcontrollers/esp32/m5stack-atom)

<--->
{{< figure src="/images/ttgo_t5-epaper.png" width=300 >}}
* GxGDEW029Z10
* 2,9 Zoll
* White / Black / Red
* 296x128
* [more details](/docs/microcontrollers/esp32/ttgo-t5-epaper)

<--->
{{< figure src="/images/esp32/display-control-app-all info.png" width=300 >}}
* Boards : [ttgo-t-display](/docs/microcontrollers/esp32/ttgo-t-display)
* Platformio
* MQTT
* Json
* Buttons Temperature up/down
* [more details](/docs/microcontrollers/esp32/ttgo-t-display/#thermostat-control)

{{</grid>}}


