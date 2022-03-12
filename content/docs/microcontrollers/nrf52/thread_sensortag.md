---
title: "Thread SensorTag"
description: "A Low power SensorTag with High quality measures of light, Temperature, Humidity and Pressure. Based on nRF52840 which supports Thread and other protocols"
date: 2021-02-07T08:48:57+00:00
lastmod: 2021-05-14T08:00:00+00:00
images: ["/images/thread_sensortag/sensortag_v1.1.webp"]
weight: 1
toc: true
---
{{<load-svg-pan-zoom>}}
{{<load-model_viewer>}}
{{<load-photoswipe >}}

{{<icon_button relref="/docs/networks/thread/" text="Networks / Thread" >}}
{{<icon_button relref="/docs/frameworks/chip/" text="Frameworks / CHIP" >}}

# Overview
{{<image src="/images/thread_sensortag/sensortag_v1.1.webp" width="600px" >}}

* `nRF52840` on a [MS88SF2](?svg=nrf52-sensor-tag&text=MS88SF2) module.
* i²C bus scl `P1.13` and sda `P1.15`
* Light sensor [VEMLS6030](?svg=nrf52-sensor-tag&text=VEML6030) @`0x48`
* [MS8607](?svg=nrf52-sensor-tag&text=MS860702BA01-50) with relative humidity @`0x40`, pressure and temperature @`0x76`
* Reset button `P0.18` and [user button](?svg=nrf52-sensor-tag&text=S2) SW0 `P1.09`
* [RGB LED](?svg=nrf52-sensor-tag&text=LED1) `P0. 04 06 08`

# Hardware
## Schematics

{{<svg-pan-zoom "/images/thread_sensortag/nrf52-sensor-tag.svg" "white" >}}

* version 1.1
* update from v1.0 added RGB LED

{{<icon_button href="https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF52840" text="nRF52840" icon="new" >}}

{{<icon_button href="https://www.minew.com/products/nrf52840-module-ms88sf2.html" text="MS88SF2" icon="new" >}}

{{<icon_button href="https://www.mouser.com/datasheet/2/418/5/NG_DS_MS8607-02BA01_B3-1134999.pdf" text="MS86072" icon="new" >}}

{{<icon_button href="https://www.vishay.com/docs/84366/veml6030.pdf" text="VEML6030 DS" icon="new" >}}
{{<icon_button href="https://www.vishay.com/docs/84367/designingveml6030.pdf" text="VEML6030 AN" icon="new" >}}

