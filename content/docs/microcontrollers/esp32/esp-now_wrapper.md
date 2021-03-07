---
title: "ESP-Now Wrapper"
description: ""
date: 2021-02-26T00:00:00+09:00
images: []
weight: 20
---
# Relates to
{{<icon_button relref="/docs/microcontrollers/esp32/esp-mesh_wrapper/" text="ESP-Mesh Wrapper" >}}

# Github repo
{{<icon_button text="Github repo" href="https://github.com/ESP32Home/ESPNowWrapper" icon="github">}}

# Status
{{<hint warning>}}
Not satisfactory, as it is not reliable. The ESP-Now does not have a quality of service concept that ensures transmission like TCP, given the broadcast nature of the protocol. Also the handling of a wifi gateway does not allow a large enough listening time, so a wifi gateway is not possible and a serial one has to be developer.
{{</hint>}}

# Examples
## Broadcast
{{<details "Arduino Broadcast example">}}
```c++
#include <Arduino.h>
#include <ArduinoJson.h>
#include <WiFi.h>

#include <ConfigUtils.h>
#include <EspNowWrapper.h>
#include <ESPStringUtils.h>

DynamicJsonDocument config(5*1024);//5 KB
DynamicJsonDocument secret(1*1024);//1 KB
WiFiClient wifi;//needed to stay on global scope
NowApp espnow;

uint32_t cycle_count = 0;

String topic;

void meshMessage(String &payload,String from){
  Serial.printf("RX> from(%s) => [%s]\n",from.c_str(),payload.c_str());
}

void setup() {
  
  Serial.begin(115200);
  timelog("Boot ready");

  load_json(config,"/config.json");
  load_json(secret,"/secret.json");
  timelog("config loaded");

  String ssid = secret["wifi"]["access_point"];
  String pass = secret["wifi"]["password"];
  WiFi.mode(WIFI_MODE_STA);
  timelog("config loaded");


  byte mac[6];
  WiFi.macAddress(mac);
  topic = "espnow/"+hextab_to_string(mac);

  espnow.start(config,secret);
  espnow.onMessage(meshMessage);

  espnow.set_channel(config["wifi"]["channel"]);

  timelog("setup() done");

}

void loop() {
  cycle_count++;
  Serial.printf("\n\n");
  timelog("loop start cycle ("+String(cycle_count)+")");
  espnow.print_info();
  espnow.broadcast("Hello Everyone ("+String(cycle_count)+")");
  delay(5000);
}
```
{{</details>}}

## MQTT Gateway
{{<hint danger>}}
Not successfully tested yet.
{{</hint>}}


{{<icon_button text="espnow_mqtt_gateway.ino" href="https://github.com/ESP32Home/ESPNowWrapper/blob/master/examples/espnow_mqtt_gateway.ino" icon="github">}}

# Frequently Asked Questions
Comments, Questions and suggestions are welcome on the [Home Smar Mesh - Discord server](https://discord.gg/57cZapWHkC)

{{<details title="Is it possible for a device to identify its ESP-Now peers ?">}}
* Given the ESP-Now [API Reference](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/network/esp_now.html#api-reference), it looks like that it is a low level point to point messaging including broadcast, but not more. Therefore it's only based on mac addresses and does not have a higher level management. If it's needed to discover devices without entering their mac manually as a peer list, then it's probably better to look for [ESP-Mesh](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/mesh.html) which is intended to manage a fleet of devices. This [ESP-Mesh-Wrapper](/docs/microcontrollers/esp32/esp-mesh_wrapper/) is a wrapper for Arduino framework to make usage ESP-Mesh easier.
* but I'm not really much satisfied with the custom protocols ideas, the ESP is at its best with wifi, when it comes to 2.4 GHz mesh solutions, why use a custom one while Standards exist like zigbee and Thread.
{{</details>}}

{{<details title="Can ESP-Now solve the 'Provisioning' Problem or make it easier ?">}}
* The provisioning dilemma is unfortunately common to all protocols and is a Home Automation Nightmare : "Provisioning / Pairing / Joining / Configuring".

Let's first have a look on other protocols deal with provisioning :
* Zigbee : Is the worst I've ever seen, as the state is not visible to the user shared between a device and a server, if a device goes offline it still thinks it's paired, to overcome temporary network shortage, and if you want it to join a new network you need to actively be playing around with it to figure out how each device has a different way to be reset and then enable pairing on the gateway...

* Wifi : has a secred that each device has to know, but how can you give that secret, the own hotspot is not easy, as you need a webserver and manual entry, there are some protocols designed to make that easier :
 * DDP Device Provisioning Protocol
 * SmartConfig : this is [another example](https://github.com/geeksville/AutoWifi), you can read more about Unified [Provisioning on the Espressif website](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/provisioning/provisioning.html)

Now, we might be disgusted from wifi for two reasons, provisioning and also range which is very limited compared to mesh networks that have repeaters.

* Custom RF : e.g. the same here applies for ESP-NOW or any other custom mesh such as the [SimpleMesh](/docs/networks/nrf/) which is as it's names suggest simple, but also stable. Changing the battery is not more complex than changing the battery. The way I solved the configuration issue there, is by an automated script run after attaching a devices with the debugger, it reads the serial number, which has an associated short id coming from a config file. That short id will be used in radio packets as unique ID, I could also assign a master key for encryption, but I did not need security as it is only about light and temperature values, no control.

So when it comes to ESP-Now, you also need to configure each of them to give them the security with "esp_now_set_pmk()" if you do not want everyone using your devices to spy on each other.
The problem gets bigger if you want to give your devices to a user that has to use it securely on his network with a unique pmk and that user does not want to be flashing and messing with a configuration.

To summarize, there's no way around a "provisioning" process whatever protocol you use unless you need no security, nor networks isolation, or can flash the secret uniquely for each device, which is also a sort of provisioning. And not mentioning the provisioning that needs an internet connection to synchronise with a backend and user account.
{{</details>}}