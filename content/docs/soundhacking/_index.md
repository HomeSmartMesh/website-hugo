---
title : "Sound Hacking"
description: "Mastering real time sound capture processing and generation on embedded processors"
date: 2021-01-31T00:00:00+00:00
weight: 20
bookFlatSection: true
bookCollapseSection: false
bookToC: false
---

The idea behind the [Sound Hacking github organisation](https://github.com/SoundHacking) is to take advantage of the latest advances of Machine learning and Audio processing to create innovative applications.

## Interactive sound input
* inputs in microcontrollers for preprocessing interface to PC for further processing.
* it's also possible to have microcontroller standalone applications

{{< image src="/images/sound/goal.png" >}}

## Debug workflow
* it's possible to debug an embedded sound processing system by injecting packets and retrieving  the results

{{< image src="/images/sound/usb_cdc_test.png" width="500">}}

# Discord channel

{{< discord id="780126809671991328" >}}

* invitation to the [Sound Hacking discord server](https://discord.gg/57cZapWHkC)
* A place to ask questions, make suggestions and see posts about the progress

# Topics
## Offline Preparation
The Offline preparation is a necessary step to understand the signal and condition it for the target application where it will be used, that's where Python with its scientific toolkits and machine learning environment will be used.
* Signal Analysis
* Machine Learning

## Real Time Processing
Smart applications are ones that instantly react to the user in an interactive, collaborative or assistive mode such as during tracking.
Therefore an instant response of the system is what creates a feeling of connection with the instrument.

Real time Audio processing requires dedicated hardware acceleration to prevent delays. 

## Capture
It's about capture of audio and other user inputs. Audio goes through  I2S microphone arrays connected to microcontrollers. Other user inputs can be accelerometer or touch interfaces that are to be used as control for sound generation
* Microphones
* Accelerometers
* smart touch surface
* USB interfaces (Audio / HID / Serial Data)

## Display
The output should not only be hearable as a sound signal but also visual in order to assist the interaction.
* webapp view of live audio signal
* Status feedback through LEDs

## Microcontrollers
{{< image src="/images/hardware.png" width=800 >}}

* MEMS microphones : [SPH0645](https://cdn-reichelt.de/documents/datenblatt/A300/DEBO_MEMS_MIC_DB_EN.pdf)
* [STM32F407VGT6](https://stm32-base.org/boards/STM32F407VGT6-STM32F4XX-M.html)

Microcontrollers in scope :

* STM32 based on ARM-M4
    * Black pill : STM32F411 (USB)
    * Audio Discovery : STM32F407 (USB, stereo out)
    * DevEBox : STM32F407 (USB, SDCARD)
* ESP32 (wifi / BT)
* nRF52 (custom RF / BT)

Example integration of cubemx with pio :
* [f411_piocube_lib_blink](https://github.com/STM32Libs/f411_piocube_lib_blink)
# Repositories

## [pystream](https://github.com/SoundHacking/pystream)
* List audio devices
* Test Audio input and output
* connect inputs to outputs
* process the live stream between input and output
* generate tone

### dependencies

* matplotlib
* numpy
* [sounddevice](https://python-sounddevice.readthedocs.io/)

## see sound

{{< new_button href="https://github.com/SoundHacking/see_sound/" text="github project" >}}

* a web app framework to see the sound in different forms
* real-time analysis of played audio
* Playing sound with html5 [web Audio API](https://www.w3.org/TR/webaudio/)
### Live demo

{{< iframe src="https://soundhacking.github.io/see_sound/" height="300" >}}

## librosa_demo

{{< new_button href="https://github.com/SoundHacking/librosa_demo/" text="github project" >}}

* Jupyter notebooks to showcase librosa functions

{{< image src="/images/librosa.png" width=600 >}}
