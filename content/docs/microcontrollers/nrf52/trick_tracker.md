---
title: "Trick Tracker"
description: "The Trick Tracker that tracks tricks"
date: 2021-02-07T08:48:57+00:00
lastmod: 2021-05-14T08:00:00+00:00
images: ["/images/trick_tracker/title_image.jpg"]
weight: 3
toc: true
---
{{<load-svg-pan-zoom>}}
{{<load-model_viewer>}}
{{<load-photoswipe >}}

# Overview
{{<image src="/images/trick_tracker/concept.png" width="600px" >}}

The Trick-Tracker targets any user who is interested in tracking skill and improvement of certain fun sport devices. The Main focus of the first version are bikes but in general it can be used for any sort of sport devices which can be used to do stunts and tricks.
It targets beginners as well as professionals to either compete or just compare skills like Wheelie/Manual driving distance, jump height and distance etc. how steep was that ride. What was the bike doing in its airtime while doing a trick. Visualize your tricks digital to go through them step by step to see what can be improved. Was the front wheel not high enough or maybe the angle to less during the landing. 

## Hardware Requirements 

* `Bluetooth` | To connect to the phone and show data in real time 
* `Battery` | 
A Battery Flat-Pack with around 1000mAh. An runtime of 8h should be good enough
* `Battery Connector` | Connector to connect the battery to
* `Charger` | Charging circuit to charge the battery
*  `Accelerometer | Magnetometer | Gyroscope`
9-Axis Sensors to track any movement and rotation of the bike
* `Pressure Sensor` |
Precise pressure sensor which is able to track the height. possibly also of a jump
* `USB-C` |
For charging and to connect to a computer
* `USB Mass Storage` |
Device should be able to be detected as a mass storage
* `Extended Memory` |
To save the data locally instead of using an SD Card to do so
* `Buttons` |
Button Nr1 to reset the device? Maybe calibration?
Button Nr2 for an Event which can be placed in the data to mark special events
* `ON/OFF Switch` |
Switch to turn the device on and off
* `QWIIC Connector` |
To daisy chain sensors if needed with I2C. https://www.adafruit.com/product/4208
* `LED Indicators` | 
LED Indication for charging and charging done
LED for debugging connected to the MCU
LED to indicate power on
* `ToF Sensor (optional via QWiiC)` | To measure distance from the bike to the ground. Might be added by cable (Qwicc) and not be on the board directly for adjustment reasons.

## MVP and PoC Featureset
* Detect a jump and track its height
* Calculate Airtime
* Detect a wheelie and its travel time

## Schematics

{{<svg-pan-zoom "/images/trick_tracker/trick_tracker_schematics.svg" "white" >}}
Version 1.0
