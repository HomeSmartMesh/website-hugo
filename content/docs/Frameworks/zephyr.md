---
title: "Zephyr RTOS"
description: "The linux style RTOS for microcontrollers"
date: 2021-02-20T08:00:00+00:00
lastmod: 2021-02-20T08:00:00+00:00
weight: 5
toc: true
---
{{<load-photoswipe >}}
{{<icon_button relref="/docs/networks/thread/" text="Thread Protocol" >}}
{{<icon_button relref="/docs/frameworks/chip/" text="Project Matter" >}}

# Trace with with SystemView
* [Segger SystemView](https://www.segger.com/products/development-tools/systemview/) : Real time OS Tracing
* [SystemView user manual](https://www.segger.com/downloads/free-utilities/UM08027) : Getting started, API reference,...

{{<hint warning>}}It's highly recommended to read the SystemView user manual to understand how the RT OS concepts are displayed{{</hint>}}

Steps :
* Install SystemView
* compile the target with the overlay `overlay-tracing.conf` and flash, reset
* SystemView Recorder configuration `J-Link`, `NRF52840_XXAA`, `SWD`, `4000 KHz`
* SystemView `Start Recording` the `Stop Recording`
* if SystemViewer keeps crashing try closing the window `CPU load`

{{<gfigure src="/images/zephyr/systemview.webp" width="500px">}}

content of `overlay-tracing.conf`
```conf
#Tracing
CONFIG_TRACING=y
CONFIG_SEGGER_SYSTEMVIEW=y
CONFIG_THREAD_NAME=y
CONFIG_SEGGER_SYSTEMVIEW_BOOT_ENABLE=y
# careful adjustment as long as there is enough SRAM use it for tracing 128 KB
CONFIG_SEGGER_SYSVIEW_RTT_BUFFER_SIZE=131072
```
## Hints
* power on tracing : In case a power-on startup has to be traced, it is necessary to increase the target local RTT buffer size, because it keeps bauffering the tracing until the user has the time to start SystemView recording. For that, it is important to check how much free memory the system has, and then adjust the RTT buffer
* ISR ID Identification : the function `sysview_get_interrupt()` is using `SCB->ICSR VECTACTIVE`
* not the same as `IRQn_Type` defined in `modules\hal\nordic\nrfx\mdk\nrf52840.h`

{{<table "table table-striped table-bordered">}}
VECTACTIVE | IRQ
-----------|-----
16 | nrf_clock_event_check
17 | nrf5_radio_irq
19 | nrfx_twi_0_irq_handler

{{</table>}}


# Debug with OZone
* [Ozone](https://www.segger.com/products/development-tools/ozone-j-link-debugger/) : performance analyzer
* [RTOS Awareness](https://www.segger.com/products/development-tools/ozone-j-link-debugger/technology/rtos-awareness/) : debug an RTOS

Steps :
* make sure to use Ozone version 3.22d or higher
* in the new project wizard, make sure an svd file is selected e.g. from the Zephyr projecz `modules\hal\nordic\nrfx\mdk\nrf52840.svd`
* For a Segger j-link edu, select the Target interface SWD
* compile with `CONFIG_DEBUG_THREAD_INFO=y`
* select the elf file of the Zephyr build `hsm/hsm/samples/tag_power/build/zephyr/zephyr.elf`
* in the bottom left console type `Project.SetOSPlugin("ZephyrPlugin_CM4")`. That will load the Zephyr RTOS awareness plugin
* you should now be able to access the Zephyr debug window from the menu `View` then in the `Advanced` section `Zephyr`

{{<gfigure src="/images/zephyr/ozone_zephyr.png" width="500px">}}

# Serial Debug
* Log : `CONFIG_LOG` creates a `logging` thread and enables `LOG_INF()`, `LOG_DBG()`,... functions
	* needs a backend e.g. `CONFIG_LOG_BACKEND_RTT`, `CONFIG_LOG_BACKEND_UART`
* Shell : `CONFIG_SHELL` creates a `shell_xxx` thread that enables an interactive command line e.g. `rtt:~$ kernel threads`
	* needs a backend e.g. `CONFIG_SHELL_BACKEND_RTT`, `CONFIG_SHELL_BACKEND_SERIAL`
## shell command examples
```shell
>help
>kernel threads
>kernel stacks
>log enable dbg main

```

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
