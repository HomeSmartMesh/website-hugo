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

# Windows Install
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
# Linux Install
* installing Zephyr with its own compiler toolchain `Zephyr SDK`

* [install Zephyr dependencies - Ubuntu](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/zephyr/getting_started/index.html#install-required-tools)
* install [nRF Command Line tools](https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Command-Line-Tools/Download#infotabs)

```bash
tar -xf nRFCommandLineTools10121Linuxamd64.tar.gz
sudo dpkg -i nRF-Command-Line-Tools_10_12_1_Linux-amd64.deb
```
# Build and Flash

Testing blinky sample

add `CONFIG_BOARD_HAS_NRF5_BOOTLOADER=n` to `prj.conf`

tested version 2.5.99
```bash
cd ~/zephyrproject/zephyr/samples/basic/blinky
west build -p auto -b nrf52840dongle_nrf52840 -- -DCONF_FILE=prj.conf
west flash
nrfjprog -f nrf52 --reset
```
## RTT config
required configuration to have logs running over the segger j-link RTT (log through the same SWD interface used for programming)
{{<details "prj.conf">}}
```conf
CONFIG_GPIO=y
CONFIG_SERIAL=n
CONFIG_BOARD_HAS_NRF5_BOOTLOADER=n

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

{{<details "main.c">}}
```C
#include <zephyr.h>
#include <logging/log.h>
LOG_MODULE_REGISTER(main, LOG_LEVEL_INF);
void main(void)
{
	LOG_INF("Starting");
	int count = 0;
	while (1) {
		LOG_INF("loop: %d",count++);
	}
}

```
{{</details>}}

{{<icon_button text="pio zephyr log example" href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/tree/main/firmware/pio_zephyr_blink" icon="github">}}
