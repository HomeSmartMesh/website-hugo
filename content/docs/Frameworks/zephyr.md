---
title: "Zephyr RTOS"
description: "The linux style RTOS for microcontrollers"
date: 2021-02-20T08:00:00+00:00
lastmod: 2021-02-20T08:00:00+00:00
weight: 4
toc: true
---
# Relates to
{{<icon_button relref="/docs/networks/thread/" text="Thread Protocol" >}}
{{<icon_button relref="/docs/frameworks/chip/" text="Project CHIP" >}}

# Windows
## Install
{{<icon_button href="https://docs.zephyrproject.org/2.3.0/getting_started/index.html" text="Getting Started details..."  icon="new" >}}

The details are in the link above, the summary of the step for installing on windows are
* Installing `choco`
* Installing `west` dependencies with `choco`
* installing `west` with `pip`
* the retrieving the repo and building are performed through `west`
* The compiler toolchain as multiple options
  * Zephyr own SDK, not available on windows
  * GNU ARM Embedded : to be installed on a path without spaces

e.g. for a path `"D:\tools\gnu_arm_embedded\9_2020-q2-update\bin\arm-none-eabi-gcc.exe"` `GNUARMEMB_TOOLCHAIN_PATH` shall be set to `D:\tools\gnu_arm_embedded\9_2020-q2-update`

## Build and Flash
```bash
west build -p auto -b nrf52840dongle_nrf52840 samples\basic\blinky
west flash
nrfjprog -f nrf52 --reset
```
{{<hint warning "hello ?">}}Flashing this USB Dongle board requires one of the following options{{</hint>}}

{{<icon_button href="https://docs.zephyrproject.org/2.3.0/boards/arm/nrf52840dongle_nrf52840/doc/index.html#programming-and-debugging" text="How to Flash docs.zephyrproject"  icon="new" >}}

* Option1 `Using the built-in bootloader only` : uses nrfutils and requires the default bootloader to be already flashed
* Option2 `Using MCUboot in Serial Recovery Mode` : requires building the MCUboot and using nrfutils
* Option3 `Using an External Debug Probe` :
  * requires soldering SWD pins or using [this pogo pins adapter](/docs/microcontrollers/nrf52/usb_dongle/#pogo-pin-adapter)
  * also `manual reset` or `nrfjprog -f nrf52 --reset`

{{<details "modifying the DTS">}}
The board device tree source located in `boards\arm\nrf52840dongle_nrf52840\nrf52840dongle_nrf52840.dts`
the end of the file should look like below after the modification
```
/* Include flash partition table.
 * Two partition tables are available:
 * fstab-stock		-compatible with Nordic nRF5 bootloader, default
 * fstab-debugger	-to use an external debugger, w/o the nRF5 bootloader
 */
#include "fstab-debugger.dts"

&usbd {
	compatible = "nordic,nrf-usbd";
	status = "okay";
};
```
we notice that after that change the generated dts file is different `build\zephyr\zephyr.dts`

{{<image src="/images/thread_sensortag/west_dts.png" >}}
{{</details>}}

# Linux
## Install
* installing Zephyr with its own compiler toolchain `Zephyr SDK`

* [install Zephyr dependencies - Ubuntu](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/zephyr/getting_started/index.html#install-required-tools)
* install [nRF Command Line tools](https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Command-Line-Tools/Download#infotabs)

```bash
tar -xf nRFCommandLineTools10121Linuxamd64.tar.gz
sudo dpkg -i nRF-Command-Line-Tools_10_12_1_Linux-amd64.deb
```
## Build and Flash

Testing blinky sample

add `CONFIG_BOARD_HAS_NRF5_BOOTLOADER=n` to `prj.conf`

tested version 2.5.99
```bash
cd ~/zephyrproject/zephyr/samples/basic/blinky
west build -p auto -b nrf52840dongle_nrf52840 -- -DCONF_FILE=prj.conf
west flash
nrfjprog -f nrf52 --reset
```
