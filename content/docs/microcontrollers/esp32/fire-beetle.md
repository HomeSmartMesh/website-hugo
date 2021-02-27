---
title: "FireBeetle"
description: "Low power design with ESP-WROOM-32 module with 10 uA deep sleep"
date: 2021-02-27T00:00:00+09:00
draft: false
images: ["/images/esp32/firebeetle/3 Charging.jpg"]
weight: 15
---
# Overview
{{<image src="/images/esp32/firebeetle_board.png" width="300" >}}

* 10 uA deep sleep current
* USB and external DC battery power inputs (3.3V - 5V)
* battery charging from USB external DC
* TP4056 Li-Ion battery charger
* RT9080 Low Drop out Linear Regulator
* supported by platformio (`board=firebeetle32`)

## PINS
* LED : (IO2)
* VBAT : (A0 - 36 - SENSOR_VP) measure ratio = 0.5

{{<hint danger>}}
The 0 Ohm resistors R10 and R11 needed to sense the battery voltage by A0-Pin38 are not populated (as of the boards I have). Therefore solder bridges are required to be added manually 😢
{{</hint>}}
* Another caveat is the usage of pin 36 A0 which conflicts with wifi. To avoid that, in the program below, the battery is measured before the wifi is activated.
## Gallery

{{< load-photoswipe >}}
{{< gallery dir="/images/esp32/firebeetle/" />}}

## Links
{{<icon_button text="FireBeetle - DFRobot" href="https://www.dfrobot.com/product-1590.html" icon="new">}}

{{<icon_button text="Wiki" href="https://wiki.dfrobot.com/FireBeetle_ESP32_IOT_Microcontroller(V3.0)__Supports_Wi-Fi_&_Bluetooth__SKU__DFR0478" icon="new">}}

{{<icon_button text="Schematics" href="https://raw.githubusercontent.com/DFRobot/Wiki/master/DFR0478/%5BDFR0478%5DFireBeetle%20Board-ESP32(V4.0)%E7%94%9F%E4%BA%A7%E6%A3%80%E6%9F%A5%E5%9B%BE.PDF" icon="download">}}

{{<icon_button text="User Manual" href="https://github.com/Robert-MARKII/Document/raw/master/FireBeetle%20Board-ESP32%20User%20Manual%20update.pdf" icon="download">}}

## Related products


{{<icon_button text="Solar Power Manager Micro" href="https://www.dfrobot.com/product-1781.html" icon="new">}}

# Applications

{{<icon_button href="https://github.com/ESP32Home/firebeetle_solar_small.git" text="ESP32 Program Github Repo" icon="github">}}

## Config
```json
{
    "mqtt" : {
        "host":"10.0.0.42",
        "port":1883,
        "client_id":"esp_firebeetle_1"
    },
    "base_topic":"esp/firebeetle_1",
    "deep_sleep_sec":10
}
```
## Main
{{<details "main.cpp" >}}
```c++
#include "Arduino.h"
#include <WiFi.h>

#include <ArduinoJson.h>
#include "freertos/FreeRTOS.h"
#include <WiFi.h>
#include <MQTT.h>
#include <esp_wifi.h>

#include "json_file.h"
#include "battery.h"
#include "wifi_secret.h"

DynamicJsonDocument config(1*1024);//5 KB
MQTTClient mqtt(1*1024);// 1KB for small messages
WiFiClient wifi;//needed to stay on global scope

void setup() {

  Serial.begin(115200);
  uint32_t vref = bat_init();
  Serial.printf("eFuse Vref:%u mV\n", vref);
  float battery_f = bat_get_voltage();
  battery_f /=1000;

  pinMode(LED_BUILTIN, OUTPUT);
  blink(100);
  WiFi.begin(ssid, password);
  load_config(config,true);
  timelog("config loaded");

  if(connect()){
    mqtt_publish_status(battery_f);    timelog("=>status");
    mqtt.loop();
  }

  Serial.println("ESP going to deep sleep");
  Serial.flush();
  blink(100);

  esp_wifi_stop();
  uint32_t deep_sleep_sec = config["deep_sleep_sec"];
  esp_deep_sleep(deep_sleep_sec*1000000);
  esp_deep_sleep_start();

}

void loop() {
  //no loop
}

```
{{</details>}}


{{<details "Build info...">}}
```bash
PACKAGES:
 - framework-arduinoespressif32 3.10004.201016 (1.0.4)
 - tool-esptoolpy 1.30000.201119 (3.0.0)
 - toolchain-xtensa32 2.50200.80 (5.2.0)
 ...
 Dependency Graph
|-- <WiFi> 1.0
|-- <ArduinoJson> 6.16.1
|-- <MQTT> 2.4.7        
|-- <FS> 1.0
|-- <SPIFFS> 1.0        
|   |-- <FS> 1.0   
```
{{</details>}}

{{< details "Platformio ini file ..." >}}
```ini
[env]
board = firebeetle32
framework = arduino
lib_deps =
    WiFi
    ArduinoJson@6.16.1
    617@2.4.7   #MQTT 256dpi/arduino-mqtt
lib_ldf_mode = deep+
[env:firebeetle32]
platform = espressif32
monitor_speed = 115200
```
{{< /details >}}
