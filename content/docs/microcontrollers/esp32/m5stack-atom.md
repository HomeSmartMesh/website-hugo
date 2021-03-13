---
title: "M5 Atom Lite"
description: "Smallest tiny ESP32 pico with button and RGB LED"
date: 2021-03-13T00:00:00+09:00
draft: false
images: ["/images/esp32/M5-Atom-Lite.webp"]
weight: 16
---
# Overview
{{<image src="/images/esp32/M5-Atom-Lite.webp" width="300" >}}

* Tiny 24 x 24 x 10 mm
* User Button + Reset
* RGB Led SK6812
* IR Led

## Pinout
{{<image src="/images/esp32/M5-Atom-pinout.webp" width="300" >}}

## Links
{{<icon_button text="Product Page" href="https://docs.m5stack.com/#/en/core/atom_lite" icon="new">}}

## schematics
{{< load-photoswipe >}}
{{<gfigure alt="Atom Schematics" src="/images/esp32/ATOM_LITE_SIMPLE_CIRCUT_20200514.webp">}}

# Application - Serial to MQTT
* The serial to MQTT App creates a `request` and `response` topics that will propagate through a second Serial interface

{{<image src="/images/esp32/atom-serial-mqtt.png" >}}

* Although the board is available on `platform-espressif32` it was not avialable on platformio release as of Today, therefore the board is stored locally.
{{<icon_button href="https://github.com/platformio/platform-espressif32/blob/develop/boards/m5stack-atom.json" text="board on platform-espressif32" icon="github">}}

Application repo

{{<icon_button href="https://github.com/ESP32Home/mqtt_serial" text="ESP32 Program Github Repo" icon="github">}}

## Application Details
{{<details "main.cpp" >}}
```c++
#include <Arduino.h>
#include <ArduinoJson.h>
#include <MQTT.h>
#include "WiFi.h"
#include "M5Atom.h"
...
const char* host = "10.0.0.42";
int port = 1883;
const char *mqtt_client_id = "light_dimmer_mqtt";

const char* rx_topic = "esp/dimmer/request";
const char* tx_topic = "esp/dimmer/response";
const char* main_topic = "esp/dimmer";

MQTTClient mqtt(1024);// 1KB
WiFiClient wifi;//needed to stay on global scope

//      G
//      5V
#define TX_PIN 25
#define RX_PIN 21
...
void mqtt_loop(){
  mqtt.loop();
  if (!mqtt.connected()) {
    connect();
  }
}

void process_serial_2_mqtt(String str_message){
  mqtt.publish(tx_topic, str_message);
  Serial.println("\nSerial > MQTT");
  Serial.println(str_message);
}
void messageReceived(String &topic, String &payload) {
  Serial1.println(payload);
  Serial.println("\nMQTT > Serial");
  Serial.println(payload);
}

void setup() {
  
  M5.begin(false, false, true);
  delay(50);
  M5.dis.drawpix(0, CRGB(0,50,0));

  Serial.begin(115200);
  Serial1.begin(115200, SERIAL_8N1, RX_PIN, TX_PIN);
  WiFi.begin(ssid, password);

  mqtt.begin(host,port, wifi);
  mqtt.onMessage(messageReceived);

}
...
void loop() {
  mqtt_loop();
  if (M5.Btn.wasPressed()){
    switch_led();
  }

  delay(50);
  M5.update();
  if(Serial1.available()>0){
    String msg = Serial1.readString();
    process_serial_2_mqtt(msg);
  }
}
```
{{</details>}}


{{<details "Build info...">}}
```bash
PACKAGES:
 - framework-arduinoespressif32 3.10005.210308 (1.0.5)
 - tool-esptoolpy 1.30000.201119 (3.0.0)
 - toolchain-xtensa32 2.50200.97 (5.2.0)
...
Dependency Graph
|-- <ArduinoJson> 6.16.1
|-- <MQTT> 2.4.7
|-- <M5Atom> 0.0.1
|   |-- <FastLED> 3.4.0
|   |-- <Wire> 1.0.1
|-- <FastLED> 3.4.0
|-- <WiFi> 1.0
```
{{</details>}}

{{< details "Platformio ini file ..." >}}
```ini
[env]
board = m5stack-atom
framework = arduino
lib_deps =
    ArduinoJson@6.16.1
    617@2.4.7   #MQTT 256dpi/arduino-mqtt
    M5Atom
    fastled/FastLED @ ^3.4.0
lib_ldf_mode = deep+
[env:m5stack-atom]
platform = espressif32
monitor_speed = 115200
```
{{< /details >}}
