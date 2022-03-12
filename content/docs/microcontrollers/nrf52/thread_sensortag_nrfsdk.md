---
title: "nRF-SDK Thread"
description: "deprecated nrf SDK samples for a Low power SensorTag with High quality measures of light, Temperature, Humidity and Pressure. Based on nRF52840 which supports Thread and other protocols"
date: 2021-02-07T08:48:57+00:00
lastmod: 2021-05-14T08:00:00+00:00
images: ["/images/thread_sensortag/sensortag_v1.1.webp"]
weight: 4
toc: true
---
{{<load-svg-pan-zoom>}}
{{<load-model_viewer>}}
{{<load-photoswipe >}}

{{<hint warning>}} this page is kept as documentation only as the old NRF-SDK is deprecated and this project is continued in the following link using the nRF-Connect SDK based on Zephyr.
{{</hint>}}

{{<icon_button relref="/docs/microcontrollers/nrf52/thread_sensortag/" text="Thread SensorTag" >}}
{{<icon_button relref="/docs/networks/thread/" text="Networks / Thread" >}}
{{<icon_button relref="/docs/frameworks/chip/" text="Frameworks / CHIP" >}}


# NRF-SDK Firmware

{{<icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/tree/main/firmware" text="repo directory" icon="github" >}}

## 01 dongle nrfsdk mqttsn client

{{<hint info>}} Tested firmware with MQTT-SN gateway and Mosquitto broquer
{{</hint>}}

* example based on `nRFSDK for Thread and Zigbee v4.1.0` which path should be declared in an environment variable `THREAD_SDK_ROOT`
* Note that, as specified in the SDK file `components\toolchain\gcc\Makefile.windows` the used toolchain is `GNU Tools ARM Embedded/7 2018-q2-update`
* Adapted Makefile
* As the usb dongle has only one user key KEY_0, the sequence of `search_gateway`, `connect`, `publish` is not cyclically executed when pressing KEY_0 multiple times.

## 04 dongle pio zephyr blink
* zephyr with platformio environment for easy install
* dongle led blink
* added `boards\nrf52840dongle_nrf52840.json`
* jlink is the default upload method for the nrf52840 boards

## 06 tag zephyr hello
* hello world example using the `nrf52840_sensortag` custom board

zephyr examples usage
```bash
west build -b nrf52840_sensortag
west flash
```
{{<details "build log">}}
```bash
-- west build: generating a build system
Including boilerplate (Zephyr base (cached)): D:/Projects/zephyrproject/zephyr/cmake/app/boilerplate.cmake
-- Application: D:/Dev/nrf52/nrf52_thread_sensortag/firmware/tag_zephyr_hello
-- Zephyr version: 2.5.99 (D:/Projects/zephyrproject/zephyr)
-- Found west (found suitable version "0.9.0", minimum required is "0.7.1")
-- Board: nrf52840_sensortag
-- Cache files will be written to: D:/Projects/zephyrproject/zephyr/.cache
-- Found toolchain: gnuarmemb (D:/tools/gnu_arm_embedded/9_2020-q2-update)
```
{{</details>}}

{{<details "run log">}}
```log
*** Booting Zephyr OS build zephyr-v2.5.0-159-g2bfaadffb807  ***

[00:00:00.337,707] <inf> main: Hello from Sensor Tag
[00:00:01.337,768] <inf> main: loop: 0
[00:00:02.337,860] <inf> main: loop: 1
[00:00:03.338,012] <inf> main: loop: 2
[00:00:04.338,104] <inf> main: loop: 3
[00:00:05.338,256] <inf> main: loop: 4
[00:00:06.338,348] <inf> main: loop: 5
[00:00:07.338,500] <inf> main: loop: 6
[00:00:08.338,562] <inf> main: loop: 7
[00:00:09.338,653] <inf> main: loop: 8
[00:00:10.338,745] <inf> main: loop: 9
[00:00:11.338,836] <inf> main: loop: 10
[00:00:12.338,928] <inf> main: loop: 11
[00:00:13.339,019] <inf> main: loop: 12
[00:00:14.339,111] <inf> main: loop: 13
[00:00:15.339,202] <inf> main: loop: 14
[00:00:16.339,294] <inf> main: loop: 15
[00:00:17.339,385] <inf> main: loop: 16
[00:00:18.339,477] <inf> main: loop: 17

```
{{</details>}}

## 07 tag zephyr button
* introducing user button interrupt


{{<details "build log">}}
```bash
-- west build: generating a build system
Including boilerplate (Zephyr base): D:/Projects/zephyrproject/zephyr/cmake/app/boilerplate.cmake
-- Application: D:/Dev/nrf52/nrf52_thread_sensortag/firmware/07_tag_zephyr_button
-- Zephyr version: 2.5.99 (D:/Projects/zephyrproject/zephyr)
-- Found Python3: C:/Users/User/AppData/Local/Programs/Python/Python39/python.exe (found suitable exact version "3.9.0") found components: Interpreter
-- Found west (found suitable version "0.9.0", minimum required is "0.7.1")
-- Board: nrf52840_sensortag
-- Cache files will be written to: D:/Projects/zephyrproject/zephyr/.cache
-- Found toolchain: gnuarmemb (D:/tools/gnu_arm_embedded/9_2020-q2-update)
-- Found BOARD.dts: D:/Dev/nrf52/nrf52_thread_sensortag/firmware/boards/arm/nrf52840_sensortag/nrf52840_sensortag.dts
```
{{</details>}}

{{<details "run log">}}
rtt log
```log
00> *** Booting Zephyr OS build zephyr-v2.5.0-159-g2bfaadffb807  ***
00> 
00> [00:00:00.334,259] <inf> main: Hello from Sensor Tag Button example
00> [00:00:00.334,289] <inf> main: Set up button at GPIO_1 pin 9
00> 
00> [00:00:01.334,350] <inf> main: loop: 0
00> [00:00:02.334,442] <inf> main: loop: 1
00> [00:00:03.334,533] <inf> main: loop: 2
00> [00:00:04.334,625] <inf> main: loop: 3
00> [00:00:05.334,716] <inf> main: loop: 4
00> [00:00:06.334,808] <inf> main: loop: 5
00> [00:00:07.334,899] <inf> main: loop: 6
00> [00:00:08.334,991] <inf> main: loop: 7
00> [00:00:09.335,083] <inf> main: loop: 8
00> [00:00:10.335,174] <inf> main: loop: 9
00> [00:00:11.335,266] <inf> main: loop: 10
00> [00:00:12.335,357] <inf> main: loop: 11
00> [00:00:13.335,449] <inf> main: loop: 12
00> [00:00:14.335,540] <inf> main: loop: 13
00> [00:00:15.335,632] <inf> main: loop: 14
00> [00:00:16.335,723] <inf> main: loop: 15
00> [00:00:17.335,815] <inf> main: loop: 16
00> [00:00:18.335,906] <inf> main: loop: 17
00> [00:00:19.335,998] <inf> main: loop: 18
00> [00:00:20.336,090] <inf> main: loop: 19
00> [00:00:21.336,181] <inf> main: loop: 20
00> [00:00:22.336,273] <inf> main: loop: 21
00> [00:00:23.336,364] <inf> main: loop: 22
00> [00:00:24.336,456] <inf> main: loop: 23
00> [00:00:25.336,547] <inf> main: loop: 24
00> [00:00:26.336,639] <inf> main: loop: 25
00> [00:00:27.336,730] <inf> main: loop: 26
00> [00:00:28.336,822] <inf> main: loop: 27
00> [00:00:29.336,914] <inf> main: loop: 28
00> [00:00:30.337,005] <inf> main: loop: 29
00> [00:00:31.337,097] <inf> main: loop: 30
00> [00:00:32.337,188] <inf> main: loop: 31
00> [00:00:32.920,257] <inf> main: Button pressed at 1078731
00> 
00> [00:00:33.282,348] <inf> main: Button pressed at 1090596
00> 
00> [00:00:33.337,249] <inf> main: loop: 32
00> [00:00:34.337,310] <inf> main: loop: 33
00> [00:00:35.337,402] <inf> main: loop: 34
00> [00:00:36.337,493] <inf> main: loop: 35
00> [00:00:37.337,585] <inf> main: loop: 36
00> [00:00:38.337,677] <inf> main: loop: 37
00> [00:00:39.337,768] <inf> main: loop: 38
00> [00:00:40.337,860] <inf> main: loop: 39
00> [00:00:41.337,951] <inf> main: loop: 40
00> [00:00:42.338,043] <inf> main: loop: 41
00> [00:00:43.338,134] <inf> main: loop: 42
00> [00:00:44.338,226] <inf> main: loop: 43
00> [00:00:45.338,317] <inf> main: loop: 44
00> *** Booting Zephyr OS build zephyr-v2.5.0-159-g2bfaadffb807  ***
00> 
00> [00:00:00.250,396] <inf> main: Hello from Sensor Tag Button example
00> [00:00:00.250,396] <inf> main: Set up button at GPIO_1 pin 9
00> 
00> [00:00:01.250,457] <inf> main: loop: 0
00> [00:00:02.250,549] <inf> main: loop: 1
00> [00:00:03.250,640] <inf> main: loop: 2
00> [00:00:04.250,732] <inf> main: loop: 3
00> [00:00:05.250,823] <inf> main: loop: 4
00> [00:00:05.709,991] <inf> main: Button pressed at 187105
00> 
00> [00:00:06.250,915] <inf> main: loop: 5
00> [00:00:07.250,976] <inf> main: loop: 6
00> [00:00:08.251,068] <inf> main: loop: 7
00> [00:00:09.251,159] <inf> main: loop: 8
00> [00:00:10.251,251] <inf> main: loop: 9
00> [00:00:11.251,342] <inf> main: loop: 10
00> [00:00:12.251,434] <inf> main: loop: 11
00> [00:00:13.251,525] <inf> main: loop: 12
00> [00:00:14.251,617] <inf> main: loop: 13
00> [00:00:15.251,708] <inf> main: loop: 14
00> [00:00:16.251,800] <inf> main: loop: 15
00> [00:00:17.251,892] <inf> main: loop: 16
00> [00:00:18.251,983] <inf> main: loop: 17
00> [00:00:19.252,075] <inf> main: loop: 18
00> [00:00:20.252,166] <inf> main: loop: 19
00> [00:00:21.252,258] <inf> main: loop: 20
00> [00:00:22.252,349] <inf> main: loop: 21
```
{{</details>}}

## 08 tag zephyr thread
Simplest example with thread stack running

* thread stack added through config only
* menuconfig only runs on linux or wsl with the command `west build -t menuconfig`
* the generated config is under `build/zephyr/.config`
* Note that once the openthread config is persisted in non-volatile memory, changing the config would have no effect as the stored config has precedence over code
* the persisted config can be deleted with `ot factoryreset`
* Note that up to Zephyr version `zephyr-v2.5.0-159-g2bfaadffb807` the compilation throws an error 

Environment variables
* `GNUARMEMB_TOOLCHAIN_PATH` = D:\tools\gnu_arm_embedded\9_2020-q2-update
* `ZEPHYR_BASE` = D:\Projects\zp\zephyrproject\zephyr
* `ZEPHYR_TOOLCHAIN_VARIANT` = gnuarmemb

usage
```bash
west build -b nrf52840_sensortag -- -DCONF_FILE=prj-shell.conf
west build -b nrf52840_sensortag -- -DCONF_FILE=prj-log.conf
west flash
```

{{<details "default config">}}
```conf
#
# Thread Network configuration
#
CONFIG_OPENTHREAD_PANID=43981
CONFIG_OPENTHREAD_CHANNEL=13
CONFIG_OPENTHREAD_NETWORK_NAME="ot_zephyr"
CONFIG_OPENTHREAD_XPANID="de:ad:00:be:ef:00:ca:fe"
CONFIG_OPENTHREAD_MASTERKEY=""
CONFIG_OPENTHREAD_FTD=y
# CONFIG_OPENTHREAD_MTD is not set
CONFIG_OPENTHREAD_MAX_CHILDREN=32
CONFIG_OPENTHREAD_MAX_IP_ADDR_PER_CHILD=6
CONFIG_OPENTHREAD_CONFIG_PLATFORM_INFO="Zephyr"
CONFIG_OPENTHREAD_RADIO_LINK_IEEE_802_15_4_ENABLE=y
# CONFIG_OPENTHREAD_RADIO_LINK_TREL_ENABLE is not set
CONFIG_OPENTHREAD_CSL_SAMPLE_WINDOW=30
CONFIG_OPENTHREAD_CSL_RECEIVE_TIME_AHEAD=3
# CONFIG_OPENTHREAD_SRP_CLIENT is not set
# CONFIG_OPENTHREAD_SRP_SERVER is not set
# end of Thread Network configuration
```
{{</details>}}

{{<details "project shell config">}}
```conf
CONFIG_NETWORKING=y
CONFIG_NET_L2_OPENTHREAD=y

CONFIG_OPENTHREAD_CHANNEL=13
CONFIG_OPENTHREAD_PANID=47825

# Disable certain parts of Zephyr IPv6 stack
CONFIG_NET_IPV6_NBR_CACHE=n
CONFIG_NET_IPV6_MLD=n

# Kernel options
CONFIG_MAIN_STACK_SIZE=2048
CONFIG_INIT_STACKS=y

#pios
CONFIG_GPIO=y
CONFIG_SERIAL=n

# Logging
CONFIG_LOG=n
CONFIG_LOG_BACKEND_RTT=n
CONFIG_LOG_BACKEND_UART=n
CONFIG_BOOT_BANNER=y
CONFIG_USE_SEGGER_RTT=y

CONFIG_CONSOLE=y
CONFIG_UART_CONSOLE=n
CONFIG_RTT_CONSOLE=y

# shell
CONFIG_SHELL=y
CONFIG_SHELL_BACKEND_RTT=y
CONFIG_SHELL_BACKEND_SERIAL=n
```
{{</details>}}



{{<details "build log">}}
```bash
-- west build: generating a build system
Including boilerplate (Zephyr base): D:/Projects/zp/zephyrproject/zephyr/cmake/app/boilerplate.cmake
-- Application: D:/Dev/nrf52/nrf52_thread_sensortag/firmware/08_tag_zephyr_thread
-- Zephyr version: 2.5.99 (D:/Projects/zp/zephyrproject/zephyr)
-- Found Python3: C:/Users/User/AppData/Local/Programs/Python/Python39/python.exe (found suitable exact version "3.9.0") found components: Interpreter
-- Found west (found suitable version "0.10.1", minimum required is "0.7.1")
-- Board: nrf52840_sensortag
-- Cache files will be written to: D:/Projects/zp/zephyrproject/zephyr/.cache
-- Found toolchain: gnuarmemb (D:/tools/gnu_arm_embedded/10 2020-q4-major)
-- Found BOARD.dts: D:/Dev/nrf52/nrf52_thread_sensortag/firmware/boards/arm/nrf52840_sensortag/nrf52840_sensortag.dts
```
{{</details>}}

{{<details "run log">}}
```log
rtt:~$ *** Booting Zephyr OS build zephyr-v2.5.0-2187-g757cd12e6602  ***
ot state
leader

Done

rtt:~$ ot ipaddr
fdde:ad00:beef:0:0:ff:fe00:fc00

fdde:ad00:beef:0:0:ff:fe00:1800

fdde:ad00:beef:0:e004:6a1d:8704:d57f

fe80:0:0:0:38a9:1cd0:752:f538

Done
```
{{</details>}}

## 09 tag zephyr i2c
* using i2c_0 device from dts
* direct transactions with sensors using `i2c_reg_read_byte()`

{{<details "config">}}
```conf
CONFIG_GPIO=y
CONFIG_SERIAL=n

CONFIG_I2C=y

# Logging
CONFIG_LOG=y
CONFIG_LOG_BACKEND_RTT=y
CONFIG_LOG_BACKEND_UART=n
CONFIG_BOOT_BANNER=y
CONFIG_USE_SEGGER_RTT=y
CONFIG_CONSOLE=y
CONFIG_UART_CONSOLE=n
CONFIG_RTT_CONSOLE=y
```
{{</details>}}

{{<details "build log">}}
```bash
-- west build: generating a build system
Including boilerplate (Zephyr base): D:/Projects/zp/zephyrproject/zephyr/cmake/app/boilerplate.cmake
-- Application: D:/Dev/nrf52/nrf52_thread_sensortag/firmware/09_tag_zephyr_i2c
-- Zephyr version: 2.5.99 (D:/Projects/zp/zephyrproject/zephyr)
-- Found Python3: C:/Users/User/AppData/Local/Programs/Python/Python39/python.exe (found suitable exact version "3.9.0") found components: Interpreter
-- Found west (found suitable version "0.10.1", minimum required is "0.7.1")
-- Board: nrf52840_sensortag
-- Cache files will be written to: D:/Projects/zp/zephyrproject/zephyr/.cache
-- Found toolchain: gnuarmemb (D:/tools/gnu_arm_embedded/10 2020-q4-major)
-- Found BOARD.dts: D:/Dev/nrf52/nrf52_thread_sensortag/firmware/boards/arm/nrf52840_sensortag/nrf52840_sensortag.dts

...

[146/146] Linking C executable zephyr\zephyr.elf
Memory region         Used Size  Region Size  %age Used
           FLASH:       27168 B         1 MB      2.59%
            SRAM:        7936 B       256 KB      3.03%
        IDT_LIST:          0 GB         2 KB      0.00%
```
{{</details>}}

{{<details "run log">}}
```log
*** Booting Zephyr OS build zephyr-v2.5.0-2187-g757cd12e6602  ***

[00:00:00.327,209] <inf> main: Hello from Sensor Tag I2C example
[00:00:00.327,728] <inf> main: VEML6030 @48 reg[0] 'ALS_CONF' = 1
[00:00:00.328,247] <inf> main: MS8607 @40 'Relative Humidity RH' reg[0xE7] 'user register' = 2
[00:00:00.328,765] <inf> main: MS8607 @76 'Pressure and Temperature' reg[0] 'ADC Read' => 0
```
{{</details>}}



# FAQ - Discussion
* If you need support, want to ask a question or suggest a different answer, you can join the discussion on the Github forum
{{<icon_button text="Home Smart Mesh - sdk-hsm-sensortag" href="https://github.com/HomeSmartMesh/sdk-hsm-sensortag/discussions" icon="github" >}}
