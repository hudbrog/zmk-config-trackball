# Trackball Wiring (nice!nano v2 + ADNS9800)

This file documents the current wiring used by this shield configuration.

## ADNS9800 connections

| ADNS9800 pin | nice!nano pin | nRF pin |
|---|---|---|
| SCLK | D15 | P1.13 |
| MOSI | D16 | P0.10 |
| MISO | D14 | P1.11 |
| NCS/CS | D18/A0 | P1.15 |
| MOTION/IRQ | D19/A1 | P0.02 |
| VDD | 3V3 | 3V3 |
| GND | GND | GND |

Notes:
- SPI bus is `pro_micro_spi` (`spi1`) in this config.
- CS is set in `boards/shields/trackball/boards/nice_nano_v2.overlay`.
- IRQ is set in `boards/shields/trackball/trackball.overlay`.

## Key switch GPIO connections (6 keys)

These are direct GPIO inputs from `kscan-gpio-direct` in `trackball.dtsi`.

| Key index | Pro Micro pin | nRF pin |
|---|---|---|
| 0 | D2 | P0.17 |
| 1 | D3 | P0.20 |
| 2 | D4/A6 | P0.22 |
| 3 | D5 | P0.24 |
| 4 | D6/A7 | P1.00 |
| 5 | D7 | P0.11 |

## Default key functions

From `trackball.keymap` (base and scroll layers currently use the same 6 bindings):

| Key index | Binding |
|---|---|
| 0 | `&mkp LCLK` |
| 1 | `&mkp RCLK` |
| 2 | `&tog SCROLL` |
| 3 | `&mkp MCLK` |
| 4 | `&mkp MB4` |
| 5 | `&mkp MB5` |
