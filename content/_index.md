---
title : "Home Smart Mesh"
description: "Overview of the website content"
date: 2020-10-06T08:48:23+00:00
lastmod: 2020-10-06T08:48:23+00:00
BookToC: false
---
## Home Automation Mind Map

{{< svg-pan-zoom "/images/home automation.svg" >}}

# Networks

{{< center-col >}}
## Custom Mesh
{{< figure src="/images/nRF52832_sensortag.png" height=100 >}}
{{< btn-local href="/docs/networks/nrf/" text="nRF Mesh Doc" >}}

✅ A simpler, more efficient alternative to Z-x, BL-x, Thread, standard RF protocols.

✅ Open source SW is provided for off the shelf nRF SoC tags and dongles.

<--->
## Wifi
✅ Universal, Simple, secure

❌ Range issues and password hussle

<--->
## Zigbee
{{< figure src="/images/zigbee.png" height=100 >}}
{{< btn-local href="docs/networks/zigbee/" text="Zigbee Doc" >}}
✅ Mesh, wider range, reliable compared to wifi

✅ Low Power

❌ difficult pairing and attributes discovery unreliable


{{< /center-col >}}

{{< center-col >}}
## Thread
✅ IPv6 generic gateway

<--->
## Bluetooth
{{< /center-col >}}

{{< image src="/images/networks_deployment.png" width=600 >}}
When deploying multiple networks, it's important to select non interfering channels.
Above is an example deployment of wifi and  zigbee along a custom RF.

# Frameworks
{{< center-col >}}

## Custom DIY
{{< figure src="/images/pi_logo.png" height=100 >}}
{{< my_button href="docs/raspi/readme/" text="Raspi Doc" >}}

Best way to learn and dive deep. Sensors to MQTT to influx to Grafana. With Python and Node.js scripts ruling the automation.

<--->
## Home Assistant
{{< figure src="/images/hassio.png" height=100 >}}
{{< my_button href="https://www.home-assistant.io/" text="website" >}}

<--->
## OpenHAB
{{< figure src="/images/openhab.png" height=100 >}}
{{< my_button href="https://www.openhab.org/" text="website" >}}

{{< /center-col >}}


# Applications

{{< center-col >}}
## Webapps
{{< figure src="/images/floor_temperature.png" height=100 >}}

{{< btn-local href="docs/webapps/home3d/" text="Webapps Doc" >}}

Smart Home 3d : Control your home like a Video Game served from your local pi. See your 3d rooms model real time colored with your temperature sensor.


<--->

Voronoi Editor, Graphysics

{{< /center-col >}}


## Home Automation Devices

{{< svg-pan-zoom "/images/devices.svg" >}}

{{< columns >}} 

Here we can see a partitioning into Vendors specific Gateways and Open Gateways for which the Raspberry pi is an ideal open Hardware standard.

The same partitioning is done for the devices,  where the ESP32 and the Arduino are a major player in the DIY electronics. More and more users are crossing the step of bravery of building their own garage opener, which might be less secure as a protocol, but as it is limited to local network can occur much less risk than vendors devices that could be Hacked from the other side of the world.
{{< /columns >}}

{{< columns >}} 


## Networks
Ethernet and wifi being used by most generic consumer devices is very convenient and is used wherever it is possible. Even smaller devices with relatively low power (ESP32) can use wifi, which solves all communication questions. Usage of websocket allow browsers and webapps to be independent from proxy servers which increases the flexibility of the client servers architecture and allows clients to run on any device (Smartphone / PC : mac, win,linux).  MQTT lowers the application overhead in comparison to REST API and solves all communication scenarios with client polling and server based events notification.

The 2.4 GHz range clusters in two groups, user centered environment and Autonomous server based environment. The user, expected to be armed with a smartphone, has placed the Bluetooth as main protocol for device to user direct interactions. Although Bluetooth is catching up by adding support for Mesh, this mixes of concerns in order to win market over mesh based devices has kept support for Bluetooth mesh marginal. Zigbee, based on the 802.15.4 standard, has well succeeded into providing products suited for home applications. Currently support is added for sub GHz, but most devices operate in the 2.4 GHz. Limitations from both frequency and transmission power have been solved by the mesh protocol, where light bulbs, as first target of Zigbee, play the role of routers. If to think about a mesh at home, what a better location for a router than in the center of the room, under the ceiling for obstacle avoidance and in a powered spot to allow permanent radio listening and routing, and this is where the light bulbs are usually placed.

Zigbee has been struggling with profiles compatibility which challenges users’s understanding and trust of compatibility. The standard’s focus moved from ZLL (Zigbee Light Link) to ZHA (Zigbee Home Automation), then to Zigbee 3.0 keeping retro compatibility at device level (not routing level). Is Thread going to supersede Zigbee ? The question is rather when ? Thread, using ipv6, provides a generic resolution of addresses  between local networks and internet which is standard define and vendor free. In order to catch up and not to lose the market, the Zigbee alliance are pushing the “dot dot” to grow to application level of both protocols, and hope to allow products smoothly transit and support both. At the same time, Thread comes with concept allowing MQTT-SN (Sensor’s Network) to run at the node level, which does not fit in the zigbee profile.

How can a user invest in a home equipment without getting deprecated before the installation is complete ? How can a user use his favorite User Interface with his favorite Light bulbs and Sensors ? And this without requiring Hacker’s skills ? These are still open questions.

<--->
## Frameworks, Applications, Assistants
Two main frameworks appeared in order to answer the compatibility issues and interoperability problems. OpenHAB and Home Assistant. Both try to be compatible with most smart devices available in the market, and provide a common interface that allow control and creation of rules.  While the success is undeniable for the compatibility and control of many devices, the user interface still obey the rules of physics, being either ugly and functional, nice looking but annoying to use, or both nice and practical but require a genius skills for configuration. It is legitimate to try to invent a new language for writing rules, so that the non developers have to learn a special programming language anyway, one they cannot use anywhere other than in one app, and the developers are extremely annoyed with the limitations, still have to memorize more key words and deal with issues of immature programming concepts only observed in the early stages of standard languages development. The custom environments, if they have a well isolated configuration  or one custom prepared configuration, they can compete with open frameworks or even with consumer grade applications.

Philips Hue and Phoscon are listed here because the light bulbs are the first successful product in home automation. By successful, it is meant that the product is truly smart, without degradation of the basic functionality, like a PC that needs 5 minutes to boot that makes you wish you sent the mail by post. Philips Hue has the limitation of not having a concept for event based API notification. Despite their connection to closed protocol IFTTT, they do not allow PUTs to custom local URLs. Phoscon on the other hand, do provide a websocket clean concept to notify of events.

Assistants are listed here as part of the Automation scene, Alexa, Google and Apple Home. These assistants are here to help on particular circumstances where voice control is suited. Two regressions in the quality of life can happen from their usage. First, if the voice becomes the only way to actuate a device, that turns things as simple and quick as pressing a button into shouting at night and begging and repeating just to switch a TV on. Second, all complex orders that would require  complete paragraph of description to get you what you want, while with a slider, the space geometry is intuitive enough to save you that pain. Same for selecting a particular channel, or opening the window just enough to let a bit of wind and not too much not to let a cat through.

The smartest environment is definitely the one that knows what you want so that you don’t need to ask. Know’s that you’re reading a book so adjusts the light on reading level where you are, so the less screens and the less talk required, the smarter is the environment.

{{< /columns >}}

