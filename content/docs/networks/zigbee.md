---
title: "Zigbee"
description: ""
lead: ""
date: 2020-10-06T08:48:57+00:00
lastmod: 2020-10-06T08:48:57+00:00
draft: false
images: ["/images/zigbee_devices.png"]
weight: 4
toc: true
---

## Devices Overview

Below are examples of used devices in this project 

{{<figure src="/images/zigbee_devices.png" width=600 >}}

As in the above figure, I kept all the light with the hue gateway for a reliable function, and opted for zigbee2mqtt for all other non lighting devices.

For a detailed list of used devices and their documentation see the [devices list](#devices-list) section

## Frameworks
### Philips Hue
{{< image src="/images/hue.png" class="center" width=150 >}}

✅ Philips Hue have an open API that can run in a local network, therefore allowing third party apps integration. This API has been tested in this project, it is of professional grade and reliable.

{{< my_button href="https://developers.meethue.com/develop/get-started-2/" text="meethue developers" >}}

* [Hue Light Control](/docs/raspi/readme/#hue-light-control) from Aquara wall switches and buttons
* The [Smart Home3D webapp](/docs/applications/home3d/) is using the hue API, and advanced light groups dimming with 3d sliders

### Zigbee2mqtt
✅ The most popular framework that enables DIY fans to use even non-standard devices on standard gateways

❌ as this project is diconnected from the devices vendors integration testing, some instabilities and interoperabilites issues might be expected for some devices

{{< my_button href="https://www.zigbee2mqtt.io/" text="zigbee2mqtt.io" >}}

#### Useful commands

```bash
> cd /opt/zigbee2mqtt
> ./update
> sudo systemctl restart zigbee2mqtt
> nano /opt/zigbee2mqtt/data/configuration.yaml
> nano /opt/zigbee2mqtt/data/database.db
> pub 'lzig/bridge/config/permit_join' -m 'true'
> pub 'lzig/bridge/config/force_remove' -m '0x00158d0012341234'
```
#### Links

* [github repo](https://github.com/Koenkk/zigbee2mqtt)
* [install](https://www.zigbee2mqtt.io/getting_started/running_zigbee2mqtt.html)
* [supported devices](https://www.zigbee2mqtt.io/information/supported_devices.html)
* [firmware](https://github.com/Tropicao/zigbridge/blob/master/doc/firmware_instructions.md)
* [mqtt messages](https://www.zigbee2mqtt.io/information/mqtt_topics_and_message_structure.html)


## Standard
### Specification
* ZLL : Zigbee Light Link, Easy - No Coordinator `13001r00ZB_MWG-ZigBee_Light_Link_Standard.pdf`
* ZHA : Zigbee Home Automation `075367r03ZB_AFG-Home_Automation_Profile_for_Public_Download.pdf`, see also Silicon Labs ZLL Fundamentals `UG103.9`
* ZGP : Zigbee Green Power
* HAP : Homekit Accessory Protocol
* CLIP : Connected Light Internet Protocol
* ZHA + ZLL + … `webcom_zigbee_rg2017.pdf`
* Security `us-15-Zillner-ZigBee-Exploited-The-Good-The-Bad-And-The-Ugly-wp.pdf`
* Zigbee Pro 2015 : `docs-05-3474-21-0csg-zigbee-specification.pdf`
* ZCL : Zigbee Cluster Library `07-5123-06-zigbee-cluster-library-specification.pdf`
* Zigbee 3 : Zigbee 3.0 Base Device Behavior Specification `docs-13-0402-13-00zi-Base-Device-Behavior-Specification-2.pdf`

For the latest version of the standard documents please refer to the Zigbee alliance official website
{{< my_button href="https://zigbeealliance.org/" text="zigbeealliance.org" >}}

### Layers Architecture
In this Figure from the `Zigbee Pro 2015 Page2`, We see the multiple layers, mainly the physical, the MAC, the network and the application. It is therefore not appropriate to compare Zigbee with other networking protocols such as wifi given that the zigbee includes standardisation of the Application layer (APL)
{{< image src="/images/zigbee_stack_arch.png" class="center" width=100% >}}

## Firmware
### ZBOSS

ZBOSS 3.0 and 2.1 are independent, portable, high-performance Zigbee® software protocol stacks

{{< my_button href="https://dsr-zboss.com/" text="dsr-zboss.com" >}}

### Nordic integration

The ZBOSS is used in Nordic devices either as lower stack with the application running on the same SoC or used as a Network Coprocessor

{{< figure src="/images/zigbee_platform_design_split.svg" width=600 >}}

{{< my_button href="https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_tz_v4.0.0%2Findex.html" text="Nordic Zigbee design" >}}

### TI Z-Stack
{{< image src="/images/stack_3_0.jpg" class="center" width=200 >}}


This is the Stack used by [Zigbee2mqtt](#zigbee2mqtt)

{{< my_button href="https://www.ti.com/tool/Z-STACK" text="TI Z-Stack" >}}

reference documents to be accessed through TI website
* ressource guide
* Z-Stack ZNP (ZNP Protocol)
* Z-Stack Monitor and Test API

## Wireshark sniffing

* Observing the network is mandatory for experiemnting with zigbee devices, as it's sometimes not possible to identify the source of the issue (end device or coordinator) without looking at the air messages.
* Note that an RF sniffer for 802.15.4 is enough to sniff zigbee as the zigbee higher layer protocol dissection is supported by wireshark

* [nRF Sniffer for 802.15.4](https://github.com/NordicSemiconductor/nRF-Sniffer-for-802.15.4)
* [TI packet sniffer](https://www.ti.com/tool/PACKET-SNIFFER)


#### Filters
| Scope | Expression |
| - | - |
| Zigbee	 | zbee_nwk |
| IEEE 802.15.4 | wpan.src64 |
| Zigbee Network | zbee_nwk.src64 |
| Short Address | zbee_nwk.src |
| Short Address | zbee_nwk.dst |
|  | 	zbee_nwk && (zbee_nwk.src64 != 00:12:4b:00:18:00:00:00) && (zbee_nwk.src64 != |  |00:12:4b:00:18:00:00:00) |
|  | 	zbee_nwk && (wpan.dst16 != 0xffff) && !((wpan.src16 == 0x0000) &&(wpan.dst16 == 0xd9be)) |
|  | 	zbee_zdp.ext_addr == 00:15:8d:00:02:00:00:00 |
|  | 	wpan.dst_pan == 0x35ad |
|  | 	!(wpan.src_pan == 0x1a62) && !(wpan.dst_pan == 0xffff) && !(wpan.dst16 == 0xffff) |
|  | 	!(wpan.src_pan == 0x2c52) && !(wpan.dst_pan == 0x2c52) && !(wpan.dst_pan == 0xffff) && !|  |(wpan.src_pan == 0xffff) |
| ack | (wpan.frame_type == 0x2) |
| exlude broadcasts | (wpan.dst16 != 0xffff) |
| no route no bcst | zbee_nwk && (wpan.dst16 != 0xffff) && (wpan.src16 != 0xd9be) && (wpan. |dst16 | != 0xd9be) |
| coord to router | !((wpan.src16 == 0x0000) &&(wpan.dst16 == 0xd9be)) |

#### Coloring rules

`MAC,	wpan.src64 == 00:15:8d:00:00:11:22:33`

## Devices List

### TI MCUs

| Chip | Feature |
| ---- | ------- |
| [CC2531](https://www.ti.com/product/CC2531) | USB, 8051, IEEE 802.15.4 & Zigbee |
| [CC2530](https://www.ti.com/product/CC2530) | No USB, 8051 |
| CC2591 | range extender |
| CC2592 | range extender |
| CC2533 | remote control |
| [CC2538 datasheet](https://www.ti.com/lit/ds/swrs096d/swrs096d.pdf) | USB, ARM M3, + 6LoWPAN |
| [CC2630 datasheet](https://www.ti.com/lit/ds/symlink/cc2630.pdf) | ARM-M3, ARM-M0, No USB |
| CC2652R | ARM-M4F, ARM-M0, No USB |

* instructive reference : [TI wireless connectivity](https://www.ti.com/lit/wp/swry010a/swry010a.pdf)

### TI Dongles

{{< image src="/images/CC2531EMK.png" width=200 >}}

* [CC2531EMK](https://www.ti.com/tool/CC2531EMK)
* [Zigbee2mqtt guide](https://www.zigbee2mqtt.io/information/supported_adapters.html#texas-instruments-cc2531)

### TI based dongles
#### GB2530
{{< image src="/images/GB2530.png" width=300 >}}
This dongle has an additional amplifier chip.

* GBAN GB2530-S = CC2530 + RFX2401
* GBAN GB2530-H = CC2530 + CC2591
* [Aliexpress](https://de.aliexpress.com/item/4000112752493.html?spm=a2g0s.9042311.0.0.27424c4dL0oeWU)

required config for zigbee2mqtt
```ini
advanced:
  rtscts: false
```

#### CC2652 based
This dongle is based on the 2nd generation chips that are more powerful (including ARM-M4F and ARM-M0) also has more memory to handle more zigbee paired devices.

{{< image src="/images/CC2652.png" width=200 >}}
</br>
{{< my_button href="https://www.tindie.com/products/electrolama/zzh-cc2652r-multiprotocol-rf-stick/" text="Product page" >}}



* [Zigbee2mqtt guide](https://www.zigbee2mqtt.io/information/supported_adapters.html#electrolama-zig-a-zig-ah-zzh)

### Nordik Dongle dev kit
Unfortunately not supported by Zigbee2mqtt, but there's chance in case they move to a ZBOSS common stack. Nevertheless, it's possible to use it as a sniffer give the zigbee being the stame standard in the air physical layer.

{{< image src="/images/nrf52840-usb-dongle.png" width=200 >}}
</br>
{{< my_button href="https://www.nordicsemi.com/Software-and-tools/Development-Kits/nRF52840-Dongle" text="Product page" >}}


* cheap, around 9€ [on Mouser](https://www.mouser.de/ProductDetail/Nordic-Semiconductor/nRF52840-Dongle?qs=gTYE2QTfZfTbdrOaMHWEZg==)

* See also its usage in the [custom mesh framework](/docs/networks/nrf/#08-usb-dongle-nrf52840-dongle).

### Eurotronics Heating

{{< image src="/images/eurotronics_heat.png" width=200 >}}
</br>
{{< my_button href="https://eurotronic.org/produkte/zigbee-heizkoerperthermostat/spirit-zigbee/" text="Product page" >}}

* [User Manual](https://eurotronic.org/wp-content/uploads/2019/01/Spirit_ZigBee_BAL_web_DE_view_V9.pdf) including details about the used clusters and attributes

usefull commands
```bash
> pub 'lzig/office heat/set' -m '{"current_heating_setpoint": 9}'
> pub 'zig/living heat/set' -m '{"current_system_mode": 16}'
> pub 'zig/living heat/set' -m '{"current_system_mode": 32}'
```

Provided measures

* current_heating_setpoint
* occupied_heating_setpoint
* unoccupied_heating_setpoint
* eurotronic_system_mode
* pi_heating_demand
* eurotronic_error_status
* battery
* linkquality
* local_temperature

Used in applications

* [Home heating app](/docs/raspi/readme/#home-heating-app)
* [Heat cut rule](/docs/raspi/readme/#heat-cut)

❌ Unresolved mystery : it happened few times in 2 years that a device starts spontaneously heating with full power after a sort of connection loss or self reset. Not funny when this happens in summer, especially when someone is not at home. No answer from the support.

