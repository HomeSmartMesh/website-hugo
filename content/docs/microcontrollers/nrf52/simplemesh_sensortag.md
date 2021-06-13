---
title: "Simple Mesh SensorTag"
description: "A Sensor tag to monitor Light brightness with MAX44009, Temperature, Humidity and Pressure with BME280. The SoC is an nRF52832 that communicates through a Simple Mesh Framework"
images: ["/images/simplemesh_sensortag.png"]
date: 2021-02-14T08:00:00+00:00
lastmod: 2021-02-14T08:00:00+00:00
weight: 3
toc: true
---
{{<load-svg-pan-zoom>}}
{{<load-photoswipe >}}

{{<icon_button relref="/docs/networks/nrf/" text="Networks / Simple Mesh" >}}
{{<icon_button relref="/docs/frameworks/raspi_iot/" text="Frameworks / Raspi IoT" >}}
{{<icon_button href="https://www.tindie.com/products/wassfila/nrf52-mesh-sensor-node/" text="Tindie Product Page" icon="new" >}}


# Concept

{{<image src="/images/simplemesh_sensortag.png" >}} 

It is possible to mount either CR2032 or CR2477

{{<image src="/images/mounted.png" >}}

# Hardware
board render

{{< icon_button href="https://github.com/nRFMesh/nRF52_Mesh/tree/master/boards/nrf52_sensortag" text="repo directory" icon="github" >}}

{{< icon_button href="https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF52832" text="nRF52832" icon="new" >}}

{{< icon_button href="https://www.bosch-sensortec.com/products/environmental-sensors/humidity-sensors-bme280/" text="BME280" icon="new" >}}

{{< icon_button href="https://www.maximintegrated.com/en/products/interface/sensor-interface/MAX44009.html" text="MAX44009" icon="new" >}}

## Schematics
{{< svg-pan-zoom "/images/simplemesh_sensortag/schematics.svg" "white" >}}

## Board
### Top

{{< svg-pan-zoom "/images/simplemesh_sensortag/top.svg" "white" >}}
### Bottom
{{< svg-pan-zoom "/images/simplemesh_sensortag/bottom.svg" "white" >}}
### Module
{{< figure src="/images/module_size.png" width="100%" >}}

# Application
## Firmware

{{< icon_button href="https://github.com/nRFMesh/nRF52_Mesh/tree/master/applications/01_sensortag" text="repo directory" icon="github" >}}

### Low Power configuration
|Flags to clear|
--- |
| NRFX_UARTE_ENABLED |
| NRFX_UART_ENABLED | 
| UART_ENABLED | 
| UART0_ENABLED |
| NRF_FPRINTF_ENABLED |
| NRF_LOG_BACKEND_UART_ENABLED |
| NRF_LOG_STR_FORMATTER_TIMESTAMP_FORMAT_ENABLED |
| NRF_LOG_ENABLED |
* removed nrf_drv_uart.c from Makefile
* Required nRF52832 Errata [89] TWI: Static 400 uA current while using GPIOTE

## Low Power Measures
| Mode | Current |
--- | --- |
| RTC + RAM | 9.6 uA |
| // + Sensors | 22 uA |
| // without TWI Woraround | 470 uA |
| Uart Log | 500 uA |
| Uart Log + HF | 700 uA |

* 250 us @ 12 mA
* 1.5 ms @ 3.5 mA
* 50 uA @ infinite

{{<gfigure src="/images/simplemesh_sensortag/Power measures.png" >}}

## PPK-II Measures
### First wakeup
{{<gfigure src="/images/simplemesh_sensortag/first wakeup.png" >}}

### wakeup cycle type 1
{{<gfigure src="/images/simplemesh_sensortag/wakeup cycle.png" >}}

### wakeup cycle type 2
{{<gfigure src="/images/simplemesh_sensortag/wakeup_cycle_t2.png" >}}

### sleep period
{{<gfigure src="/images/simplemesh_sensortag/sleep_period.png" >}}


## BME280 vs MS8607
* `Thread sensortag` is using MS8607 and `nRF52832 sensortag` is using BME280
* `Thread sensortag` is sampling every 3 sec while `nRF52832 sensortag` every 40 sec
* `Thread sensortag` has no case while `nRF52832 sensortag` is inside a PLA case
### temperature
{{<gfigure src="/images/nrf52832_sensortag/compare_temperature.png" >}}
{{<gfigure src="/images/nrf52832_sensortag/temperature_hour.png" >}}
### humidity
{{<gfigure src="/images/nrf52832_sensortag/humidity.png" >}}
{{<gfigure src="/images/nrf52832_sensortag/humidity_hour.png" >}}
### pressure
{{<gfigure src="/images/nrf52832_sensortag/pressure.png" >}}

## MAX44009 vs VEML6030
* `Thread sensortag` is using VEML6030 and `nRF52832 sensortag` is using MAX44009
### light
{{<gfigure src="/images/nrf52832_sensortag/light.png" >}}
{{<gfigure src="/images/nrf52832_sensortag/light_day.png" >}}
{{<gfigure src="/images/nrf52832_sensortag/light_hour.png" >}}
