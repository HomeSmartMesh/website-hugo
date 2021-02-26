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

