---
title: nRF52840 USB dongle
description: Nordic's official dev kit PCA10059 in a form of a usb dongle for the nRF52840
image: /images/nrf_usb_dongle.png
order: 5
date: 2021-02-14T08:00:00+00:00
lastmod: 2021-02-14T08:00:00+00:00
tags: [nRF52840, USB]
features:
    - board codename : PCA10056
    - nRF52840
    - ARM M4 64 MHz
    - ROM 1024 KB, RAM 256 KB
    - Bluetooth 5, Mesh
    - 802.15.4, Thread, Zigbee
    - USB 2
---

:button[]{label="Product Page" link="https://www.nordicsemi.com/Software-and-tools/Development-Kits/nRF52840-Dongle/" icon="new"}

:image[]{src="/images/nrf_usb_dongle.png" center width="400"}

* [nRF52840](https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF52840)
* ARM M4 64 MHz
* ROM 1024 KB, RAM 256 KB
* Bluetooth 5, Mesh
* 802.15.4, Thread, Zigbee
* USB 2

### Applications
* [custom mesh coordinator](https://github.com/nRFMesh/nRF52_Mesh#08-usb-dongle-nrf52840-dongle)

# nRF52840 Dongle

## Pinout

### Serial Wire Debug
* the usb dongle is referred to as `PCA10059` which in projects usually shifts the app to offset `0x1000` as openbootloader usage is expected
* With serial wire debug, it's possible to flash applications at same address as the `PCA10056` which is at address 0x0000

[Serial Wier Debug](/images/thread_sensortag/serial_wire_debug.png)

### LEDS

* LED1 Green : `P0.06`
* LED2 Green : `P0.08`
* LED2 Green : `P1.09`
* LED2 Blue  : `P0.12`


## pogo pin adapter
* Pogo Pin P75-E2 Dia 1.3mm Length 16.5mm

[Pgogo Adapter](https://raw.githubusercontent.com/HomeSmartMesh/models/main/nrf/pogo_adapter_swd.glb)

:button[]{label="STL models zip" link="https://raw.githubusercontent.com/HomeSmartMesh/models/main/nrf/nRF52840_usb_pogo_top.zip" icon="download"}

## micro python
:button[]{label="build micro python" link="/frameworks/upython/#config" }

## gallery
```yaml gallery
dir: ./nrf52_dongle
```

# bootloader
## options
* nRF SDK v16.0.0 [bootloader](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fcom.nordic.infocenter.sdk5.v15.0.0%2Flib_bootloader.html)
* Nording [nRF52840 Dongle tutorial](https://devzone.nordicsemi.com/guides/short-range-guides/b/getting-started/posts/nrf52840-dongle-programming-tutorial)
* MCU boot - zephyr bootloader
* UF2 bootloader

## recovery
* on nRF SDK recovering the openbootloader mode would then require to reflash the MBR with any project `make flash_mbr`

* without nRF SDK, download the bootloader from the [nordic tutorial](https://devzone.nordicsemi.com/guides/short-range-guides/b/getting-started/posts/nrf52840-dongle-programming-tutorial) and rename to `pca10059_bootloader.hex`

then run

```bash
J-Link>Power on
cd pca10059_bootloader
>nrfjprog -f nrf52 --eraseall
 >nrfjprog -f nrf52 --chiperase --program pca10059_bootloader.hex --verify --reset
```

## build for USB DFU
on any sample from nRF Connect that supports the nrf52840dongle_nrf52840 run

```bash
cd nrf\v2.3.0\nrf\samples\xxx\
>west build -p always -b nrf52840dongle_nrf52840 -- -DOVERLAY_CONFIG="overlay-usb.conf" -DDTC_OVERLAY_FILE="usb.overlay"
```

## USB DFU command line
It is possible to use the nRF Connect programmer, but to perform the same with command line, you need to generate a package the flash it as follows

```bash
>nrfutil pkg generate --hw-version 52 --sd-req=0x00 --application build/zephyr/zephyr.hex --application-version 1 build/zephyr/zephyr.zip
nrfutil dfu usb-serial -pkg build/zephyr/zephyr.zip -p COM8
```
