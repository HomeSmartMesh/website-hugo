---
title: "Fisheye Cam OV3660"
description: "ESP32 timer camera OV3660 with low power sleep"
date: 2020-12-26T00:00:00+09:00
draft: false
weight: 11
---

![img](/images/fisheye-cam-ov36.png)

* 3 Mega Pixels OV3660
* Fisheye lens DFOV 120°
* Ultra low power, sleep current 2μA
* 270mAh battery
* PSRAM 8MB

## Product page
* [Fisheye Timer camera $20](https://m5stack.com/collections/m5-unit/products/esp32-psram-timer-camera-fisheye-ov3660)

## Platformio porting
### board definition
* local file : `boards\m5stack-timer-cam.json`
* should be added to the [platformio/platform-espressif32](https://github.com/platformio/platform-espressif32/tree/master/boards) repo
* still missing from v2.1.0

### pins declaration
* file required `pins_arduino.h`
* available on [pins_arduino.h@1.0.5](https://github.com/espressif/arduino-esp32/blob/master/variants/m5stack_timer_cam/pins_arduino.h) not yet released

## Applications
### wakeup
* [m5stack/Arduino wakeup example](https://github.com/m5stack/TimerCam-arduino/tree/master/examples/wakeup)
* [sep32home/Platformio wakeup example](https://github.com/ESP32Home/timercam_wakeup)

{{< details "Build info" "..." >}}
```log
PACKAGES:
 - framework-arduinoespressif32 0.0.0+sha.b05bdf6
 - tool-esptoolpy 1.30000.201119 (3.0.0)
 - toolchain-xtensa32 2.50200.80 (5.2.0)
Dependency Graph
|-- <Timer-CAM> 0.0.1
```
{{< /details >}}


### webcam
* [m5stack/Arduino webcam example](https://github.com/m5stack/TimerCam-arduino/tree/master/examples/web_cam)
* [esp32home/Platformio webcam example](https://github.com/ESP32Home/timercam_webcam)


{{< details "Build info" "..." >}}
Note `app_httpd` is part of the `Timer-CAM` lib
```log
PACKAGES:
 - framework-arduinoespressif32 0.0.0+sha.b05bdf6
 - tool-esptoolpy 1.30000.201119 (3.0.0)
 - tool-mkspiffs 2.230.0 (2.30)
 - toolchain-xtensa32 2.50200.80 (5.2.0)
Dependency Graph
|-- <Timer-CAM> 0.0.1   
|-- <WiFi> 1.0
```
{{< /details >}}
