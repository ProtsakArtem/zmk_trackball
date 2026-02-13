# ZMK firmware for Pro Micro nRF52840 + PMW3610

This repository is configured as a `zmk-config` project for a BLE trackball device
that uses only the `PMW3610` sensor (no keyboard matrix).

## Build target

- Board: `promicro_nrf52840`
- Shield: `pmw3610_trackball`

## Wiring used in overlay

- `SCLK` -> `P0.24` (`D5`)
- `SDIO` -> `P1.00` (`D6`, single-wire SPI mode: MOSI+MISO on one pin)
- `NCS` -> `P0.22` (`D4`)
- `MOTION` -> `P0.20` (`D3`)
- `VCC` -> `3V3`
- `GND` -> `GND`

If your hardware is wired to other GPIOs, edit:
`config/boards/shields/pmw3610_trackball/pmw3610_trackball.overlay`.

## Build (local)

```powershell
west init -l config zmk-app
cd zmk-app
west update
west zephyr-export
west build -s zmk/app -b promicro_nrf52840 -- -DSHIELD=pmw3610_trackball -DZMK_CONFIG="%CD%\\..\\config"
```

## Notes

- BLE is enabled, USB is disabled in `config/pmw3610_trackball.conf`.
- PMW3610 external driver is included through `config/west.yml`.
- This setup uses the external module variant: `compatible = "pixart,pmw3610-alt"` and
  `CONFIG_PMW3610_ALT=y`.
