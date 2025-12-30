# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a ZMK firmware configuration repository for the Ergomech Sofle Hybrid keyboard - a 6×4+5 keys column-staggered split keyboard using low profile Kailh Choc switches with Nice!Nano microcontrollers. The right side features a 5-way switch instead of a rotary encoder, and the left side has a rotary encoder with mouse scroll behavior.

**Important branch structure:**
- `Default` branch: Legacy support for older PCB versions (pre-May 2025)
- `sofle_hybrid_rgb_rev5`: Current main branch for Rev 5 PCBs with custom roller encoder (use this for PRs)
- `suho`: Current working branch

## Building Firmware

This repository uses GitHub Actions for building. **There is no local build process** - all firmware compilation happens via GitHub workflows.

### Trigger a build:
```bash
# Push to master or sofle_hybrid_rgb_rev5 branches triggers automatic build
git push origin sofle_hybrid_rgb_rev5

# Or manually trigger via GitHub Actions tab
gh workflow run build.yml
```

### Download firmware:
After a successful build, download artifacts from the GitHub Actions tab. The build produces:
- `sofle_left-nice_nano_v2-zmk.uf2` - Left side firmware
- `sofle_right-nice_nano_v2-zmk.uf2` - Right side firmware
- `settings_reset-nice_nano_v2-zmk.uf2` - Settings reset utility

## Build Configuration

The build is configured in `build.yaml`:
- Left side includes ZMK Studio support (`CONFIG_ZMK_STUDIO=y`) with USB-UART snippet
- Right side is standard build without Studio
- Uses ZMK v0.3 (specified in `config/west.yml`)

## Keymap Structure

### File locations:
- **Primary keymap**: `config/sofle_ergomech.keymap` - Main keymap file with 4 layers (BASE, LOWER, RAISE, ADJUST)
- **Keymap drawer config**: `sofle_ergomech.yaml` - Visual keymap representation config
- **Generated SVG**: `keymap-drawer/sofle_ergomech.svg` - Auto-generated keymap visualization

### Layer architecture:
1. **BASE (0)**: Default QWERTY layer with mouse movement on right 5-way switch
2. **LOWER (1)**: Function keys, numbers, and symbols. Contains `studio_unlock` button (top left)
3. **RAISE (2)**: Navigation, Bluetooth controls, and editing shortcuts
4. **ADJUST (3)**: RGB underglow controls and extended Bluetooth settings - activated automatically when both LOWER and RAISE are held (conditional layer)

### 5-way switch mapping:
The 5-way switch keys are mapped on the **last line** of each layer's bindings (after the main key grid). Order: Push button, Right, Up, Left, Down.

Example from keymap:
```
&kp ENTER  &mmv MOVE_RIGHT  &mmv MOVE_UP  &mmv MOVE_LEFT  &mmv MOVE_DOWN
```

**Critical quirk**: To support the visual keymap editor, the push button is placed at the bottom left in the textual keymap file, but it represents the center button of the 5-way switch.

### Encoder configuration:
Left encoder uses custom `mousescroll` behavior (defined in keymap) with `tap-ms = <20>` for smooth scrolling.

## Hardware Configuration

### Device tree files:
- `boards/shields/sofle_ergomech/sofle_ergomech.dtsi` - Shared base configuration (matrix transform, OLED setup, encoder definition)
- `boards/shields/sofle_ergomech/sofle_ergomech_left.overlay` - Left side GPIO mapping (6 columns, 5 rows)
- `boards/shields/sofle_ergomech/sofle_ergomech_right.overlay` - Right side GPIO mapping (6 columns, 6 rows for 5-way switch)
- `boards/shields/sofle_ergomech/sofle_ergomech-layouts.dtsi` - Physical layout definitions

### Features enabled (config/sofle_ergomech.conf):
- OLED display support
- Rotary encoder (EC11) support
- RGB underglow (with external power toggle disabled to keep display on)

## ZMK Studio

The keyboard supports ZMK Studio for live keymap remapping without reflashing:
- Only works over USB (not Bluetooth)
- Access via https://zmk.studio/ (Chrome or Edge)
- Unlock button located on top left of LOWER layer (`studio_unlock` binding)
- Only the left side has Studio enabled

## Keymap Drawer Workflow

The keymap visualization auto-updates when keymap files change:

```bash
# Automatically triggered on push to:
# - config/*.keymap
# - config/*.dtsi
# - keymap_drawer.config.yaml

# Manual trigger:
gh workflow run keymap_drawer.yaml
```

Output is committed to `keymap-drawer/` directory.

## Making Keymap Changes

### Option 1: ZMK Studio (live editing)
1. Flash firmware with Studio enabled (left side only)
2. Connect via USB to https://zmk.studio/
3. Press `studio_unlock` on LOWER layer
4. Edit keymap live (changes are temporary until flashed)

### Option 2: Manual editing
1. Edit `config/sofle_ergomech.keymap`
2. Maintain consistent indentation for readability
3. Remember: 5-way switch mappings are on the last line of each layer
4. Push changes to trigger build workflow

### Option 3: Visual keymap editor
Use https://nickcoutsos.github.io/keymap-editor (repository is configured for this editor)

## Key Behaviors and Bindings

- Mouse movement: `&mmv MOVE_*` (on BASE layer 5-way switch)
- Mouse scroll: `&msc SCRL_*` (custom `mousescroll` behavior on left encoder)
- Layer momentary: `&mo LAYER` (hold to activate layer)
- Conditional layers: ADJUST layer activates automatically when both LOWER and RAISE are active
- Bluetooth: `&bt BT_CLR`, `&bt BT_SEL N` (on RAISE/ADJUST layers)
- RGB: `&rgb_ug RGB_*` (on ADJUST layer)
- Studio unlock: `&studio_unlock` (top left of LOWER layer)

## Important Implementation Notes

1. **Matrix transform**: The keymap uses a 4-row, 16-column matrix with special handling for the 5th and 6th rows for thumb clusters and 5-way switch
2. **Encoder sensor bindings**: Defined per-layer in the keymap after the main bindings section
3. **RGB underglow**: Configured to NOT toggle external power to prevent OLED from turning off
4. **ZMK version**: Pinned to v0.3 in `config/west.yml` - update this if upgrading ZMK
