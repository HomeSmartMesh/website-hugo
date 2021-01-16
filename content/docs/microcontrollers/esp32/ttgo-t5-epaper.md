---
title: "TTGO T5-EPaper"
description: "ESP32 with epaper display"
date: 2020-12-25T00:00:00+09:00
draft: false
weight: 4
---
![img](/images/ttgo_t5-epaper.png)

## Simple example
* [epaper-TTGO-T5_1_2-hello](https://github.com/ESP32Home/epaper-TTGO-T5_1_2-hello)

### dependencies
* [GxEPD](https://github.com/ZinggJM/GxEPD)

```ini
lib_ldf_mode = deep+
lib_deps =
    GxEPD@3.1.0
```

### Example
```c++
#include <SPIFFS.h>
#include <epaper.h>

EPaper epaper;

void setup()
{
    Serial.begin(115200);
    SPIFFS.begin();

    epaper.init();
    epaper.display.fillScreen(GxEPD_WHITE);
    epaper.drawBitmap("/esp_home.bmp", 0, 50, true);
    epaper.displayText("Hello simple", 20, EPaper::RIGHT_ALIGNMENT);
    epaper.display.update();

}

void loop()
{
}
```
## Display
* GxGDEW029Z10
* 2,9 Zoll
* White / Black / Red
* 296x128

## References
* LilyGo epaper ESP-IDF repo [LilyGO/ESP32_T5Epaper_2.9inch](https://github.com/LilyGO/ESP32_T5Epaper_2.9inch)
    * fork from [loboris/ESP32_ePaper_example](https://github.com/loboris/ESP32_ePaper_example) 
* LilyGo epaper platformio-arduino repo [Xinyuan-LilyGO/T5-Ink-Screen-Series](https://github.com/Xinyuan-LilyGO/T5-Ink-Screen-Series)
    * fork from [TTGO-EPaper-Series](https://github.com/lewisxhe/TTGO-EPaper-Series)
* Waveshare specification pdf [2.9inch_e-Paper_Datasheet.pdf](https://github.com/LilyGO/ESP32_T5Epaper_2.9inch/blob/master/Documents/2.9inch_e-Paper_Datasheet.pdf)
## Suppliers
* [Aliexpress ~ 20€](https://de.aliexpress.com/item/32854552241.html?spm=a2g0s.9042311.0.0.27424c4djfgWXd)
