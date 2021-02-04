---
title: "Software"
description: "ESP32 Software frameworks"
date: 2020-12-25T00:00:00+09:00
weight: 3
---

## Arduino ESP32
* [platform package](https://github.com/espressif/arduino-esp32)
* [as an idf component](https://github.com/espressif/arduino-esp32/blob/master/docs/esp-idf_component.md)

## ESP-IDF
* [esp-idf](https://github.com/espressif/esp-idf)
* [esp-iot-solution](https://github.com/espressif/esp-iot-solution.git)
* [mesh-dev-framework](https://github.com/espressif/esp-mdf)
* [mdf prog guide](https://docs.espressif.com/projects/esp-mdf/en/latest/)

### Over The Air Update

    >get_idf
    >idf.py set-target esp32

{{< new_button href="https://github.com/espressif/esp-idf/tree/master/examples/system/ota" text="OTA Github Readme">}}
    
    >idf.py menuconfig

{{< image src="/images/esp32/ota-config.png" >}}

    https://10.0.0.36:8070/hello-world.bin

    >cp ca_cert.pem ../simple_ota_example/server_certs/
    >idf.py -p /dev/ttyUSB0 flash monitor



on another linux host, copied `ca_cert.pem` then tested :

    >curl -v https://10.0.0.36:8070/simple_ota.bin --cacert ca_cert.pem > simple_ota.bin

{{<image src="/images/esp32/cert-test.png" >}}

Common Name (e.g. server FQDN or YOUR name) []:10.0.0.36

    >openssl req -x509 -newkey rsa:2048 -keyout ca_key.pem -out ca_cert.pem -days 365 -nodes
    >openssl s_server -WWW -key ca_key.pem -cert ca_cert.pem -port 8070

{{< details "Log output, click to expand..." >}}
```bash
wass@wass-vb:~/esp/simple_ota_example$ idf.py -p /dev/ttyUSB0 flash monitor
Executing action: flash
Running ninja in directory /home/wass/esp/simple_ota_example/build
Executing "ninja flash"...
[1/4] Performing build step for 'bootloader'
ninja: no work to do.
[1/2] cd /home/wass/esp/esp-idf/components/esptool_py && /usr/bin/cm...d" -P /home/wass/esp/esp-idf/components/esptool_py/run_esptool.cmake
esptool.py --chip esp32 -p /dev/ttyUSB0 -b 460800 --before=default_reset --after=hard_reset write_flash --flash_mode dio --flash_freq 40m --flash_size 4MB 0x8000 partition_table/partition-table.bin 0xd000 ota_data_initial.bin 0x1000 bootloader/bootloader.bin 0x10000 simple_ota.bin
esptool.py v3.0-dev
Serial port /dev/ttyUSB0
Connecting.....
Chip is ESP32-D0WDQ6 (revision 1)
Features: WiFi, BT, Dual Core, 240MHz, VRef calibration in efuse, Coding Scheme None
Crystal is 40MHz
MAC: 10:52:1c:67:b1:10
Uploading stub...
Running stub...
Stub running...
Changing baud rate to 460800
Changed.
Configuring flash size...
Compressed 3072 bytes to 138...
Writing at 0x00008000... (100 %)
Wrote 3072 bytes (138 compressed) at 0x00008000 in 0.0 seconds (effective 2801.9 kbit/s)...
Hash of data verified.
Compressed 8192 bytes to 31...
Writing at 0x0000d000... (100 %)
Wrote 8192 bytes (31 compressed) at 0x0000d000 in 0.0 seconds (effective 14189.4 kbit/s)...
Hash of data verified.
Compressed 25024 bytes to 15395...
Writing at 0x00001000... (100 %)
Wrote 25024 bytes (15395 compressed) at 0x00001000 in 0.4 seconds (effective 552.6 kbit/s)...
Hash of data verified.
Compressed 815728 bytes to 514732...
Writing at 0x00010000... (3 %)
Writing at 0x00014000... (6 %)
Writing at 0x00018000... (9 %)
Writing at 0x0001c000... (12 %)
Writing at 0x00020000... (15 %)
Writing at 0x00024000... (18 %)
Writing at 0x00028000... (21 %)
Writing at 0x0002c000... (25 %)
Writing at 0x00030000... (28 %)
Writing at 0x00034000... (31 %)
Writing at 0x00038000... (34 %)
Writing at 0x0003c000... (37 %)
Writing at 0x00040000... (40 %)
Writing at 0x00044000... (43 %)
Writing at 0x00048000... (46 %)
Writing at 0x0004c000... (50 %)
Writing at 0x00050000... (53 %)
Writing at 0x00054000... (56 %)
Writing at 0x00058000... (59 %)
Writing at 0x0005c000... (62 %)
Writing at 0x00060000... (65 %)
Writing at 0x00064000... (68 %)
Writing at 0x00068000... (71 %)
Writing at 0x0006c000... (75 %)
Writing at 0x00070000... (78 %)
Writing at 0x00074000... (81 %)
Writing at 0x00078000... (84 %)
Writing at 0x0007c000... (87 %)
Writing at 0x00080000... (90 %)
Writing at 0x00084000... (93 %)
Writing at 0x00088000... (96 %)
Writing at 0x0008c000... (100 %)
Wrote 815728 bytes (514732 compressed) at 0x00010000 in 12.2 seconds (effective 535.5 kbit/s)...
Hash of data verified.

Leaving...
Hard resetting via RTS pin...
Executing action: monitor
Running idf_monitor in directory /home/wass/esp/simple_ota_example
Executing "/home/wass/.espressif/python_env/idf4.3_py3.8_env/bin/python /home/wass/esp/esp-idf/tools/idf_monitor.py -p /dev/ttyUSB0 -b 115200 --toolchain-prefix xtensa-esp32-elf- /home/wass/esp/simple_ota_example/build/simple_ota.elf -m '/home/wass/.espressif/python_env/idf4.3_py3.8_env/bin/python' '/home/wass/esp/esp-idf/tools/idf.py' '-p' '/dev/ttyUSB0'"...
--- idf_monitor on /dev/ttyUSB0 115200 ---
--- Quit: Ctrl+] | Menu: Ctrl+T | Help: Ctrl+T followed by Ctrl+H ---
ets Jun  8 2016 00:22:57

rst:0x1 (POWERON_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:2
load:0x3fff0030,len:4
load:0x3fff0034,len:6928
ho 0 tail 12 room 4
load:0x40078000,len:14328
ho 0 tail 12 room 4
load:0x40080400,len:3660
0x40080400: _init at ??:?

entry 0x40080678
I (32) boot: ESP-IDF v4.3-dev-1561-g357a27760 2nd stage bootloader
I (32) boot: compile time 18:10:59
I (32) boot: chip revision: 1
I (36) boot_comm: chip revision: 1, min. bootloader chip revision: 0
I (43) boot.esp32: SPI Speed      : 40MHz
I (48) boot.esp32: SPI Mode       : DIO
I (52) boot.esp32: SPI Flash Size : 4MB
I (57) boot: Enabling RNG early entropy source...
I (62) boot: Partition Table:
I (66) boot: ## Label            Usage          Type ST Offset   Length
I (73) boot:  0 nvs              WiFi data        01 02 00009000 00004000
I (81) boot:  1 otadata          OTA data         01 00 0000d000 00002000
I (88) boot:  2 phy_init         RF data          01 01 0000f000 00001000
I (96) boot:  3 factory          factory app      00 00 00010000 00100000
I (103) boot:  4 ota_0            OTA app          00 10 00110000 00100000
I (111) boot:  5 ota_1            OTA app          00 11 00210000 00100000
I (118) boot: End of partition table
I (123) boot: Defaulting to factory image
I (127) boot_comm: chip revision: 1, min. application chip revision: 0
I (134) esp_image: segment 0: paddr=0x00010020 vaddr=0x3f400020 size=0x1e510 (124176) map
I (191) esp_image: segment 1: paddr=0x0002e538 vaddr=0x3ffb0000 size=0x01ae0 (  6880) load
I (194) esp_image: segment 2: paddr=0x00030020 vaddr=0x400d0020 size=0x90374 (590708) map
0x400d0020: _stext at ??:?

I (422) esp_image: segment 3: paddr=0x000c039c vaddr=0x3ffb1ae0 size=0x02428 (  9256) load
I (426) esp_image: segment 4: paddr=0x000c27cc vaddr=0x40080000 size=0x00404 (  1028) load
0x40080000: _WindowOverflow4 at /home/wass/esp/esp-idf/components/freertos/xtensa/xtensa_vectors.S:1730

I (429) esp_image: segment 5: paddr=0x000c2bd8 vaddr=0x40080404 size=0x1466c ( 83564) load
I (486) boot: Loaded app from partition at offset 0x10000
I (486) boot: Disabling RNG early entropy source...
I (498) cpu_start: Pro cpu up.
I (498) cpu_start: Starting app cpu, entry point is 0x40081150
0x40081150: call_start_cpu1 at /home/wass/esp/esp-idf/components/esp_system/port/cpu_start.c:124

I (0) cpu_start: App cpu up.
I (512) cpu_start: Pro cpu start user code
I (512) cpu_start: cpu freq: 160000000
I (512) cpu_start: Application information:
I (516) cpu_start: Project name:     simple_ota
I (522) cpu_start: App version:      1
I (526) cpu_start: Compile time:     Feb  4 2021 18:10:53
I (532) cpu_start: ELF file SHA256:  16c58cb2140dc9a6...
I (538) cpu_start: ESP-IDF:          v4.3-dev-1561-g357a27760
I (545) heap_init: Initializing. RAM available for dynamic allocation:
I (552) heap_init: At 3FFAE6E0 len 00001920 (6 KiB): DRAM
I (558) heap_init: At 3FFB7D30 len 000282D0 (160 KiB): DRAM
I (564) heap_init: At 3FFE0440 len 00003AE0 (14 KiB): D/IRAM
I (570) heap_init: At 3FFE4350 len 0001BCB0 (111 KiB): D/IRAM
I (577) heap_init: At 40094A70 len 0000B590 (45 KiB): IRAM
I (584) spi_flash: detected chip: generic
I (588) spi_flash: flash io: dio
I (593) cpu_start: Starting scheduler on PRO CPU.
I (0) cpu_start: Starting scheduler on APP CPU.
I (1037) wifi:wifi driver task: 3ffc1b2c, prio:23, stack:6656, core=0
I (1037) system_api: Base MAC address is not set
I (1037) system_api: read default base MAC address from EFUSE
I (1077) wifi:wifi firmware version: a94a000
I (1077) wifi:wifi certification version: v7.0
I (1077) wifi:config NVS flash: enabled
I (1077) wifi:config nano formating: disabled
I (1087) wifi:Init data frame dynamic rx buffer num: 32
I (1087) wifi:Init management frame dynamic rx buffer num: 32
I (1097) wifi:Init management short buffer num: 32
I (1097) wifi:Init dynamic tx buffer num: 32
I (1107) wifi:Init static rx buffer size: 1600
I (1107) wifi:Init static rx buffer num: 10
I (1107) wifi:Init dynamic rx buffer num: 32
I (1117) wifi_init: rx ba win: 6
I (1117) wifi_init: tcpip mbox: 32
I (1127) wifi_init: udp mbox: 6
I (1127) wifi_init: tcp mbox: 6
I (1127) wifi_init: tcp tx win: 5744
I (1137) wifi_init: tcp rx win: 5744
I (1137) wifi_init: tcp mss: 1440
I (1147) wifi_init: WiFi IRAM OP enabled
I (1147) wifi_init: WiFi RX IRAM OP enabled
I (1157) example_connect: Connecting to N-WLAN...
W (1157) phy_init: failed to load RF calibration data (0x1102), falling back to full calibration

Brownout detector was triggered

ets Jun  8 2016 00:22:57

rst:0xc (SW_CPU_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:2
load:0x3fff0030,len:4
load:0x3fff0034,len:6928
ho 0 tail 12 room 4
load:0x40078000,len:14328
ho 0 tail 12 room 4
load:0x40080400,len:3660
0x40080400: _init at ??:?

entry 0x40080678
I (32) boot: ESP-IDF v4.3-dev-1561-g357a27760 2nd stage bootloader
I (32) boot: compile time 18:10:59
I (33) boot: chip revision: 1
I (37) boot_comm: chip revision: 1, min. bootloader chip revision: 0
I (44) boot.esp32: SPI Speed      : 40MHz
I (49) boot.esp32: SPI Mode       : DIO
I (53) boot.esp32: SPI Flash Size : 4MB
I (58) boot: Enabling RNG early entropy source...
I (63) boot: Partition Table:
I (67) boot: ## Label            Usage          Type ST Offset   Length
I (74) boot:  0 nvs              WiFi data        01 02 00009000 00004000
I (82) boot:  1 otadata          OTA data         01 00 0000d000 00002000
I (89) boot:  2 phy_init         RF data          01 01 0000f000 00001000
I (97) boot:  3 factory          factory app      00 00 00010000 00100000
I (104) boot:  4 ota_0            OTA app          00 10 00110000 00100000
I (112) boot:  5 ota_1            OTA app          00 11 00210000 00100000
I (119) boot: End of partition table
I (123) boot: Defaulting to factory image
I (128) boot_comm: chip revision: 1, min. application chip revision: 0
I (135) esp_image: segment 0: paddr=0x00010020 vaddr=0x3f400020 size=0x1e510 (124176) map
I (192) esp_image: segment 1: paddr=0x0002e538 vaddr=0x3ffb0000 size=0x01ae0 (  6880) load
I (195) esp_image: segment 2: paddr=0x00030020 vaddr=0x400d0020 size=0x90374 (590708) map
0x400d0020: _stext at ??:?

I (423) esp_image: segment 3: paddr=0x000c039c vaddr=0x3ffb1ae0 size=0x02428 (  9256) load
I (427) esp_image: segment 4: paddr=0x000c27cc vaddr=0x40080000 size=0x00404 (  1028) load
0x40080000: _WindowOverflow4 at /home/wass/esp/esp-idf/components/freertos/xtensa/xtensa_vectors.S:1730

I (430) esp_image: segment 5: paddr=0x000c2bd8 vaddr=0x40080404 size=0x1466c ( 83564) load
I (487) boot: Loaded app from partition at offset 0x10000
I (487) boot: Disabling RNG early entropy source...
I (498) cpu_start: Pro cpu up.
I (499) cpu_start: Starting app cpu, entry point is 0x40081150
0x40081150: call_start_cpu1 at /home/wass/esp/esp-idf/components/esp_system/port/cpu_start.c:124

I (483) cpu_start: App cpu up.
I (513) cpu_start: Pro cpu start user code
I (513) cpu_start: cpu freq: 160000000
I (513) cpu_start: Application information:
I (518) cpu_start: Project name:     simple_ota
I (523) cpu_start: App version:      1
I (527) cpu_start: Compile time:     Feb  4 2021 18:10:53
I (533) cpu_start: ELF file SHA256:  16c58cb2140dc9a6...
I (539) cpu_start: ESP-IDF:          v4.3-dev-1561-g357a27760
I (546) heap_init: Initializing. RAM available for dynamic allocation:
I (553) heap_init: At 3FFAE6E0 len 00001920 (6 KiB): DRAM
I (559) heap_init: At 3FFB7D30 len 000282D0 (160 KiB): DRAM
I (565) heap_init: At 3FFE0440 len 00003AE0 (14 KiB): D/IRAM
I (571) heap_init: At 3FFE4350 len 0001BCB0 (111 KiB): D/IRAM
I (578) heap_init: At 40094A70 len 0000B590 (45 KiB): IRAM
I (585) spi_flash: detected chip: generic
I (589) spi_flash: flash io: dio
I (594) cpu_start: Starting scheduler on PRO CPU.
I (0) cpu_start: Starting scheduler on APP CPU.
I (698) wifi:wifi driver task: 3ffc2024, prio:23, stack:6656, core=0
I (698) system_api: Base MAC address is not set
I (698) system_api: read default base MAC address from EFUSE
I (718) wifi:wifi firmware version: a94a000
I (718) wifi:wifi certification version: v7.0
I (718) wifi:config NVS flash: enabled
I (718) wifi:config nano formating: disabled
I (728) wifi:Init data frame dynamic rx buffer num: 32
I (728) wifi:Init management frame dynamic rx buffer num: 32
I (738) wifi:Init management short buffer num: 32
I (738) wifi:Init dynamic tx buffer num: 32
I (748) wifi:Init static rx buffer size: 1600
I (748) wifi:Init static rx buffer num: 10
I (748) wifi:Init dynamic rx buffer num: 32
I (758) wifi_init: rx ba win: 6
I (758) wifi_init: tcpip mbox: 32
I (758) wifi_init: udp mbox: 6
I (768) wifi_init: tcp mbox: 6
I (768) wifi_init: tcp tx win: 5744
I (778) wifi_init: tcp rx win: 5744
I (778) wifi_init: tcp mss: 1440
I (778) wifi_init: WiFi IRAM OP enabled
I (788) wifi_init: WiFi RX IRAM OP enabled
I (788) example_connect: Connecting to N-WLAN...
W (798) phy_init: failed to load RF calibration data (0x1102), falling back to full calibration
I (948) phy: phy_version: 4500, 0cd6843, Sep 17 2020, 15:37:07, 0, 2
I (958) wifi:mode : sta (10:52:1c:67:b1:10)
I (968) wifi:enable tsf
I (968) example_connect: Waiting for IP(s)
I (1568) wifi:new:<5,1>, old:<1,0>, ap:<255,255>, sta:<5,1>, prof:1
I (2308) wifi:state: init -> auth (b0)
I (2308) wifi:state: auth -> assoc (0)
I (2318) wifi:state: assoc -> run (10)
I (2368) wifi:connected with N-WLAN, aid = 9, channel 5, 40U, bssid = 2c:30:33:9c:bf:c0
I (2368) wifi:security: WPA2-PSK, phy: bgn, rssi: -84
I (2368) wifi:pm start, type: 1

I (2378) wifi:AP's beacon interval = 102400 us, DTIM period = 2
W (3098) wifi:<ba-add>idx:0 (ifx:0, 2c:30:33:9c:bf:c0), tid:6, ssn:2, winSize:64
I (3688) example_connect: Got IPv6 event: Interface "example_connect: sta" address: fe80:0000:0000:0000:1252:1cff:fe67:b110, type: ESP_IP6_ADDR_IS_LINK_LOCAL
I (5688) esp_netif_handlers: example_connect: sta ip: 10.0.0.32, mask: 255.255.255.0, gw: 10.0.0.1
I (5688) example_connect: Got IPv4 event: Interface "example_connect: sta" address: 10.0.0.32
I (5698) example_connect: Connected to example_connect: sta
 (6198) example_connect: - IPv4 address: 10.0.0.32
I (6208) example_connect: - IPv6 address: fe80:0000:0000:0000:1252:1cff:fe67:b110, type: ESP_IP6_ADDR_IS_LINK_LOCAL
I (6218) wifi:Set ps type: 0

I (6218) simple_ota_example: Starting OTA example
W (6278) wifi:<ba-add>idx:1 (ifx:0, 2c:30:33:9c:bf:c0), tid:0, ssn:0, winSize:64
I (8168) esp_https_ota: Starting OTA...
I (8168) esp_https_ota: Writing to partition subtype 16 at offset 0x110000
D (26758) HTTP_CLIENT: esp_transport_read returned:-1 and errno:128 
I (26758) esp_https_ota: Connection closed
I (26758) esp_image: segment 0: paddr=0x00110020 vaddr=0x3f400020 size=0x1e558 (124248) map
I (26808) esp_image: segment 1: paddr=0x0012e580 vaddr=0x3ffb0000 size=0x01a98 (  6808) 
I (26808) esp_image: segment 2: paddr=0x00130020 vaddr=0x400d0020 size=0x90374 (590708) map
0x400d0020: _stext at ??:?

I (27008) esp_image: segment 3: paddr=0x001c039c vaddr=0x3ffb1a98 size=0x02470 (  9328) 
I (27018) esp_image: segment 4: paddr=0x001c2814 vaddr=0x40080000 size=0x00404 (  1028) 
0x40080000: _WindowOverflow4 at /home/wass/esp/esp-idf/components/freertos/xtensa/xtensa_vectors.S:1730

I (27018) esp_image: segment 5: paddr=0x001c2c20 vaddr=0x40080404 size=0x1466c ( 83564) 
I (27058) esp_image: segment 0: paddr=0x00110020 vaddr=0x3f400020 size=0x1e558 (124248) map
I (27108) esp_image: segment 1: paddr=0x0012e580 vaddr=0x3ffb0000 size=0x01a98 (  6808) 
I (27108) esp_image: segment 2: paddr=0x00130020 vaddr=0x400d0020 size=0x90374 (590708) map
0x400d0020: _stext at ??:?

I (27308) esp_image: segment 3: paddr=0x001c039c vaddr=0x3ffb1a98 size=0x02470 (  9328) 
I (27308) esp_image: segment 4: paddr=0x001c2814 vaddr=0x40080000 size=0x00404 (  1028) 
0x40080000: _WindowOverflow4 at /home/wass/esp/esp-idf/components/freertos/xtensa/xtensa_vectors.S:1730

I (27318) esp_image: segment 5: paddr=0x001c2c20 vaddr=0x40080404 size=0x1466c ( 83564) 
I (27408) wifi:state: run -> init (0)
I (27408) wifi:pm stop, total sleep time: 2993121 us / 25102797 us

W (27408) wifi:<ba-del>idx
W (27408) wifi:<ba-del>idx
I (27408) wifi:new:<5,0>, old:<5,1>, ap:<255,255>, sta:<5,1>, prof:1
W (27418) wifi:hmac tx: ifx0 stop, discard
I (27448) wifi:flush txq
I (27448) wifi:stop sw txq
I (27448) wifi:lmac stop hw txq
I (27448) wifi:Deinit lldesc rx mblock:10
ets Jun  8 2016 00:22:57

rst:0xc (SW_CPU_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:2
load:0x3fff0030,len:4
load:0x3fff0034,len:6928
ho 0 tail 12 room 4
load:0x40078000,len:14328
ho 0 tail 12 room 4
load:0x40080400,len:3660
0x40080400: _init at ??:?

entry 0x40080678
```
{{< /details>}}

error message (various reasons, certificate not copied, wrong filename,...) :

{{< image src="/images/esp32/ota-error.png" >}}

success message :

{{< image src="/images/esp32/ota-success.png" >}}

server log :
{{< image src="/images/esp32/server.png" >}}

