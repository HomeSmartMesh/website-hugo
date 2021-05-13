---
title: "Home Assistant"
description: "Integration of different Protocols, Networks and Frameworks in Home assistant"
date: 2021-02-13T08:48:57+00:00
lastmod: 2021-02-13T08:48:57+00:00
weight: 4
toc: true
---

{{< image src="/images/hassio.png" height=100 href="https://www.home-assistant.io/" >}}
{{< icon_button href="https://www.home-assistant.io/" text="Official Website" >}}


# Bluetooth
## BLE Bluetooth Low Energy

* This is a simple integration that tracks BLE devices and beacons without even requiring pairing (privacy notice might be required)

{{< icon_button href="https://www.home-assistant.io/integrations/bluetooth_le_tracker/" text="BLE Tracker" icon="new" >}}

# Bluetooth Mesh
* One investigation path for integrating Bluetooth mesh in Home Assistant is bridging it through an ip layer using the `Bearer Layer` of the [BLE Architecture](https://en.wikipedia.org/wiki/Bluetooth_mesh_networking#Architecture)

`BLE Mesh => IP => MQTT-SN => MQTT => Home Assistant`

# MQTT

* MQTT : Message Queuing Telemetry Transport, is a first class citizen in Home Assistant

{{< icon_button href="https://www.home-assistant.io/integrations/mqtt/" text="MQTT Integration" icon="new" >}}

# Zigbee

* Zigbee : Low power mesh network, allows to extend the network with repeater devices e.g. zigbee light bulbs. This Home Assistant inetgration is based on [Zigpy](https://github.com/zigpy/zigpy)

{{< icon_button href="https://www.home-assistant.io/integrations/zha/" text="MQTT Integration" icon="new" >}}

# Thread
## CHIP - Connected Home Over IP

* I'm still analyzing the options how CHIP can or will be integrated in Home Assistant

{{< icon_button relref="/docs/frameworks/chip/" text="Project CHIP">}}

## MQTT-SN over Thread
* MQTT-SN integration should be trensparent to Home Assistant and treated as any other MQTT connection
* MQTT-SN is a light weight protocol that plays the role of a bridge between an MQTT broker and constrained devices that need to minimise payload and packets traffic. Therefore it's appropriate for Low power wirelss mesh netwroks such as Thread

{{< button relref="/docs/networks/thread#mqtt-sn" >}}Thread - MQTT-SN{{</button>}}
