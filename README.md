# ZMK firmware for Pro Micro nRF52840 + PMW3610

This repository is configured as a `zmk-config` project for a BLE trackball device
that uses only the `PMW3610` sensor (no keyboard matrix).

## Build target

- Board: `promicro_nrf52840/nrf52840/uf2`
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
west build -s zmk/app -b promicro_nrf52840/nrf52840/uf2 -- -DSHIELD=pmw3610_trackball -DZMK_CONFIG="%CD%\\..\\config"
```

## Notes

- BLE and USB are enabled in `config/pmw3610_trackball.conf`.
- BLE device name is `PMW3610 TB` (ZMK limit: max 16 chars).
- PMW3610 uses the upstream Zephyr input driver:
  `compatible = "pixart,pmw3610"` and `CONFIG_INPUT_PMW3610=y`.
- `CONFIG_ZMK_BLE_CLEAR_BONDS_ON_START=y` is enabled for debugging/pairing reset.
  Turn it off after first successful pairing to keep bonds across reboot.
- `force-awake;` is enabled in the PMW3610 node for bring-up and debugging.
- `supply-gpios = <&gpio0 13 GPIO_ACTIVE_HIGH>;` keeps external `3.3V VCC` enabled on
  SuperMini-style nRF52840 boards.
- `duplex = <SPI_HALF_DUPLEX>;` is enabled for PMW3610 3-wire SDIO mode.