## Manufacturing
* v 1.1 15.04.2021
* design files by [Mirko Informaticore](https://github.com/Informaticore)
{{<icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/tree/v1.1" text="tag v1.1" icon="github" >}}

* gerber files
{{<icon_button text="Gerber files v1.1" href="/data/manufacturing_v1.1.zip" icon="download" >}}

## Board

{{<image src="/images/thread_sensortag/board_top_v1.1.webp" width="500px" >}}

## Produced versions
* v 1.1 15.04.2021

{{<icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/tree/v1.1" text="tag v1.1" icon="github" >}}

{{<gfigure src="/images/thread_sensortag/prototype_top_v1.1.webp" width="200px" >}}

{{<gfigure src="/images/thread_sensortag/prototype_bottom_v1.1.webp" width="200px" >}}

* v 1.0 10.02.2021

{{<icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/tree/v1.0" text="tag v1.0" icon="github" >}}

{{<gfigure src="/images/thread_sensortag/prototype_top.webp" width="200px" >}}

{{<gfigure src="/images/thread_sensortag/prototype_bottom.webp" width="200px" >}}

## Pogo pins adapter
* SWD pogo pins adapter
* Pogo Pin P75-E2 Dia 1.3mm Length 16.5mm

{{<icon_button text="STL Model" href="/models/thread4_lower.stl" icon="download" >}}
{{< model_viewer "/models/thread4_lower.glb" "400" >}}
{{< gallery dir="/images/thread_sensortag/swd-pogo" />}}


# Software
{{<image src="/images/thread_sensortag/software.webp" width="600px" >}}

## Openthread Dongle
This section includes instructions how to create an openthread dongle called `Radio Co-Processor` (RCP), that needs to be flashed then attached to the raspberry pi USB port.
 
{{<button relref="/docs/networks/thread#radio-co-processor-rcp">}}nRF52840-usb RCP{{</button>}}
## Raspberry Pi Setup
This section includes steps to install all needed services to run the SensorTag udp-v6 broadcast to mqtt, influxdb and grafana this also inlcudes the boarder router setup as intermediate step.
 
{{<button relref="/docs/frameworks/raspi_iot#raspberry-pi-setup">}}raspberry pi setup{{</button>}}



## Zephyr Tag Firmware
{{<icon_button href="https://github.com/HomeSmartMesh/sdk-hsm-sensortag" text="Zephyr nRF-Connect SensorTag repo" icon="github" >}}

* The development environment is based on [zephyr and west](http://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/zephyr/getting_started/index.html#getting-started)

{{<hint warning>}}
make sure there is no environment variable `ZEPHYR_BASE` in order to use the local zephyr version
{{</hint>}}

* create directory structure and fetch the sdk :
```bash
mkdir hsm_workspace
cd hsm_workspace
west init -m https://github.com/HomeSmartMesh/sdk-hsm-sensortag --mr main
```
* fetch all dependencies from referenced repos in `hsm.west.yml`
```bash
west update
```

The first presented SDK sample is the overall application that broadcasts all sensors measures, followed by a list of basic applications with minimal code for each driver and sensor separately

### tag_sensors_broadcast
{{<icon_button href="https://github.com/HomeSmartMesh/sdk-hsm-sensortag/tree/main/samples/openthread_tag_sensors" text="tag sensors broadcast" icon="github" >}}

{{<gfigure src="/images/thread_sensortag/application.png" >}}

```bash
cd hsm/samples/tag_sensors_broadcast
west build -b nrf52840_sensortag
west flash
```

{{<details "default config">}}
```conf
# Peripherals
CONFIG_ADC=y
CONFIG_BATTERY=y

CONFIG_I2C=y
CONFIG_SENSOR=y
CONFIG_VEML6030=y
CONFIG_MS8607=y

CONFIG_GPIO=y

# Power optimization
CONFIG_PM=y
CONFIG_PM_DEVICE=y
CONFIG_SERIAL=n
CONFIG_LOG=n
CONFIG_CONSOLE=n
CONFIG_UART_CONSOLE=n
CONFIG_RTT_CONSOLE=n
CONFIG_USE_SEGGER_RTT=n
CONFIG_PRINTK=n

# message generation
CONFIG_NEWLIB_LIBC=y
CONFIG_NEWLIB_LIBC_FLOAT_PRINTF=y

# Network
CONFIG_NETWORKING=y
CONFIG_NET_L2_OPENTHREAD=y

# Minimal Thread Device & Sleepy End Device
CONFIG_OPENTHREAD_MTD=y 
CONFIG_OPENTHREAD_MTD_SED=y
CONFIG_OPENTHREAD_SHELL=n
#CONFIG_OPENTHREAD_FTD=y
CONFIG_OPENTHREAD_POLL_PERIOD=600000
#CONFIG_OPENTHREAD_MANUAL_START=y

CONFIG_OPENTHREAD_CHANNEL=13
CONFIG_OPENTHREAD_PANID=4660
CONFIG_OPENTHREAD_NETWORK_NAME="OpenThreadDemo"
CONFIG_OPENTHREAD_XPANID="11:11:11:11:22:22:22:22"
CONFIG_OPENTHREAD_MASTERKEY="00:11:22:33:44:55:66:77:88:99:aa:bb:cc:dd:ee:ff"


#CONFIG_NET_CONFIG_MY_IPV6_ADDR="fdde:ad00:beef::1"
#CONFIG_NET_CONFIG_PEER_IPV6_ADDR="ff02::1"
#CONFIG_PEER_PORT=4242

# Disable certain parts of Zephyr IPv6 stack
CONFIG_NET_IPV6_NBR_CACHE=n
CONFIG_NET_IPV6_MLD=n

# Kernel options
CONFIG_MAIN_STACK_SIZE=2048
CONFIG_INIT_STACKS=y

# Generic networking options
CONFIG_NET_UDP=y
CONFIG_NET_TCP=n
CONFIG_NET_SOCKETS=y
CONFIG_NET_SOCKETS_POSIX_NAMES=y
CONFIG_NET_SOCKETS_POLL_MAX=4
CONFIG_NET_CONNECTION_MANAGER=y

# Network buffers
CONFIG_NET_PKT_RX_COUNT=16
CONFIG_NET_PKT_TX_COUNT=16
CONFIG_NET_BUF_RX_COUNT=80
CONFIG_NET_BUF_TX_COUNT=80
CONFIG_NET_CONTEXT_NET_PKT_POOL=y

# IP address options
CONFIG_NET_IF_UNICAST_IPV6_ADDR_COUNT=3
CONFIG_NET_IF_MCAST_IPV6_ADDR_COUNT=4
CONFIG_NET_MAX_CONTEXTS=10
```
{{</details>}}

{{<details "build log">}}
```bash
D:\Dev\nrf52\hsm\hsm\samples\tag_sensors_broadcast>west build -b nrf52840_sensortag -- -DCONF_FILE=prj.conf
-- west build: generating a build system
Including boilerplate (Zephyr base): D:/Dev/nrf52/hsm/zephyr/cmake/app/boilerplate.cmake
-- Application: D:/Dev/nrf52/hsm/hsm/samples/tag_sensors_broadcast
-- Zephyr version: 2.6.0-rc1 (D:/Dev/nrf52/hsm/zephyr), build: v2.6.0-rc1-120-g41e885947e4f
-- Found Python3: C:/Users/User/AppData/Local/Programs/Python/Python39/python.exe (found suitable exact version "3.9.0") found components: Interpreter 
-- Found west (found suitable version "0.10.1", minimum required is "0.7.1")
-- Board: nrf52840_sensortag
-- Cache files will be written to: D:/Dev/nrf52/hsm/zephyr/.cache
-- Found toolchain: gnuarmemb (D:/tools/gnu_arm_embedded/10 2020-q4-major)
-- Found BOARD.dts: D:/Dev/nrf52/hsm/hsm/boards/arm/nrf52840_sensortag/nrf52840_sensortag.dts
-- Generated zephyr.dts: D:/Dev/nrf52/hsm/hsm/samples/tag_sensors_broadcast/build/zephyr/zephyr.dts
...
[536/536] Linking CXX executable zephyr\zephyr.elf
Memory region         Used Size  Region Size  %age Used
           FLASH:      177224 B         1 MB     16.90%
            SRAM:       71200 B       256 KB     27.16%
        IDT_LIST:          0 GB         2 KB      0.00%
```
{{</details>}}

{{<details "run log">}}
```log
*** Booting Zephyr OS build zephyr-v2.5.0-2187-g757cd12e6602  ***
rtt:~$ rtt:~$ [00:00:00.264,617] <inf> fs_nvs: 8 Sectors of 4096 bytes
rtt:~$ [00:00:00.264,617] <inf> fs_nvs: alloc wra: 2, d60
rtt:~$ [00:00:00.264,617] <inf> fs_nvs: data wra: 2, 448
rtt:~$ [00:00:00.275,970] <inf> net_l2_openthread: State changed! Flags: 0x101fc300 Current role: 0
rtt:~$ [00:00:00.276,275] <inf> net_l2_openthread: OpenThread version: OPENTHREAD/gfd27fc3a7; Zephyr; May 13 2021 18:19:06
rtt:~$ [00:00:00.280,517] <inf> net_l2_openthread: Network name: ot_zephyr
rtt:~$ [00:00:00.289,459] <inf> net_l2_openthread: State changed! Flags: 0x0100103d Current role: 1
rtt:~$ [00:00:00.290,344] <inf> VEML6030: veml6030_init()
rtt:~$ [00:00:00.290,344] <inf> MS8607: ms8607_init> i2c_master_init()
rtt:~$ [00:00:00.292,602] <inf> main: Hello Sensors Broadcast
rtt:~$ [00:00:00.292,633] <inf> battery: battery_init() vref = 600
rtt:~$ rtt:~$ [00:00:00.292,968] <inf> main: ms8607> connected
rtt:~$ [00:00:00.292,999] <inf> main: starting loop (0)
rtt:~$ [00:00:00.293,060] <inf> battery: battery_start> adc_read()
rtt:~$ [00:00:00.293,487] <inf> battery: battery_get_mv() raw = 3447
rtt:~$ rtt:~$ [00:00:00.314,117] <inf> net_l2_openthread: State changed! Flags: 0x000010e4 Current role: 2
rtt:~$ [00:00:00.374,145] <inf> net_l2_openthread: State changed! Flags: 0x00000200 Current role: 2
rtt:~$ rtt:~$ thread_tags/7009D837C7BB557A{"alive":0,"voltage":3.029,"light":9.909,"temperature":25.15,"humidity":42.79,"pressure":951.03}
[00:00:01.961,029] <inf> udp_client: CONFIG_NET_IPV6
rtt:~$ [00:00:01.964,843] <inf> main: sleeping 10 sec
rtt:~$ rtt:~$ [00:00:11.964,904] <inf> main: starting loop (1)
rtt:~$ [00:00:11.964,996] <inf> battery: battery_start> adc_read()
rtt:~$ [00:00:11.965,148] <inf> battery: battery_get_mv() raw = 3460
rtt:~$ rtt:~$ thread_tags/7009D837C7BB557A{"alive":1,"voltage":3.041,"light":11.095,"temperature":25.16,"humidity":42.01,"pressure":950.98}
[00:00:13.631,591] <inf> main: sleeping 10 sec
rtt:~$ rtt:~$ [00:00:23.631,652] <inf> main: starting loop (2)
rtt:~$ [00:00:23.631,744] <inf> battery: battery_start> adc_read()
rtt:~$ [00:00:23.631,896] <inf> battery: battery_get_mv() raw = 3460
rtt:~$ rtt:~$ thread_tags/7009D837C7BB557A{"alive":2,"voltage":3.041,"light":10.097,"temperature":25.16,"humidity":41.95,"pressure":951.01}
[00:00:25.298,156] <inf> main: sleeping 10 sec
```
{{</details>}}

Low power performance :

{{<gfigure src="/images/thread_sensortag/deep_sleep_current.png" >}}

{{<table "table table-striped table-bordered">}}
Period | Average Current
-----------|-----
deep sleep | 3.19 uA
sensors acquisition | 54 uA
wakeup cycle 1.7 sec including RF | 125 uA
{{</table>}}

* it's possible to listen to the ipv6 thread udp packets using `socat`
```bash
socat UDP6-LISTEN:4242,fork STDOUT
```
{{<gfigure src="/images/thread_sensortag/socat.png" >}}

* This python script captures Ipv6 UDP packets and forwards their json payload to a configured MQTT broker

{{<icon_button href="https://github.com/HomeSmartMesh/raspi/blob/master/py/thread_tags/thread_tags.py" text="python json string to MQTT" icon="github" >}}

* the config file of the script allows to provide the MQTT broquer and to replace the device id with a friendly name, which is important not to hardcode the id in any source and to avoid firmware updates when moving the sensors from one room to another.

{{<details "python script config">}}
```json
{  
    "mqtt":{
        "host":"10.0.0.42",
        "port":1883,
        "keepalive":60,
        "client_id":"thread_tags",
        "subscriptions":[ ],
        "actions"   :[],
        "publish" :true,
        "subscribe" :false
    },
    "friendlyNames":{
        "7005D83727DD525A":"livingroom tag"
    },
    "log":{
        "logfile":"/home/pi/share/thread_tags_(date).log",
        "level":"Info"
    }
}
```
{{</details>}}


and [another raspi python scripts](https://github.com/HomeSmartMesh/raspi/tree/master/py/influx) forwards them to influxDB to end up in Grafana, which can also [run as a service](https://github.com/HomeSmartMesh/raspi/blob/92f549866482c3a65e2b738ad909ef740f6919e5/run_services.sh#L72).

{{<gfigure src="/images/thread_sensortag/grafana.png" width="200px">}}

### device treee drivers
the driver sensors are declared in the custom board device treee source `nrf52840_sensortag.dts`.
```conf
&i2c0 {
	compatible = "nordic,nrf-twi";
	status = "okay";
	sda-gpios = <&gpio1 15 0>;
	scl-gpios = <&gpio1 13 0>;

	ms8607@40 /*hum@40,press@76*/{
		compatible = "teconnectivity,ms8607";
		reg = <0x40>;
		label = "MS8607";
	};
	veml6030@48 {
		compatible = "vishay,veml6030";
		reg = <0x48>;
		int-gpios = <&gpio0 31 0>;
		label = "VEML6030";
	};
};
```
{{<hint warning>}}The MS8607 uses x2 i2c adresses `0x40` and `0x76`, which makes it require two separate drivers to fit in the Zephyr dts declaration. For simplicity purpose, the vendor driver has been reused with minimal adaptations for Zephyr. Only the adress `0x40` is declared in the dts. That loses the multi instance feature as the adress `0x76` is hardcoded in the driver, the sensor anyway does not have any i2c multi instance configuration option.{{</hint>}}

### tag_sensor_veml6030

```bash
cd hsm/samples/tag_sensor_veml6030
west build -b nrf52840_sensortag -t guiconfig
west build -b nrf52840_sensortag -- -DCONF_FILE=prj-shell.conf
west build -b nrf52840_sensortag -- -DCONF_FILE=prj-log.conf
west flash
```
{{<icon_button href="https://github.com/HomeSmartMesh/sdk-hsm-sensortag/tree/main/samples/tag_sensor_veml6030" text="tag sensor veml6030" icon="github" >}}


{{<details "default config">}}
```conf
CONFIG_GPIO=y
CONFIG_SERIAL=n

CONFIG_I2C=y
CONFIG_SENSOR=y
CONFIG_VEML6030=y

CONFIG_NEWLIB_LIBC=y
CONFIG_NEWLIB_LIBC_FLOAT_PRINTF=y
```
{{</details>}}

{{<details "build log">}}
```bash
-- west build: generating a build system
Including boilerplate (Zephyr base (cached)): D:/Dev/nrf52/hsm/zephyr/cmake/app/boilerplate.cmake
-- Application: D:/Dev/nrf52/hsm/hsm/samples/tag_sensor_veml6030
-- Zephyr version: 2.5.99 (D:/Dev/nrf52/hsm/zephyr)
-- Found west (found suitable version "0.10.1", minimum required is "0.7.1")
-- Board: nrf52840_sensortag
-- Cache files will be written to: D:/Dev/nrf52/hsm/zephyr/.cache
-- Found toolchain: gnuarmemb (D:/tools/gnu_arm_embedded/10 2020-q4-major)
-- Found BOARD.dts: D:/Dev/nrf52/hsm/hsm/boards/arm/nrf52840_sensortag/nrf52840_sensortag.dts
```
{{</details>}}

running the initial versions on ambiant light, hand cover then flash light

{{<details "run log preliminary">}}
```log
[00:00:00.325,683] <inf> VEML6030: veml6030_init() power on
[00:00:00.326,202] <inf> VEML6030: i2c_burst_write(0x0000) success
*** Booting Zephyr OS build zephyr-v2.5.0-2187-g757cd12e6602  ***

[00:00:00.326,324] <inf> main: VEML6030 light sensor application

Found device "VEML6030", getting sensor data

[00:00:00.427,062] <inf> main: sensor: lum reading: 1255

[00:00:05.527,740] <inf> main: sensor: lum reading: 1253

[00:00:10.628,448] <inf> main: sensor: lum reading: 1242

[00:00:15.729,156] <inf> main: sensor: lum reading: 107

[00:00:20.829,864] <inf> main: sensor: lum reading: 111

[00:00:25.930,572] <inf> main: sensor: lum reading: 1304

[00:00:31.031,280] <inf> main: sensor: lum reading: 1482

[00:00:36.131,988] <inf> main: sensor: lum reading: 23082

[00:00:41.232,696] <inf> main: sensor: lum reading: 23791

[00:00:46.333,404] <inf> main: sensor: lum reading: 1257

[00:00:51.434,112] <inf> main: sensor: lum reading: 1365

[00:00:56.534,820] <inf> main: sensor: lum reading: 1354

[00:01:01.635,528] <inf> main: sensor: lum reading: 1351

[00:01:06.736,236] <inf> main: sensor: lum reading: 1349

[00:01:11.836,944] <inf> main: sensor: lum reading: 1274
```
{{</details>}}

running with auto mode

{{<details "run log auto">}}
```log
[00:00:00.324,188] <inf> VEML6030: veml6030_init()
*** Booting Zephyr OS build zephyr-v2.5.0-2187-g757cd12e6602  ***

[00:00:00.324,310] <inf> main: VEML6030 light sensor application
=====> light 3.614 lux
=====> light 3.582 lux
=====> light 7.142 lux
auto_measure>sample 65535 not optimal ; gain = 2.000 ; it = 800 ms
auto_measure>new params => gain = 1.000000 ; it = 800
=====> light 449.741 lux
=====> light 453.672 lux
auto_measure>sample 65535 not optimal ; gain = 1.000 ; it = 800 ms
auto_measure>new params => gain = 0.250000 ; it = 800
=====> light 1502.496 lux
=====> light 1645.315 lux
auto_measure>sample 65535 not optimal ; gain = 0.250 ; it = 800 ms
auto_measure>new params => gain = 0.125000 ; it = 800
=====> light 3081.197 lux
auto_measure>sample 65535 not optimal ; gain = 0.125 ; it = 800 ms
auto_measure>new params => gain = 0.125000 ; it = 400
=====> light 5908.954 lux
auto_measure>sample 24 not optimal ; gain = 0.125 ; it = 400 ms
auto_measure>new params => gain = 2.000000 ; it = 800
=====> light 3.031 lux
=====> light 3.010 lux
=====> light 3.136 lux
=====> light 3.794 lux
=====> light 4.478 lux
=====> light 3.139 lux
```
{{</details>}}

* The previous auto mode has a disadvantage, when saturating, the best guess is still biased as it used a wrong saturated measure.
* The following example makes a special case of a saturated measure and directly jumps to the highest mode which has the lowes integration time anyway so only 25 ms are spent, after which the sample is sure not to be saturated and either has a chance to already be in optimal mode (if it is the highest) or guqrantees that the next selected mode will be the optimal.

{{<details "run log auto sat">}}
```log
[00:00:00.325,836] <inf> main: VEML6030 light sensor application
=====> light 70.247 lux
=====> light 67.090 lux
auto_measure>sample 65535 not optimal ; gain = 2.000 ; it = 800 ms
auto_measure>new params => gain = 0.125000 ; it = 25
auto_measure>sample 487 not optimal ; gain = 0.125 ; it = 25 ms
auto_measure>new params => gain = 0.250000 ; it = 800
=====> light 897.811 lux
=====> light 793.872 lux
auto_measure>sample 2668 not optimal ; gain = 0.250 ; it = 800 ms
auto_measure>new params => gain = 2.000000 ; it = 800
=====> light 69.358 lux
=====> light 69.289 lux
=====> light 68.566 lux
=====> light 68.465 lux
```
{{</details>}}


### tag_sensor_ms8607

```bash
cd hsm/samples/tag_sensor_ms8607
west build -b nrf52840_sensortag -t guiconfig
west build -b nrf52840_sensortag -- -DCONF_FILE=prj.conf
west flash
```
{{<icon_button href="https://github.com/HomeSmartMesh/sdk-hsm-sensortag/tree/main/samples/tag_sensor_ms8607" text="tag sensor ms8607" icon="github" >}}


{{<details "default config">}}
```conf
CONFIG_GPIO=y
CONFIG_SERIAL=n

CONFIG_I2C=y
CONFIG_SENSOR=y
CONFIG_MS8607=y

...

CONFIG_NEWLIB_LIBC=y
CONFIG_NEWLIB_LIBC_FLOAT_PRINTF=y

```
{{</details>}}

{{<details "build log">}}
```bash
Including boilerplate (Zephyr base): D:/Dev/nrf52/hsm/zephyr/cmake/app/boilerplate.cmake
-- Application: D:/Dev/nrf52/hsm/hsm/samples/tag_sensor_ms8607
-- Zephyr version: 2.5.99 (D:/Dev/nrf52/hsm/zephyr)
-- Found Python3: C:/Users/User/AppData/Local/Programs/Python/Python39/python.exe (found suitable exact version "3.9.0") found components: Interpreter
-- Found west (found suitable version "0.10.1", minimum required is "0.7.1")
-- Board: nrf52840_sensortag
-- Cache files will be written to: D:/Dev/nrf52/hsm/zephyr/.cache
-- Found toolchain: gnuarmemb (D:/tools/gnu_arm_embedded/10 2020-q4-major)
-- Found BOARD.dts: D:/Dev/nrf52/hsm/hsm/boards/arm/nrf52840_sensortag/nrf52840_sensortag.dts
-- Generated zephyr.dts: D:/Dev/nrf52/hsm/hsm/samples/tag_sensor_ms8607/build/zephyr/zephyr.dts
...
[135/135] Linking C executable zephyr\zephyr.elf
Memory region         Used Size  Region Size  %age Used
           FLASH:       44688 B         1 MB      4.26%
            SRAM:        8544 B       256 KB      3.26%
        IDT_LIST:          0 GB         2 KB      0.00%
```
{{</details>}}

{{<details "run log">}}
```log
*** Booting Zephyr OS build zephyr-v2.5.0-2187-g757cd12e6602  ***

[00:00:00.325,714] <inf> main: MS8607 Temperature Humidity pressure sensor application
[00:00:00.326,049] <inf> main: ms8607> connected
ms8607> t=22.59 °  p=963.62 mbar  h=38.59 %RH
ms8607> t=22.59 °  p=963.63 mbar  h=38.90 %RH
ms8607> t=22.59 °  p=963.63 mbar  h=38.88 %RH
ms8607> t=22.60 °  p=963.64 mbar  h=39.74 %RH
ms8607> t=22.17 °  p=963.62 mbar  h=51.82 %RH
ms8607> t=22.10 °  p=963.62 mbar  h=58.70 %RH
ms8607> t=22.09 °  p=963.62 mbar  h=60.83 %RH
ms8607> t=23.16 °  p=963.69 mbar  h=72.36 %RH
ms8607> t=22.67 °  p=963.63 mbar  h=77.52 %RH
ms8607> t=22.65 °  p=963.55 mbar  h=53.33 %RH
```
{{</details>}}

### tag_battery

```bash
cd hsm/samples/tag_battery
west build -b nrf52840_sensortag -t guiconfig
west build -b nrf52840_sensortag -- -DCONF_FILE=prj.conf
west flash
```
{{<icon_button href="https://github.com/HomeSmartMesh/sdk-hsm-sensortag/tree/main/samples/tag_battery" text="tag battery" icon="github" >}}


{{<details "default config">}}
```conf
CONFIG_GPIO=y
CONFIG_SERIAL=n

CONFIG_ADC=y
...
CONFIG_NEWLIB_LIBC=y
CONFIG_NEWLIB_LIBC_FLOAT_PRINTF=y

```
{{</details>}}

{{<details "build log">}}
```bash
Including boilerplate (Zephyr base (cached)): D:/Dev/nrf52/hsm/zephyr/cmake/app/boilerplate.cmake
-- Application: D:/Dev/nrf52/hsm/hsm/samples/tag_battery
-- Zephyr version: 2.5.99 (D:/Dev/nrf52/hsm/zephyr)     
-- Found west (found suitable version "0.10.1", minimum required is "0.7.1")
-- Board: nrf52840_sensortag
-- Cache files will be written to: D:/Dev/nrf52/hsm/zephyr/.cache
-- Found toolchain: gnuarmemb (D:/tools/gnu_arm_embedded/10 2020-q4-major)
-- Found BOARD.dts: D:/Dev/nrf52/hsm/hsm/boards/arm/nrf52840_sensortag/nrf52840_sensortag.dts
-- Generated zephyr.dts: D:/Dev/nrf52/hsm/hsm/samples/tag_battery/build/zephyr/zephyr.dts
...
[107/107] Linking C executable zephyr\zephyr.elf
Memory region         Used Size  Region Size  %age Used
           FLASH:       41528 B         1 MB      3.96%
            SRAM:        8544 B       256 KB      3.26%
        IDT_LIST:          0 GB         2 KB      0.00%
```
{{</details>}}

{{<details "run log">}}
```log
*** Booting Zephyr OS build zephyr-v2.5.0-2187-g757cd12e6602  ***

[00:00:00.325,714] <inf> main: Test App for Battery Voltage Measure through ADC
[00:00:00.325,744] <inf> battery: battery_init() vref = 600
[00:00:00.325,805] <inf> battery: battery_start> adc_read()
[00:00:00.425,872] <inf> battery: battery_get_mv() raw = 3764
[00:00:00.425,872] <inf> main: battery> Voltage = 3308 mV
[00:00:05.326,049] <inf> battery: battery_start> adc_read()
[00:00:05.426,086] <inf> battery: battery_get_mv() raw = 3768
[00:00:05.426,116] <inf> main: battery> Voltage = 3311 mV
```
{{</details>}}

# FAQ - Discussion
* If you need support, want to ask a question or suggest a different answer, you can join the discussion on the Github forum
{{<icon_button text="Home Smart Mesh - sdk-hsm-sensortag" href="https://github.com/HomeSmartMesh/sdk-hsm-sensortag/discussions" icon="github" >}}

{{<faq>}}
Does this Thread SensorTag support MQTT ?
<--->
A first firmware was tested with MQTT-SN [MQTT Sensors Node](/docs/networks/thread/#mqtt-sensors-node), but given that it used the nRF SDK and not Zephyr, development was discontinued. As alternative a simple udp protocol with a python scripts can fulfill the broadcast function from openthread to an MQTT broker details in the [udp broadcast](/docs/microcontrollers/nrf52/thread_sensortag/#tag_sensors_broadcast) example.
<===>
Is it possible to save energy by collecting many sensor samples over time and sending them together?
<--->
Yes, sure, the use cases could be slpit as follows :
* For short term buffering, it's possible to use the RAM retention feature, you pay as you go fine granular (per 4 kB block: 30nA)
* For long term buffering, we're no longer on the sensors network use case and rather on the logger tag use case, there you can use the Flash, 1 MB is huge.
In my use case, I don't use buffering in order to allow live update of measures on the user's apps, but combining like a level trigger to immediately send values on big changes with buffering and sending all the values cyclically could be quite smart, you will need a QoS through to ensure your data is really sent after a while, which will also consume RF transactions, I opted for sending every single value on an RF packet which is also used as an alive signal to know when the sensor is dead or out of reach, so no problem if once a packet is lost in a while, but practically, as I use a flood mesh, most measures arrive more than once to the server.
I think after all, such things should be configurable by the end users's app, and should not be hardcoded
<===>
Can this firmware be used on nRF51 ? which families are supported ?
<--->
No, Thread is relaying on the RF MAC layer 802.15.4 which is unfortunately only supported by nRF5340, nRF52840, nRF52833, nRF52820, nRF52811 more details on the nordic [Thread](https://www.nordicsemi.com/Products/Thread) page.
Note that this application is developed for the nRF family but openthread is supported on other chips as well e.g. TI, NXP,... 
{{</faq>}}


