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
    - name: "Discussion Forum"
      width: 300
      tags: []
    - name: "Github Repo"
      width: 300
      tags: [Network]
---
{{<load-photoswipe >}}

{{<grid "my_grid" >}}
{{<image src="/images/discourse.svg" height=60 href="https://github.com/HomeSmartMesh/raspi/discussions" >}}
{{<image src="/images/discourse/discourse_simple.png" height=100 href="https://github.com/HomeSmartMesh/raspi/discussions" >}}
Support and discussions on the [Github Discussions](https://github.com/HomeSmartMesh/raspi/discussions)
<--->
{{<image src="/images/raspi_iot.png" height="170px" href="https://github.com/HomeSmartMesh/raspi" target="_blank">}}

Source code on the [Github Repo](https://github.com/HomeSmartMesh/raspi)

{{</grid>}}

# Home Webapp
This `mobile first` webapp from the [web/home-next](https://github.com/HomeSmartMesh/raspi/tree/master/web/home-next) directory is based on [next.js](https://nextjs.org/), [react](https://reactjs.org/), [Material UI](https://mui.com/) and [MQTT.js](https://github.com/mqttjs/MQTT.js#readme). All of the features are running on a raspberry pi as a server with services from a deployed instance of this repo.

{{< image src="/images/home-next.gif">}}


* The Heat panel controls the temperature with `Eurotronics` Zigbee valves running on [Zigbee2mqtt](https://zigbee2mqtt.io/). The room humidity is also shown from [nRF custom sensors](https://github.com/nRFMesh/nRF52_Mesh)
* The sound panel controls sonos system to switch it on and off through `Tuya` smart sockets, and it controls the volume through [node-sonos-http-api](https://github.com/jishi/node-sonos-http-api). The sockets can also be controlled with an `Aqara` button.
* The power panel controls `Tuya` smart sockets and displays the power consumption. On and Off images are updated depending on the state. The PC socket is switched on with an `Aqara` button and switched off with an auto power off [pc_control](https://github.com/HomeSmartMesh/raspi/tree/master/js/pc_control) script.
* The Ambient Panel is a Grafana dashboard that shows the last day charts of humidity, temperature and light from the [nRF custom sensors](https://github.com/nRFMesh/nRF52_Mesh)
* The Energy panels is also a Grafana dashboard that shows cumulated energy for the last 24h or last 30d, combining energy measures from `Shelly plug s` wifi and `Tuya` zigbee sockets.


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
* Base services
  * Mosquitto : from [mosquitto.org](https://mosquitto.org/) installed with a docker container
  * influxDB : from [influxdata.com](https://www.influxdata.com/products/influxdb-overview/) installed with a docker container
  * mqtt2influx : [py/influx](#mqtt2influx) is a python script service that sends configured mqtt topics to influxdb
  * Grafana : from [grafana.com](https://grafana.com/) installed with a docker container and provisioned with a default datasource and dashboard
* x to mqtt
  * Thread UDP to mqtt : [thread_tags](#thread_tags)
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
* others
  * graphql sensors : experimental service, details in [js/graphql](https://github.com/HomeSmartMesh/raspi/blob/master/js/graphql/readme.md)


## raspberry pi setup
It is not necessary but recommended to perform the install on a new sd card image, that is how these scripts have been tested.
  {{<details "Image creation and ssh connection" >}}
  1. download an os from https://www.raspberrypi.org/software/operating-systems/#raspberry-pi-os-32-bit tested with `2021-05-07-raspios-buster-armhf-lite.img`

  2. use the Raspberry pi Imager tool to write the image. The tool is available from https://www.raspberrypi.org/software/

  3. eject, then reinsert the sdcard, write a file on root named `ssh` without extensions
    **Optional**: Enable WiFi"
      Create a file called `wpa_supplicant.conf` on root
      ```
        country=US # Your 2-digit country code
        ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
        network={
            ssid="YOUR_NETWORK_NAME"
            psk="YOUR_PASSWORD"
            key_mgmt=WPA-PSK
        }
      ```

  4. connect through ethernet, identify the ip from the router connect through ssh with `pi` and pw `raspberry`. 
  In case the same raspberry pi was already known under a different host, make sure to delete it from `~/.ssh/known_hosts`.
  {{</details>}}

1. download and run the `get_raspi.sh` with the following commands
    ```shell
    curl https://raw.githubusercontent.com/HomeSmartMesh/raspi/master/get_raspi.sh -o get_raspi.sh && sudo bash get_raspi.sh
    ```
  {{<details "Script Details" >}}
  The `get_raspi.sh` script will run the following
  * `apt-get update` and `apt-get upgrade` with auto-confirmation `-y` option
  * install git if not available
  * clone the raspi_iot repo
  * run the `raspi/setup.sh` script
  {{</details>}}

2. the setup script will need to be relaunched every time the raspi reboots (2 times)
    ```shell
    cd raspi
    sudo bash setup.sh
    ```
  {{<details "Script Details" >}}
  The `raspi/setup.sh` script will run the following
  * check if `docker` is available otherwise install it
  * check if `docker-compose` is available otherwise install it
  * check if `openthread` is available otherwise install it
  * if any or all of the services above were install a reboot will be performed before continuing, in which case the script has to be run again manually
  * when the script is run and all of the above services are availalble, the script will proceed with the last step of launching the `setup_thread_services.sh`

  The `setup_thread_services.sh` script will perform the following
  * start the docker compose file that includes docker images for `mosquitto`, `influx` and `grafana`
    * the grafana container have the config and provisioning mapped from the raspi `grafana` directory
    * the grafana has porivioned the databse `mqtt` from `http://localhost:8086`
  * install the `influx_mqtt` which is an mqtt to influx python service, then creates an influx databse named `mqtt`
  * install the `thread_tags` udp-v6 to mqtt python service
  {{</details>}}
### Openthread Network Configuration
* discover the raspi ip address from the router (will be used to replace `<raspi_ip>`) then navigate on a browser on `http://<raspi_ip>`, the OT Border Router web GUI should appear. Use it to form a network e.g. (channel 18, panid 0x4412,...). Make sure that the same parameters are used in the thread sensor tag firmware.
{{<gfigure src="/images/thread_sensortag/form_network.png" width="300px" >}}

* the grafana dashboard `SensorTag Compare` should be accessible from `http://<raspi_ip>:3000`

## Main Webapp
see [Home Webapp](#home-webapp) above.
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

## thread tags

{{<gfigure src="/images/thread_sensortag/application.png" >}}

this python script listens to an ipv6 udp port and takes a text in the form of `mqtt_topic{json_payload}` then broadcasts the `json_payload` on the `mqtt_topic` of the configured mqtt broquer in the [config.json]() file

usage with a thread sensor gag
{{<button relref="/docs/microcontrollers/nrf52/thread_sensortag#tag_sensors_broadcast">}}tag sensors broadcast{{</button>}}

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

## zigbee
### graphview
* Github repo directory

{{<icon_button href="https://github.com/HomeSmartMesh/raspi/tree/master/zigbee/graph_view" text="zigbee/graph_view" icon="github" >}}

* js library used to generate SVG from graph

{{<icon_button href="https://github.com/magjac/d3-graphviz" text="d3-graphviz" icon="github" >}}

* the js library is a port of the graphviz project

{{<icon_button href="https://graphviz.org/" text="graphviz" icon="github" >}}

* An alternative that is also configure in this project's docker-compose file is to use Zigbee2Mqtt-Assistant which in addition to providing a nice devices listing and control also offers a graph view of the network

{{<icon_button href="https://github.com/yllibed/Zigbee2MqttAssistant" text="Zigbee2MqttAssistant" icon="github" >}}
### zigbee2mqtt
* bind usb device to a static name, failure to do so can result on occasional total failure where usb devices gets mixed and none of them can operate
* hints from [stackexchange post](https://unix.stackexchange.com/questions/66901/how-to-bind-usb-device-under-a-static-name)

* get info with
```
udevadm info -a -p  $(udevadm info -q path -n /dev/ttyUSB1)
```
* edit the file
```
sudo nano /etc/udev/rules.d/10-local.rules
```
* add 
```
ACTION=="add", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="7523", SYMLINK+="/dev/ttyUSB1"
```
udevtest /dev/ttyUSB1
references
* [reactivated.net - guide](http://www.reactivated.net/writing_udev_rules.html)
* [stackexchange - post](https://unix.stackexchange.com/questions/66901/how-to-bind-usb-device-under-a-static-name)

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
* If you need support, want to ask a question or suggest a different answer, you can join the discussion on the github forum
{{<icon_button text="Home Smart Mesh - Discussions" href="https://github.com/HomeSmartMesh/raspi/discussions" icon="github" >}}

{{<faq>}}
how does the Raspi-IoT framework compare to existing frameworks like home assistant or OpenHAB
<--->
Raspi-IoT is intended for developpers that prefer to stick to standard scripting like Python or Node.js and would like to use a minimalistic framework where everything can be debugged with standard tools, and also for sutdents and startups who would like to understand the underlaying techniques of home automation and IoT in order to create their own projects or products.
{{</faq>}}


