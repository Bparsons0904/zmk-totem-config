# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a ZMK firmware configuration repository for the TOTEM split keyboard (38 keys, column-staggered). The firmware is built automatically via GitHub Actions and targets the SEEED XIAO BLE controller.

## Build System

Firmware builds are triggered automatically on push/pull request via GitHub Actions. The workflow in `.github/workflows/build.yml` uses ZMK's official `build-user-config.yml` workflow to compile:
- `totem_left-seeeduino_xiao_ble-zmk.uf2`
- `totem_right-seeeduino_xiao_ble-zmk.uf2`
- `settings_reset-seeeduino_xiao_ble-zmk.uf2`

**To get firmware**: Push changes to GitHub, then download the `firmware.zip` artifact from the Actions tab.

There is no local build system - all builds happen in CI.

## Key Files to Edit

### Primary Configuration
- **`config/totem.keymap`** - Main keymap file containing all layers and key bindings
- **`config/totem.conf`** - Keyboard settings (name, sleep timeout, Bluetooth power, features)

### Shield Definition (rarely need to edit)
- `config/boards/shields/totem/totem.dtsi` - Hardware device tree
- `config/boards/shields/totem/*.overlay` - Left/right half overlay files
- `config/boards/shields/totem/Kconfig.*` - Kernel config

### Build Configuration
- `build.yaml` - Defines which board/shield combinations to build
- `config/west.yml` - ZMK manifest (points to upstream ZMK)

## Keymap Structure

The keymap (`config/totem.keymap`) uses:
- **4 layers**: BASE (0), NAV (1), SYM (2), ADJ (3)
- **Home row mods**: GUI/Alt/Ctrl/Shift on home row with mod-tap (`&mt`)
- **Layer switching**: `&lt NAV SPACE` (hold Space for NAV layer), `&lt SYM RET` (hold Enter for SYM layer)
- **Combos**: ESC on Q+W (positions 0+1)
- **Macros**: `&gif` macro for typing "@gif"

## ZMK References

- Keycodes: https://zmk.dev/docs/codes/
- Behaviors (mod-tap, layer-tap, etc.): https://zmk.dev/docs/behaviors/
- Configuration options: https://zmk.dev/docs/config/

## Current Configuration Notes

From `config/totem.conf`:
- Keyboard name: "KBHTotem"
- Sleep enabled with 15-minute timeout (900000ms)
- Increased Bluetooth TX power (`CONFIG_BT_CTLR_TX_PWR_PLUS_8`)
- Pointing device support enabled
