# ZMK configuration for BastardKB Charybdis (4x6)

## Overview

This repository contains my ZMK firmware configuration for the BastardKB Charybdis MK2 (4x6) split
ergonomic keyboard with trackball. It is set up to build firmware for nice!nano v2 controllers for
both halves and includes a settings reset image. The keymap is defined in config/charybdis.keymap,
and a layout descriptor for the Keymap Editor is provided at config/charybdis.json.

## Who is this for?

- Anyone who wants to build and flash this exact Charybdis configuration.
- Anyone who wants a starting point to customize a Charybdis keymap using the Keymap Editor.

## Quick links

- Keymap Editor (configure it to point to this repo): https://nickcoutsos.github.io/keymap-editor/
- This config repository (GitHub): https://github.com/josenaldo/zmk-for-charybdis
- ZMK docs: https://zmk.dev/docs
- BastardKB default keymaps: https://docs.bastardkb.com/fw/default-keymaps.html
- BastardKB flashing guide: https://docs.bastardkb.com/fw/flashing.html

## Repository layout

- build.yaml: Defines GitHub Actions build matrix producing UF2 files for:
    - nice_nano_v2 + charybdis_left
    - nice_nano_v2 + charybdis_right
    - nice_nano_v2 + settings_reset
- config/charybdis.keymap: The ZMK keymap used by both halves.
- config/charybdis.json: Layout coordinates for the Keymap Editor.
- config/boards/shields/charybdis: Charybdis shield definitions and settings.
- config/west.yml: ZMK module pinning (used by GitHub Actions build).

## Prerequisites

- A BastardKB Charybdis 4x6 with nice!nano v2 controllers in each half.
- A USB data cable.
- A GitHub account if you want to build from Actions (recommended).
- Optional: A Linux/macOS/Windows machine to flash via USB.

## How to build the firmware

### Option A — Build on GitHub (recommended)

1) Fork this repository on GitHub: https://github.com/josenaldo/zmk-for-charybdis
2) Enable GitHub Actions for your fork if prompted.
3) Push any changes (for example, after editing the keymap). The Actions workflow configured by ZMK
   will build artifacts automatically.
4) After the build completes, open the "Actions" tab in your repo, select the latest workflow run,
   and download the artifacts. You should find UF2 files named similar to:
    - nice_nano_v2-charybdis_left.uf2
    - nice_nano_v2-charybdis_right.uf2
    - nice_nano_v2-settings_reset.uf2

### Option B — Build locally (advanced)

Follow the official ZMK docs to set up a local west workspace and build for the targets
above: https://zmk.dev/docs/development/setup

Editing the keymap with the Keymap Editor
The Keymap Editor can read and write keymaps directly from a GitHub repository.

1) Open: https://nickcoutsos.github.io/keymap-editor/
2) In the Repo settings, point it to your fork of this repository.
3) Select the charybdis layout and open config/charybdis.keymap.
4) Edit your layers and bindings. Commit the changes back to your repo from the editor.
5) Trigger a new build (push/commit), then fetch the updated UF2 artifacts from GitHub Actions.

### Flashing the firmware to the keyboard

Charybdis halves use the nice!nano bootloader and accept UF2 files. Flash each half separately.

#### Put a half into bootloader mode

- On the underside of each half there is a small recessed reset button.
- Quickly double-press the button (two presses within ~500 ms). The half will enter bootloader mode.
- When connected over USB, your computer will mount a drive named NICENANO (or similar).

#### Flash the UF2

- Copy the correct UF2 file to the NICENANO drive for the side currently connected:
    - Left half: nice_nano_v2-charybdis_left.uf2
    - Right half: nice_nano_v2-charybdis_right.uf2
- The drive will disappear and the controller will reboot automatically after the copy.

#### Resetting settings (optional)

- If you need to wipe stored settings (e.g., Bluetooth bonds), flash the
  nice_nano_v2-settings_reset.uf2 to a half, then re-flash the normal firmware UF2 for that side.

#### Note for Ubuntu/Linux users

- Because the copy-and-reboot happens very quickly, some file managers may show a transient copy
  error. This is expected—the device rebooted too fast for the OS to keep up. You can safely ignore
  this if the device immediately disconnects and reboots.

#### After flashing both halves

- Disconnect and reconnect the keyboard normally.
- If using Bluetooth, use the function keys defined in the keymap to pair/select profiles:
    - Layer 1 includes BT_SEL 0..4, BT_NXT, BT_PRV, and BT_CLR. See ZMK Bluetooth
      docs: https://zmk.dev/docs/behaviors/bluetooth

## Troubleshooting

- If a half does not show up as NICENANO when entering bootloader, try another USB cable/port or
  repeat the double-press.
- If one side does not connect wirelessly after flashing, reset settings on both halves and re-pair.
- Consult BastardKB flashing docs: https://docs.bastardkb.com/fw/flashing.html and ZMK
  docs: https://zmk.dev/docs

## Credits

- Original configuration: https://github.com/Vzhao-L/zmk-for-charybdis
- BastardKB Charybdis: https://github.com/Bastardkb/Charybdis/
- ZMK Firmware: https://zmk.dev
