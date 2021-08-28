---
title: "micro Python"
description: "links and instructions for uPython"
date: 2021-04-03T08:00:00+00:00
lastmod: 2021-04-03T08:00:00+00:00
weight: 6
toc: true
---

{{<icon_button text="nRF52840 usb dongle" relref="/docs/microcontrollers/nrf52/usb_dongle" >}}

# cross compiler
* note `CROSS_COMPILE` is referencing the system toolchain with loose recommendations `7.3.1/2Q18 or 8.2.1/4Q18.`
* check the toolchain version
```bash
arm-none-eabi-gcc --version
which arm-none-eabi-gcc --version
```
* testing with version `9-2020-q2-update - 9.3.1 20200408`

# build instructions
```bash
git clone --recurse-submodules https://github.com/micropython/micropython.git
cd micropython/mpy-cross
make
cd ../ports/nrf
make submodules
make BOARD=pca10059
```

# config
## usb
* each board has the pins configured in a hreader file
```c++
ports/nrf/boards/pca10059/mpconfigboard.h
```
* USB CDC activation
* UART pin assignement
```c++
#define MICROPY_HW_USB_CDC          (1)
...
// UART config
#define MICROPY_HW_UART1_RX         (13)
#define MICROPY_HW_UART1_TX         (15)
#define MICROPY_HW_UART1_CTS        (17)
#define MICROPY_HW_UART1_RTS        (20)
#define MICROPY_HW_UART1_HWFC       (1)
...
```
* the UART is used a stdin only if neither of USB nor BLE are enabled such as in `ports/nrf/mphalport.c`
```c++
#if !MICROPY_PY_BLE_NUS && !MICROPY_HW_USB_CDC
int mp_hal_stdin_rx_chr(void) {
    for (;;) {
        if (MP_STATE_PORT(board_stdio_uart) != NULL && uart_rx_any(MP_STATE_PORT(board_stdio_uart))) {
            return uart_rx_char(MP_STATE_PORT(board_stdio_uart));
...
```
## uart

* use board `PCA10056` note that this board has `MICROPY_HW_USB_CDC` not defined, disabled, set to `(0)`
* edit the config file `ports/nrf/boards/pca10056/mpconfigboard.h`

{{<hint warning>}}
In case the Hardware flow control is not used, which means the UART CTS and RTS pins are not attached, it is important to disable the `MICROPY_HW_UART1_HWFC` by setting it to `(0)`
{{</hint>}}

```c++
// UART config
#define MICROPY_HW_UART1_RX         (9)
#define MICROPY_HW_UART1_TX         (10)
#define MICROPY_HW_UART1_CTS        (7)
#define MICROPY_HW_UART1_RTS        (5)
#define MICROPY_HW_UART1_HWFC       (0)
```
* build
```bash
make BOARD=pca10056
```

# Thread Sensortag
## config
```c++
#define MICROPY_HW_LED1             (4) // LED1
#define MICROPY_HW_LED2             (6) // LED2
#define MICROPY_HW_LED3             (8) // LED3
#define MICROPY_HW_LED4             (1) // LED4

// UART config
#define MICROPY_HW_UART1_RX         (2)
#define MICROPY_HW_UART1_TX         (29)
#define MICROPY_HW_UART1_CTS        (1)
#define MICROPY_HW_UART1_RTS        (1)
#define MICROPY_HW_UART1_HWFC       (0)

// I2C busses
#define MICROPY_HW_I2C1_NAME        "I2C1"
#define MICROPY_HW_I2C1_SCL         (45)  // SCL
#define MICROPY_HW_I2C1_SDA         (47)  // SDA

```
## python code
```python
from machine import Pin, I2C

i2c = I2C(0, scl=Pin(45), sda=Pin(47))
i2c.scan()
```
## output
```
[64, 72, 118]
```
## references
https://github.com/adafruit/Adafruit_CircuitPython_MS8607
https://github.com/eewiki/Xbee3-MicroPython/blob/master/samples/Zigbee_MS8607_i2c_rev1.py
