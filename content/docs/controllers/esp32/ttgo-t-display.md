---
title: "TTGO T-Display"
description: "Small dev kit with O-LED display"
date: 2020-12-25T00:00:00+09:00
draft: false
weight: 1
---

![img](/images/ttgo_t-display.png)

## Simple Example
* [t-display_controller](https://github.com/ESP32Home/t-display_controller)

### dependencies
* [TFT_eSPI_ttgo_t-display](https://github.com/ESP32Home/TFT_eSPI_ttgo_t-display)
    * fork from [Bodmer/TFT_eSPI](https://github.com/Bodmer/TFT_eSPI)
* [GfxUi](https://github.com/ESP32Home/GfxUi)

```ini
lib_deps =    
    https://github.com/ESP32Home/TFT_eSPI_ttgo_t-display.git#2.3.4_t-display
    https://github.com/ESP32Home/GfxUi.git#v1.0.1
```
### Example
```c++
#include <Arduino.h>
#include <TFT_eSPI.h>
#include <SPI.h>
#include "GfxUi.h"
#include <SPIFFS.h>

TFT_eSPI tft = TFT_eSPI(135, 240);
GfxUi ui = GfxUi(&tft);

void setup()
{
    Serial.begin(115200);

    tft.init();
    tft.setRotation(3);
    SPIFFS.begin(true);//FORMAT_SPIFFS_IF_FAILED
    ui.drawBmp("/esp_home.bmp", 0, 0);//'ui' needs tft and SPIFF

    tft.setCursor(0, 0);
    tft.setTextColor(TFT_GREEN);
    tft.setTextSize(2);
    tft.drawString("Hello TFT World",  tft.width() / 2, tft.height() / 2 );

}

void loop()
{

}
```


## Display
* IPS ST7789V
* 1,14 Zoll
* 135x240

## Suppliers
* [Aliexpress ~ 10€](https://de.aliexpress.com/item/4000829894292.html?spm=a2g0s.9042311.0.0.33794c4dbkKB4T)
