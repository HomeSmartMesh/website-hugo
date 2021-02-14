---
title: "Thread SensorTag"
description: "A Low power SensorTag with High quality measures of light, Temperature, Humidity and Pressure. Based on nRF52840 which supports multiple protocols including Thread"
date: 2021-02-07T08:48:57+00:00
lastmod: 2021-02-14T08:00:00+00:00
images: ["/images/thread_sensortag/concept.png"]
weight: 2
toc: true
---

{{<icon_button relref="/docs/networks/thread/" text="Networks / Thread" >}}
{{<icon_button relref="/docs/frameworks/chip/" text="Frameworks / CHIP" >}}

# v1.0 MS8607 VEML6030

{{<image src="/images/thread_sensortag/concept.png" >}}
{{<image src="/images/thread_sensortag/kicad_render.png" width="300vw" >}}

{{< icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/commit/32a897a91b8c92f2495ef75408c987166ff7b465" text="commit" icon="github" >}}

{{< icon_button href="https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF52840" text="nRF52840" icon="new" >}}

{{< icon_button href="https://www.minew.com/products/nrf52840-module-ms88sf2.html" text="MS88SF2" icon="new" >}}

{{< icon_button href="https://www.mouser.com/datasheet/2/418/5/NG_DS_MS8607-02BA01_B3-1134999.pdf" text="MS86072" icon="new" >}}

{{< icon_button href="https://www.mouser.com/datasheet/2/427/veml6030-1767367.pdf" text="VEML6030" icon="new" >}}

## Schematics

{{< svg-pan-zoom "/images/thread_sensortag/nrf52-sensor-tag.svg" "white" >}}

## Board

{{< svg-pan-zoom "/images/thread_sensortag/nrf52-sensor-tag-brd.svg" "white" >}}

## 3D Board model

{{< model_viewer "/models/nrf/sensortag_1.glb" "400" >}}


# v0.0 BME280 MAX44009

{{< icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/commit/cb1083393b6b34cd9d02931da196c2d072bd03fc" text="commit" icon="github" >}}

{{<image src="/images/thread_sensortag/concept_0.png" >}}


The Thread sensorTag is based on an `nRF52840` which is integrated inside an `ISP1807`. On the tag, the used sensors are the `BME280` for Temperature, Humidity, Pressure and 

{{< icon_button href="https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF52840" text="nRF52840" icon="new" >}}

{{< icon_button href="https://www.insightsip.com/products/bluetooth-smart-modules/isp1807" text="ISP1807" icon="new" >}}

{{< icon_button href="https://www.bosch-sensortec.com/products/environmental-sensors/humidity-sensors-bme280/" text="BME280" icon="new" >}}

{{< icon_button href="https://www.maximintegrated.com/en/products/interface/sensor-interface/MAX44009.html" text="MAX44009" icon="new" >}}



## Repo

{{< icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/commit/fe999911c088dd4ad4b96288b7cdaeced648661c" text="schematics 07/02/2021 commit fe9999"  icon="github" >}}

## Components

### Consumption

{{< iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vTjyTmBeA3KFeX2gRebiZoTsREhtoXCE8qZstHx1fQO6X83I-5LDIG0I0BbR6QdomUmvdfttt59Nt4Q/pubhtml?gid=0&amp;single=true&amp;widget=true&amp;headers=false" height="200" >}}

## Schematics

{{< svg-pan-zoom "/images/thread_sensortag/nrf52-sensor-tag_v0.svg" "white" >}}

## Board

{{< svg-pan-zoom "/images/thread_sensortag/nrf52-sensor-tag-brd_v0.svg" "white" >}}

## 3D Board model

{{< model_viewer "/models/nrf/sensortag_0.glb" "400" >}}

# Firmware
## Breadboard testing

{{<image src="/images/thread_sensortag/breadboard_test.png" >}}
