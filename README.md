# ⚡ STUBA EV Charger – STM32F105

**Riadenie nabíjačky s OLED displejom, päťmi prevádzkovými režimami a meraním cez DMA na STM32F105.**

![Rok](https://img.shields.io/badge/rok-2018-blue) ![MCU](https://img.shields.io/badge/STM32-F105RB-03234b) ![Jazyk](https://img.shields.io/badge/C-HAL%20%2B%20CubeMX-lightgrey) ![Tím](https://img.shields.io/badge/STUBA-Green%20Team-green)

> **Rok realizácie:** 2018 · **Tagy:** `stm32` `stm32f1` `cortex-m3` `ev-charger` `power-electronics` `ssd1306` `oled` `adc` `dma` `can-bus` `embedded` `stuba` `green-team`

## O projekte

Druhá generácia riadiacej jednotky nabíjačky, vznikla počas práce v **STUBA Green Team** na FEI STU. Oproti [verzii na STM32F429](https://github.com/VRDragon/STUBA-EV-Charger-V1.0) beží na lacnejšom čipe **STM32F105RB** a pridáva používateľské rozhranie: OLED displej (SSD1306) a znakový LCD cez I²C.

## Čo firmvér robí

- Meria napätie a prúd (ADC s DMA) a číta dva analógové ovládacie vstupy.
- Generuje referenčné signály pre napätie a prúd (DAC).
- Zobrazuje prevádzkové informácie na OLED displeji.
- Sleduje vstupy prehriatia a blokovania, signalizuje stav cez LED.
- Obsahuje inicializáciu CAN na komunikáciu s ostatnými časťami systému.

### Prevádzkové režimy

| Režim | Názov | Účel |
|---|---|---|
| 0 | Štart | inicializácia a uzamknutie systému |
| 1 | Manuálny | ručné nastavenie napätia a prúdu |
| 2 | Automatické napätie | regulácia napätia |
| 3 | Automatický prúd | regulácia prúdu cez PWM |
| 4 | Rýchle nabíjanie | rýchle nabíjanie |

Plne implementovaný je zatiaľ manuálny režim, ostatné majú pripravenú kostru funkcií.

## Zostavenie

Projekt pre STM32CubeMX a Atollic TrueSTUDIO (`Charger_f105.ioc`). Importujte ho do IDE, skompilujte ARM GCC a naprogramujte cez ST-LINK. Podrobnosti sú v [docs/README.en.md](docs/README.en.md).

## Poznámka

Adresár `Debug/` a súbory `probe_proxy*.log` sú vedľajšie produkty prekladu a ladenia.

## Autor

**Vratislav Režo** ([@VRDragon](https://github.com/VRDragon))
