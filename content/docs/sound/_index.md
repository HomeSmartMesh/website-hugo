---
title : "Sound Processing"
description: "Mastering real time sound capture processing and generation on embedded processors"
date: 2021-01-31T00:00:00+00:00
weight: 8
bookToC: true
images: ["/images/sound/web_py_loop.png"]
---
{{<load-photoswipe >}}

# web py loopback

{{<icon_button text="repo" href="https://github.com/SoundHacking/web_py_loop" icon="github" >}}

{{<image src="/images/sound/web_py_loop.png" >}}

* using `createScriptProcessor` which is to be deprecated
* Note `MediaStreamTrackProcessor` is not supported yet as of 02 Apr 2021. [Sample demo](https://github.com/webrtc/samples/blob/gh-pages/src/content/insertable-streams/audio-processing/js/main.js) available

## web audio script processor
* using the deprecated `ScriptProcessorNode` to divert the input and send it through websocket as `Float32Array` to the server
* on the same processing loop, the audio is replacing with the incoming `Float32Array` stream from the server
* During the first iteration of the processing loop, as the queue is empty, the output is filled with zeros

{{<gfigure src="/design/02_script_processor_loopback.svg" width="500px">}}

## media source extension API
* using `MediaRecorder` and `MediaSource` for encoded stream loopback through websocket
{{<gfigure src="/design/06_media_recorder_source_loopback.svg" width="400px">}}

## webRTC
Official example 
{{<icon_button text="peerconnection audio" href="https://github.com/webrtc/samples/tree/gh-pages/src/content/peerconnection/audio" icon="github" >}}

{{<gfigure src="/design/web_rtc.svg" width="300px">}}



## pystream
{{<icon_button text="repo" href="https://github.com/SoundHacking/pystream" icon="github" >}}

{{<image src="/images/sound/python_process.png" >}}

* List audio devices
* Test Audio input and output
* connect inputs to outputs
* process the live stream between input and output
* generate tone

### dependencies

* matplotlib
* numpy
* [sounddevice](https://python-sounddevice.readthedocs.io/)

# see sound

{{<icon_button text="repo" href="https://github.com/SoundHacking/see_sound/" icon="github" >}}

{{<image src="/images/sound/analyser.png" >}}

* a web app framework to see the sound in different forms
* real-time analysis of played audio
* Playing sound with html5 [web Audio API](https://www.w3.org/TR/webaudio/)
## Live demo

{{< iframe src="https://soundhacking.github.io/see_sound/" height="300" >}}

# librosa test

{{< new_button href="https://github.com/SoundHacking/librosa_demo/" text="github project" >}}

* Jupyter notebooks to showcase librosa functions

{{<image src="/images/sound/librosa.png" width=600 >}}



# Microcontroller loopback
{{<image src="/images/sound/nRF52_usb_loopback.png" width=600 >}}
* usb audio loopback zephyr example on `nrf52840dk_nrf52840`
* tested on the nRF52 dongle with debugger attached

build
```bash
cd zephyr/samples/subsys/usb/audio/headphones_microphone
west build -b nrf52840dk_nrf52840 -- -DCONF_FILE=prj.conf
west flash
```

{{<details "build log">}}
```bash
-- west build: generating a build system
-- Application: D:/Projects/zp/zephyrproject/zephyr/samples/subsys/usb/audio/headphones_microphone
-- Zephyr version: 2.5.99 (D:/Projects/zp/zephyrproject/zephyr)
-- Found Python3: C:/Users/User/AppData/Local/Programs/Python/Python39/python.exe (found suitable exact version "3.9.0") found components: Interpreter 
-- Found west (found suitable version "0.10.1", minimum required is "0.7.1")
-- Board: nrf52840dk_nrf52840
-- Cache files will be written to: D:/Projects/zp/zephyrproject/zephyr/.cache
-- Found toolchain: gnuarmemb (D:/tools/gnu_arm_embedded/10 2020-q4-major)
-- Found BOARD.dts: D:/Projects/zp/zephyrproject/zephyr/boards/arm/nrf52840dk_nrf52840/nrf52840dk_nrf52840.dts
-- Found devicetree overlay: D:/Projects/zp/zephyrproject/zephyr/samples/subsys/usb/audio/headphones_microphone/boards/nrf52840dk_nrf52840.overlay
-- Generated zephyr.dts: D:/Projects/zp/zephyrproject/zephyr/samples/subsys/usb/audio/headphones_microphone/build/zephyr/zephyr.dts
...
-- west build: building application
[152/159] Linking C executable zephyr\zephyr_prebuilt.elf

[159/159] Linking C executable zephyr\zephyr.elf
Memory region         Used Size  Region Size  %age Used
           FLASH:       45912 B         1 MB      4.38%
            SRAM:       16004 B       256 KB      6.11%
        IDT_LIST:          0 GB         2 KB      0.00%
```
{{</details>}}

# Future Plan

## Interactive sound input
* inputs in microcontrollers for preprocessing interface to PC for further processing.
* it's also possible to have microcontroller standalone applications

{{< image src="/images/sound/goal.png" >}}

## Debug workflow
* it's possible to debug an embedded sound processing system by injecting packets and retrieving  the results

{{< image src="/images/sound/usb_cdc_test.png" width="500">}}

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

# FAQ - Discussion
If you need support, want to ask a question or suggest an idea, you can join the discussion on the forum
{{<icon_button text="Home Smart Mesh - Discussions" href="https://github.com/HomeSmartMesh/raspi/discussions" icon="github" >}}

{{<faq>}}
What is the difference between a MediaStream and a MediaSource when used in a real time network ?
<--->
It is possible to append Buffers to a MediaSource which opens its input to a custom websocket while MediaStream, can only be bound to a webRTC RTCPeerConnection.

Note that `audio.src` is to be used with a `MediaSrouce` URL while `audio.srcObject` is for `MediaStream`.
{{</faq>}}
