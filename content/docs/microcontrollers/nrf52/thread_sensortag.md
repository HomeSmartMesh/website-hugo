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
* design files
{{<icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/tree/v1.1" text="tag v1.1" icon="github" >}}

* gerber files
{{<icon_button text="Gerber files v1.1" href="/data/manufacturing_v1.1.zip" icon="download" >}}

## Board

{{<image src="/images/thread_sensortag/board_top.webp" width="500px" >}}

# Development
## install
* install [zephyr and west](http://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/zephyr/getting_started/index.html#getting-started)

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

## sdk samples
The SDK samples are presented in two folds, first the overall application including all the measures, then a list of basic applications with minimal code for each driver and measure separately

### tag_sensors_broadcast
{{<icon_button href="https://github.com/HomeSmartMesh/sdk-hsm-sensortag/tree/main/samples/tag_sensors_broadcast" text="tag sensors broadcast" icon="github" >}}

{{<gfigure src="/images/thread_sensortag/application.png" >}}

```bash
west build -t guiconfig
west build -b nrf52840_sensortag -- -DCONF_FILE="prj.conf overlay-shell.conf"
west flash
```


{{<details "default config">}}
```conf
CONFIG_ADC=y
CONFIG_BATTERY=y

CONFIG_I2C=y
CONFIG_SENSOR=y
CONFIG_VEML6030=y
CONFIG_MS8607=y

CONFIG_GPIO=y
CONFIG_SERIAL=n

CONFIG_NEWLIB_LIBC=y
CONFIG_NEWLIB_LIBC_FLOAT_PRINTF=y

# Network
CONFIG_NETWORKING=y
CONFIG_NET_L2_OPENTHREAD=y

CONFIG_OPENTHREAD_CHANNEL=13
CONFIG_OPENTHREAD_PANID=4660
CONFIG_OPENTHREAD_NETWORK_NAME="OpenThreadDemo"
CONFIG_OPENTHREAD_XPANID="11:11:11:11:22:22:22:22"
CONFIG_OPENTHREAD_MASTERKEY="00:11:22:33:44:55:66:77:88:99:aa:bb:cc:dd:ee:ff"
CONFIG_OPENTHREAD_FTD=y

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

# Logging
CONFIG_NET_LOG=y
CONFIG_LOG=y
CONFIG_NET_STATISTICS=y
CONFIG_PRINTK=y

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

# Network shell
CONFIG_NET_SHELL=y

```
{{</details>}}

{{<details "build log">}}
```bash
-- west build: generating a build system
Including boilerplate (Zephyr base): D:/Dev/nrf52/hsm/zephyr/cmake/app/boilerplate.cmake
-- Application: D:/Dev/nrf52/hsm/hsm/samples/tag_sensors_broadcast
-- Zephyr version: 2.5.99 (D:/Dev/nrf52/hsm/zephyr)
-- Found Python3: C:/Users/User/AppData/Local/Programs/Python/Python39/python.exe (found suitable exact version "3.9.0") found components: Interpreter
-- Found west (found suitable version "0.10.1", minimum required is "0.7.1")
-- Board: nrf52840_sensortag
-- Cache files will be written to: D:/Dev/nrf52/hsm/zephyr/.cache
-- Found toolchain: gnuarmemb (D:/tools/gnu_arm_embedded/10 2020-q4-major)
-- Found BOARD.dts: D:/Dev/nrf52/hsm/hsm/boards/arm/nrf52840_sensortag/nrf52840_sensortag.dts
-- Generated zephyr.dts: D:/Dev/nrf52/hsm/hsm/samples/tag_sensors_broadcast/build/zephyr/zephyr.dts
...
[558/558] Linking CXX executable zephyr\zephyr.elf
Memory region         Used Size  Region Size  %age Used
           FLASH:      319096 B         1 MB     30.43%
            SRAM:       94016 B       256 KB     35.86%
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

* On the raspberry pi, it's possible to listen to the thread udp packets using `socat`
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
        "7005D83727DD525A":"01"
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

{{<hint danger>}}As of 14 May 2021, the port of Zephyr to nrf52 only supports `PM_STATE_OFF`{{</hint>}}
* the [power.c](https://github.com/zephyrproject-rtos/zephyr/blob/21d1ad3762302b3e461953df59430c77e0709274/soc/arm/nordic_nrf/nrf52/power.c) shows the current implementation of `pm_power_state_set()`
* As of the nRF52840 datasheet section 5.3.3 System OFF mode. This mode cannot be woken up from RTC, but can be woken up by GPIO through DETECT signal.
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
west build -t guiconfig
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
west build -t guiconfig
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
west build -t guiconfig
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


## preliminary test samples

{{<icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/tree/main/firmware" text="repo directory" icon="github" >}}

### 01 dongle nrfsdk mqttsn client

{{<hint info>}} Tested firmware with MQTT-SN gateway and Mosquitto broquer
{{</hint>}}

* example based on `nRFSDK for Thread and Zigbee v4.1.0` which path should be declared in an environment variable `THREAD_SDK_ROOT`
* Note that, as specified in the SDK file `components\toolchain\gcc\Makefile.windows` the used toolchain is `GNU Tools ARM Embedded/7 2018-q2-update`
* Adapted Makefile
* As the usb dongle has only one user key KEY_0, the sequence of `search_gateway`, `connect`, `publish` is not cyclically executed when pressing KEY_0 multiple times.

### 04 dongle pio zephyr blink
* zephyr with platformio environment for easy install
* dongle led blink
* added `boards\nrf52840dongle_nrf52840.json`
* jlink is the default upload method for the nrf52840 boards

### 06 tag zephyr hello
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

### 07 tag zephyr button
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

### 08 tag zephyr thread
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

### 09 tag zephyr i2c
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


## flashing
* SWD pogo pins adapter
* Pogo Pin P75-E2 Dia 1.3mm Length 16.5mm

{{<icon_button text="STL Model" href="/models/thread4_lower.stl" icon="download" >}}
{{< model_viewer "/models/thread4_lower.glb" "400" >}}
{{< gallery dir="/images/thread_sensortag/swd-pogo" />}}


# Produced versions
* v 1.1 15.04.2021

{{<icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/tree/v1.1" text="tag v1.1" icon="github" >}}

* v 1.0 10.02.2021

{{<icon_button href="https://github.com/HomeSmartMesh/nrf52_thread_sensortag/tree/v1.0" text="tag v1.0" icon="github" >}}
{{<gfigure src="/images/thread_sensortag/prototype_top.webp" width="200px" >}}
{{<gfigure src="/images/thread_sensortag/prototype_bottom.webp" width="200px" >}}


# FAQ - Discussion
* If you need support, want to ask a question or suggest a different answer, you can join the discussion on the discord server
{{<icon_button text="Discord - #thread-sensortag " href="https://discord.gg/SdKHaAfKN4" icon="discord" >}}

{{<faq>}}
Does this Thread SensorTag support MQTT ?
<--->
At the moment, a firmware is in preparation that support MQTT-SN which needs an MQTT-SN gateway to connect it to an MQTT broquer. Mode details in the [MQTT Sensors Node](/docs/networks/thread/#mqtt-sensors-node)
<===>
Is it possible to save energy by collecting many sensor samples over time and sending them together?
<--->
Yes, sure, the use cases could be slpit as follows :
* For short term buffering, it's possible to use the RAM retention feature, you pay as you go fine granular (per 4 kB block: 30nA)
* For long term buffering, we're no longer on the sensors network use case and rather on the logger tag use case, there you can use the Flash, 1 MB is huge.
In my use case, I don't use buffering in order to allow live update of measures on the user's apps, but combining like a level trigger to immediately send values on big changes with buffering and sending all the values cyclically could be quite smart, you will need a QoS through to ensure your data is really sent after a while, which will also consume RF transactions, I opted for sending every single value on an RF packet which is also used as an alive signal to know when the sensor is dead or out of reach, so no problem if once a packet is lost in a while, but practically, as I use a flood mesh, most measures arrive more than once to the server.
I think after all, such things should be configurable by the end users's app, and should not be hardcoded
{{</faq>}}


