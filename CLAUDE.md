# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a ZMK (Zephyr Mechanical Keyboard) firmware module for the Keyboardio Preonic keyboard. It implements a custom board definition and keymap for the Preonic using the ZMK firmware stack built on the Zephyr RTOS.

## Architecture

### Key Components
- **Board Definition**: `boards/arm/keyboardio_preonic/` - Hardware abstraction layer for the Preonic
  - `keyboardio_preonic.dts` - Device tree defining GPIO matrix, I2C, and hardware peripherals
  - `keyboardio_preonic.keymap` - Default keymap with 3 layers (base, nav, num) 
  - Kconfig files - Board configuration and feature enablement
- **Build Configuration**: `build.yaml` - GitHub Actions matrix configuration for automated builds
- **West Manifest**: `config/west.yml` - Dependency management for ZMK firmware
- **Module Definition**: `zephyr/module.yml` - Zephyr module configuration

### Hardware Details
- **MCU**: Nordic nRF52840 (ARM Cortex-M4)
- **Matrix**: 6x12 GPIO matrix with col2row diode direction
- **Features**: 
  - MAX17048 fuel gauge for battery monitoring (I2C address 0x36)
  - USB and Bluetooth connectivity
  - Two matrix transform options: ortho (full grid) and MIT (2u spacebar)

## Common Commands

### Building Firmware
```bash
# Build for keyboardio_preonic board
west build -b keyboardio_preonic

# Clean build (recommended for major changes)
west build -p -b keyboardio_preonic

# Build in specific directory
west build -d build/preonic -b keyboardio_preonic
```

### Flashing
```bash
# Flash via UF2 bootloader (preferred method)
west flash

# Flash via nrfjprog (requires J-Link)
west flash --runner nrfjprog
```

### Development Workflow
```bash
# Update dependencies
west update

# Initialize west workspace (if not already done)
west init -l config

# Install dependencies
west update
```

## Key Files to Modify

### Keymap Changes
Edit `boards/arm/keyboardio_preonic/keyboardio_preonic.keymap`:
- Modify layer definitions in `keymap` node
- Adjust key bindings using ZMK key codes
- Switch between PREONIC_ORTHO and PREONIC_MIT layouts using preprocessor defines

### Hardware Configuration  
Edit `boards/arm/keyboardio_preonic/keyboardio_preonic.dts`:
- GPIO pin assignments in `col-gpios` and `row-gpios` arrays
- I2C peripheral configuration
- Matrix transform definitions for different layouts

### Build Matrix
Edit `build.yaml`:
- Add/remove board configurations for GitHub Actions
- Configure cmake-args for specific build variants

## ZMK-Specific Concepts

- **Matrix Transform**: Maps physical switch positions to logical key positions
- **Behaviors**: ZMK's input handling system (key presses, holds, taps)
- **Layers**: Multiple keymaps that can be activated/deactivated
- **Device Tree**: Hardware description format used by Zephyr/ZMK

## Testing and Validation

- Test builds locally with `west build` before committing
- Verify matrix configuration matches physical wiring
- Check keymap functionality across all defined layers
- Validate I2C peripherals (fuel gauge) are properly configured