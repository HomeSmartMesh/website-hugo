---
title: "Thread SensorTag"
description: ""
date: 2021-02-07T08:48:57+00:00
lastmod: 2021-02-07T08:48:57+00:00
images: []
weight: 2
toc: true
---

# Concept

{{<image src="/images/thread_sensortag/concept.png" >}}


The Thread sensorTag is based on an `nRF52840` which is integrated inside an `ISP1807`. On the tag, the used sensors are the `BME280` for Temperature, Humidity, Pressure and 

{{< new_button href="https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF52840" text="nRF52840">}}

{{< new_button href="https://www.insightsip.com/products/bluetooth-smart-modules/isp1807" text="ISP1807">}}

{{< new_button href="https://www.bosch-sensortec.com/products/environmental-sensors/humidity-sensors-bme280/" text="BME280">}}

{{< new_button href="https://www.maximintegrated.com/en/products/interface/sensor-interface/MAX44009.html" text="MAX44009">}}



# Repo

{{< new_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/commit/fe999911c088dd4ad4b96288b7cdaeced648661c" text="schematics 07/02/2021 commit fe9999">}}

# Components Consumption

{{< iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vTjyTmBeA3KFeX2gRebiZoTsREhtoXCE8qZstHx1fQO6X83I-5LDIG0I0BbR6QdomUmvdfttt59Nt4Q/pubhtml?gid=0&amp;single=true&amp;widget=true&amp;headers=false" height="200" >}}

# Schematics

{{< svg-pan-zoom "/images/thread_sensortag/nrf52-sensor-tag.svg" "white" >}}

# Board

{{< svg-pan-zoom "/images/thread_sensortag/nrf52-sensor-tag-brd.svg" "white" >}}

# 3D Board model

{{< model_viewer "/models/sensortag.glb" "400" >}}

# Firmware
## Breadboard testing

{{<image src="/images/thread_sensortag/breadboard_test.png" >}}
