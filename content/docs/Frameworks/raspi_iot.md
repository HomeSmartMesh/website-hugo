---
title: "Raspi IoT"
description: "raspberry pi server for home automation. For developers, startups, students and hobbyist who want to understand the basics of home automation and IoT and use it for their own projects and products. Networks devices using Thread, Zigbee, wifi are are centralized with mqtt then controlled with servies based on python and node.js scripts interacting with web apps as user Front End."
date: 2020-10-06T08:48:57+00:00
lastmod: 2021-02-14T08:00:00+00:00
draft: false
images: ["/images/software.png", "/images/devices.png" ]
weight: 1
toc: true
grid:
    slot: 300
    items:
    - name: "Discourse Forum"
      width: 300
      tags: []
    - name: "Github Repo"
      width: 300
      tags: [Network]
---

{{<grid "my_grid" >}}
{{<image src="/images/discourse.svg" height=60 href="https://homesmartmesh.discourse.group/" >}}
{{<image src="/images/discourse/discourse_simple.png" height=100 href="https://homesmartmesh.discourse.group/" >}}
Support and discussions on the [Forum](https://homesmartmesh.discourse.group/)
<--->
{{<image src="/images/raspi_iot.png" height="170px" href="https://github.com/HomeSmartMesh/raspi" target="_blank">}}

Source code on the [Github Repo](https://github.com/HomeSmartMesh/raspi)

{{</grid>}}


# Hardware
{{< image src="/images/devices.png" width=600 >}}

* Thread
  * See [Thread SensorTag](/docs/microcontrollers/nrf52/thread_sensortag/)
* Zigbee
  * Hue Light with Hue gateway
  * Eurotronics thermostats with zigbee2mqtt
  * Aquara sensors with Zigbee2mqtt
* WiFi
  * ESP32 bed heater
  * ESP32 neopixels led panel
  * Shelly power control and monitor
  * Roborock with valetudeo server : https://github.com/Hypfer/Valetudo
* Custom RF
  * nRF52 sensors with [nRF52 Mesh](https://github.com/nRFMesh/nRF52_Mesh)
* ESP32
  * led panel : [github esp32_iot rgb_led](https://github.com/HomeSmartMesh/esp32_iot/tree/master/rgb_led)
  * bed heating [github esp32_iot bed_heater](https://github.com/HomeSmartMesh/esp32_iot/tree/master/bed_heater)

# Software
{{< image src="/images/software.png" width=600 >}}

* Raspbian OS
* Main services
  * Mosquitto : https://mosquitto.org/
  * influxDB : https://www.influxdata.com/products/influxdb-overview/
  * mqtt2influx : [py/influx](./py/influx)
  * Grafana : https://grafana.com/
  * graphql sensors : [js/graphql](./js/graphql)
* x to mqtt
  * Thread UDP to mqtt : [py/thread_tags](https://github.com/HomeSmartMesh/raspi/tree/master/py/thread_tags)
  * Zigbee2mqtt : https://www.zigbee2mqtt.io/
  * nrf2mqtt : [github nRF52_Mesh nrf_mesh](https://github.com/nRFMesh/nRF52_Mesh/tree/master/raspi/nrf_mesh)
  * miflora2mqtt : https://github.com/RaymondMouthaan/miflora-mqtt-daemon
  * telegram bots2mqtt : [js/telegraf](./js/telegraf) based on https://github.com/telegraf/telegraf
* automation services
  * Light control : [py/hue](./py/hue)
  * Phantom Load cut : [js/pc_control](./js/pc_control)
  * Heat cut : [py/heating](./py/heating)
  * Home status : [py/home_status](./py/home_status)
  * Roborock button : [py/hover](./py/hover)
* user interface
  * 3D webapp : https://github.com/HomeSmartMesh/smart_home_3d_webapp
  * Led panel : [js/leds_panel](./js/leds_panel)
  * Bed heating : [js/bed_heating](./js/bed_heating)
  * Roborock Chat : [js/telegraf/hover_poll.js](./js/telegraf/hover_poll.js)

## raspberry pi setup

* download an os from https://www.raspberrypi.org/software/operating-systems/#raspberry-pi-os-32-bit
* use the Raspberry pi Imager tool to write the image. The tool is available from https://www.raspberrypi.org/software/
* eject, then reinsert the sdcard, write a file on root named `ssh` without extensions
* connect through ethernet, identify the ip from the router connect through ssh with `pi` and pw `raspberry`
* setup the border router

{{<button relref="/docs/networks/thread#border-router">}}border router setup{{</button>}}

* restart then create a network on the border router web GUI in the url of the rasp ip address
* install docker and docker compose
```
  sudo apt-get update && sudo apt-get upgrade
  curl -fsSL https://get.docker.com -o get-docker.sh
  sudo sh get-docker.sh
  sudo usermod -aG docker pi
  sudo reboot now
  sudo apt-get install libffi-dev libssl-dev
  sudo apt install python3-dev
  sudo apt-get install -y python3 python3-pip
  sudo pip3 install docker-compose
```
* clone this repo and start services that will run docker compose `docker-compose-thread.yml` and python services
```
git clone https://github.com/HomeSmartMesh/raspi.git
cd raspi
./start_services.sh
```
these steps still manual can be scripted
* in the influx container, create a database named `mqtt`
```
$influx
>CREATE DATABASE mqtt
```
* in grafana `http://raspi_ip:3000` create an influx datasource on url `http://localhost:8086`
* upload the dashboard `grafana\Dashboards\SensorTag Compare.json`

## Meta website
{{< image src="/images/meta_website.png" width=100% >}}

{{< my_button href="https://github.com/HomeSmartMesh/raspi/tree/master/web/overview" text="website source code" >}}

### What is it ?
* Hugo : a website as easy to customise as writing markdown
* Menu : All webapps and websites can be organised in a hierarchical menu
* Full screen : Each website is embedded on an iframe that can toggle to full screen mode or open in a new page

### How does it work ?
* create a new file : `homeassistant.md`

{{< image src="/images/hugo_md_file.png" width=200 >}}

* call `hugo server` to test or `hugo` to generate the website (more details on [using hugo](https://gohugo.io/getting-started/))
* this will already create a new menu entry

{{< image src="/images/hugo_menu.png" width=200 >}}

* Fill it with the link to your webapp instance

{{< image src="/images/hugo_markdown.png" width=400 >}}

* `weight` : defines the order in the menu entry
* `title` : The text that will appear on the menu entry
* `BookToC` : Table Of Content not required so that the iframe can be wider
* `...{< iframe...` : call of the `iframe` html shortcode file that has been added in the `layouts\shortcodes` directory

{{< image src="/images/hugo_shortcodes.png" width=200 >}}

### Demo video

{{< youtube OUtunnrMKI4 >}}

## Smart home 3D webapp
{{< image src="/images/floor_temperature.png" width=600 >}}

Attempt to unify interfaces in one app. Result is quite a success though requires a beast GPU and quite some effort to arrange 3D details and camera positioning to get a nice user experiance out of it.
* Has it's own github repo : https://github.com/HomeSmartMesh/smart_home_3d_webapp
* Hackaday project : https://hackaday.io/project/169046-smart-home-3d-webapp
* Even a Hackaday Blog entry : https://hackaday.com/2019/12/29/the-smart-home-gains-an-extra-dimension/

## Led Panel webapp
{{< image src="/images/led_panel_app.jpg" width=600 >}}
{{< image src="/images/ledpanel.jpg" width=600 >}}

* Hackaday project : https://hackaday.io/project/162825-neopixels-animations-on-esp32-mqtt-json-webapp

{{< youtube LMJBS6VYZzk >}}


## mqtt2influx
* repo directory : https://github.com/HomeSmartMesh/raspi/tree/master/py/influx

Features :
* easily deploy as a Linux service : https://github.com/HomeSmartMesh/raspi/blob/master/py/influx/influx_mqtt.service
* json config : https://github.com/HomeSmartMesh/raspi/blob/master/py/influx/config.json
* `subscriptions` : topics suscriptions list

```json
        "subscriptions":[   "lzig/living heat",
                            "+/bed weather",
                            "lzig/bedroom heat",
                            "+/balcony window left",
```
* `names` : replace long topic names with friendly names 
```json
        "names":{
            "shellies/shellyplug-s-01E2B5/relay/0/power":"shelly dryer",
```

  * `types` : types enfrorcement : this is an influx design/limitation that it is not possible to change the type after creation, in case a float starts with a round number it locks the type. By using this feature the type is ensured to be correct from the first post.

```json
        "types":{
            "energy"      :"int",
            "power"      :"float",
            "pressure"      :"float",
```

 * `discard` : make sure unwanted data from specific fields are excluded from the influx post although belonging to the wanted topics
```json
        "discard":[
            "path",
            "eurotronic_host_flags",
            "occupancy"
```

* `log` : a log file with a log level
* `last_seen` fitering : mqtt allows persistance of values needed for displaying all sensor values for newly connected clients. This might have the side effect of re-injecting the same old values to the influx database in case of restart of the client. It is possible to avoid that by using this feature that filter posts which `last_seen` field, in case it exists, is fresh and not old.

## Home heating app

### web heat control

{{< image src="/images/demo.gif" width=600 >}}

The control of the heating has a feedback that ensures the execution of the command. The green displayed numbers represent the time since the last status of the zigbee device. Once a slider is modified and a command is sent, a feedback shall be received within few seconds and the time since last message should drop to `0 mn`

### python heat windows monitor

1. adjust your mqtt configuration in [config.json](raspi/heat/config.json)
2. adjust the eurotronic heater topic and apertures (apertures are the contact sensors list)
```json
    "heatings":{
        "living heat":{
            "topic":"lzig/living heat/set",
            "Apertures":[
                "balcony door",
                "balcony window right",
                "balcony window left"
            ]
        }
    }
``` 
3. add the contact sensors to the mqtt subscriptions as well
4. run the script `python raspi/heat.py`

example eurotronic mqtt payload
```json
zig/living heat {
    "current_heating_setpoint":17,
    "eurotronic_system_mode":1,
    "local_temperature":18.49,
    "occupied_heating_setpoint":21,
    "unoccupied_heating_setpoint":16,
    "eurotronic_error_status":0,
    "pi_heating_demand":0,
    "battery":100,
    "linkquality":44
}
```

### docu references

- [slider editing web page](http://danielstern.ca/range.css/#/)
- [slider css docu](https://css-tricks.com/styling-cross-browser-compatible-range-inputs-css/)
- https://www.w3schools.com/jsref/dom_obj_event.asp
- https://www.w3schools.com/jsref/dom_obj_all.asp
- https://patrickhlauke.github.io/touch/
- on release working : https://codepen.io/mhartington/pen/HKGno

## Bed Heating app
{{< image src="/images/bed_heating.png" width=400 >}}

* web app : [js/bed_heater](./js/bed_heating/)

### Bed Heating hardware
{{< image src="/images/esp_32_bed_heater.png" width=200 >}}

* ESP32 firmware : [github esp32_iot bed_heater](https://github.com/HomeSmartMesh/esp32_iot/tree/master/bed_heater)
* using a Shelly 1 pm : https://shelly.cloud/shelly-1pm-wifi-smart-relay-home-automation/
* A standard off the shelf bed heating matrass
* A Solid state relay

## Roborock chat
{{< image src="/images/hover_chat.png" width=600 >}}

* using the amazing node.js telegraf framework : https://github.com/telegraf/telegraf
* Roborock Chat : [js/telegraf/hover_poll.js](./js/telegraf/hover_poll.js)

## Hue Light control

{{< image src="/images/aqara hue zigbee.png" >}}

* Controls all the Hue light bulbs from Aquara sensor switches using the [HUE rest API](https://developers.meethue.com/develop/get-started-2/)
* custom functions
  * Long press for minimal dimming
  * Toggle on of or Toggle dim states

{{< new_button href="https://github.com/HomeSmartMesh/raspi/tree/master/py/hue" text="hue ruler service (Github repo)" >}}

{{< details "Config example. Click for details..." >}}
```json
{  
    "bridges":{
        "LivingRoom":"10.0.0.38",
        "username_config":"/home/pi/.python_hue"
    },
    "mqtt":{
        "host":"10.0.0.42",
        "port":1883,
        "keepalive":60,
        "client_id":"hue_client",
        "subscriptions":[
                            "mzig/bed light button",
                            "mzig/bed nic button",
                            "mzig/living double switch",
                            "lzig/bedroom switch",
                            "lzig/volume white",
                            "lzig/office switch",
                            "lzig/hallway entrance switch",
                            "lzig/hallway living switch",
                            "lzig/hallway corridor switch"
                        ],
        "actions"   :[],
        "publish" :true,
        "subscribe" :true
    },
    "lightmap":{
        "hallway":{
            "action":"light_list_clicks",
            "sensors":[
                "hallway entrance switch",
                "hallway living switch",
                "hallway corridor switch"
            ],
            "lights":[
                "Entrance White 1",
                "Entrance White 2",
                "Entrance White 3"
            ]
        }
    },
    "log":{
        "logfile":"/home/pi/share/hue(date).log",
        "level":"Info"
    }
}
```
{{< /details>}}

{{< image src="/images/3dprinting/dumb_to_smart.png" width="600" >}}
{{< button relref="/docs/3dprinting/light">}} 3D print a Smart Switch Cover {{</button>}}



## Phantom Load cut
* Some home devices especially device clusters such as TV sets and surroundings or PC and surroundings do have a significant cumulated power usage when in a standby mode.
* Who likes to pay money for non used electricity ?
* Who likes to keep remembering each time and toggling switching on locations hard to access ?
* This scrips [js/pc_control](./js/pc_control) comes to the rescue and does this for you in a smart way.

### Features
* configurable standby power level. For example a TV set with sound system consumes 100 W to 200 W but when on standby, still ~ 11 W
* a shelly socket provides a real time power usage through mqtt
* if the device is in standby < 12 W for more than 5 min, the shelly device is ordered to cut the power resulting in 1 W for the shelly device power usage (wifi connection). Therefore a permanent 10 W is saved.
* Low Motion sensors attached to the chair or to the remote control order the shelly to switch on again. False positive is not a problem and only costs 10W standby consumption for 5 more min before cutting again.
* If the TV started really to be used and power is > 12 W, then the standby power cut system is not doing anything so that the user enjoys watching TV until the next TV switch off cycle.

## Heat cut

{{< image src="/images/heat_windows.png" width=600 >}}

* Hackaday page : [Aquara and Eurotronics hand in hand to save energy](https://hackaday.io/project/124114-nrf5-custom-mesh-network/log/168774-aqara-and-eurotronics-hand-in-hand-to-save-energy)

* Opening a window without remembering to switch the heating off can cost a peak of heat that consists of the major power cost of the whole day !
* The smart detection based on temperature drop is no reliable and only acts late sometimes when not necessary.
* With a simple aquara contact switch the system is now reliable
* Python service : [py/heating](./py/heating)

## Home status
{{< image src="/images/shelly_status.png" width=150 >}}
{{< image src="/images/home_status.png" >}}

{{< new_button href="https://github.com/HomeSmartMesh/raspi/tree/master/py/home_status" text="home status service (Github repo)" >}}

* Leaving home, in a hurry, no time to walk through the house or to start smartphone apps to know if the heating is forgotten in one room or if a window is left open in another.
* With this script, you see the home status right next to the house door before leaving
* Any window is open : The status light is blue
* Any heater is active : The status is red
* Both red and blue can be combined

How does this work. In a controlled environemnt where the wifi is reliable, there is no need for wifi status as it is always on. Therefore the led status disable function can be re-used for another purpose. This provide the nicest integration of led status including power supply, mqtt connection without even blocking the socket for other usage.

Below are the control URLs to turn led color on and off
{{< details "json Config example. Click for details..." >}}
```json
    "status":{
        "red":{
            "on":"http://10.0.0.48/settings/?led_power_disable=false",
            "off":"http://10.0.0.48/settings/?led_power_disable=true"
        },
        "blue":{
            "on":"http://10.0.0.48/settings/?led_status_disable=false",
            "off":"http://10.0.0.48/settings/?led_status_disable=true"
        }
    }
```
{{< /details>}}

{{< image src="/images/3dprinting/cover_switch_socket.png" width="400" >}}
{{< button relref="/docs/3dprinting/light_socket">}} 3D print a Switch and Socket Cover {{</button>}}



## Roborock button
As Roborock [valetudo](https://github.com/Hypfer/Valetudo) provides an mqtt interface, all it takes is this script [py/hover](./py/hover) to order cleaning of a room or a section with a click on an aquara zigbee button.

## zigbee/graphview
* Github repo directory

{{<icon_button href="https://github.com/HomeSmartMesh/raspi/tree/master/zigbee/graph_view" text="zigbee/graph_view" icon="github" >}}

* js library used to generate SVG from graph

{{<icon_button href="https://github.com/magjac/d3-graphviz" text="d3-graphviz" icon="github" >}}

* the js library is a port of the graphviz project

{{<icon_button href="https://graphviz.org/" text="graphviz" icon="github" >}}

* An alternative that is also configure in this project's docker-compose file is to use Zigbee2Mqtt-Assistant which in addition to providing a nice devices listing and control also offers a graph view of the network

{{<icon_button href="https://github.com/yllibed/Zigbee2MqttAssistant" text="Zigbee2MqttAssistant" icon="github" >}}
### Gif Demo
{{< image src="/images/zig_demo.gif" width=600 >}}

### Features
* configurable ip, port and mqtt base name
* multiple hosts for those using mulitple zigbee2mqtt instances.
* svg pan zoom for simpleefficient viewing. graphs can be so huge that text gets tiny.
* red buttons for change graph layout algorithm. Depending on the network, some algos can be more convenient than others
* logging : `js/log.io/server.json`https://github.com/NarrativeScience/log.io
* transport-browserconsole https://www.npmjs.com/package/winston-transport-browserconsole

# Safety warning
* Power Sockets have deadly voltage and should not be self altered
* Usage of certified products such as Shelly or others is recommended for any high voltage product
* hacking custom scripts to control equipment might improve safety in case you add power cut off to a lower power for each section and device, but might also alter your food if your fridge inadvertantly goes off due to a wrong configuration
  * reboot state
  * safety power too low
* Any heat power control custom script must necessarily have another safe switch to fully cut the power when away or device is inattended.

# FAQ - Discussion
* If you need support, want to ask a question or suggest a different answer, you can join the discussion on the discourse forum
{{<icon_button text="Home Smart Mesh - Raspi IoT" href="https://homesmartmesh.discourse.group/t/about-the-raspi-iot-category/68" icon="discourse" >}}

{{<faq>}}
how does the Raspi-IoT framework compare to existing frameworks like home assistant or OpenHAB
<--->
Raspi-IoT is intended for developpers that prefer to stick to standard scripting like Python or Node.js and would like to use a minimalistic framework where everything can be debugged with standard tools, and also for sutdents and startups who would like to understand the underlaying techniques of home automation and IoT in order to create their own projects or products.
{{</faq>}}


