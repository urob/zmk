---
title: Pointer Configuration
sidebar_label: Pointers
---

These are settings related to the pointer/mouse support in ZMK.

See [Configuration Overview](index.md) for instructions on how to change these settings.

## Kconfig

Definition file: [zmk/app/mouse/Kconfig](https://github.com/zmkfirmware/zmk/blob/main/app/mouse/Kconfig)

### General

| Config                              | Type | Description                                                                | Default |
| ----------------------------------- | ---- | -------------------------------------------------------------------------- | ------- |
| `CONFIG_ZMK_MOUSE`                  | bool | Enable the general pointer/mouse functionality                             | n       |
| `CONFIG_ZMK_MOUSE_SMOOTH_SCROLLING` | bool | Enable smooth scrolling HID functionality (via HID Resolution Multipliers) | n       |

### Zephyr Settings

The following settings are from Zephyr, and should be defaulted to sane values, but can be adjusted if you encounter problems

| Config                           | Type | Description                                                | Default                         |
| -------------------------------- | ---- | ---------------------------------------------------------- | ------------------------------- |
| `CONFIG_INPUT_THREAD_STACK_SIZE` | int  | Stack size for the dedicated input event processing thread | 512 (1024 on split peripherals) |

## Input Listener

TODO: Complete description

### Devicetree

Applies to: `compatible = "zmk,input-listener"`

Definition file: [zmk/app/dts/bindings/zmk,input-listener.yaml](https://github.com/zmkfirmware/zmk/blob/main/app/dts/bindings/zmk%2Cinput-listener.yaml)

| Property           | Type   | Description                                                         | Default |
| ------------------ | ------ | ------------------------------------------------------------------- | ------- |
| `device`           | handle | Input device handle                                                 |         |
| `input-processors` | array  | List of input processors (with parameters) to apply to input events |         |

#### Child Properties

Additional properties can be set on child nodes, which allows changing the settings when certain layers are enabled:

| Property           | Type  | Description                                                                                      | Default |
| ------------------ | ----- | ------------------------------------------------------------------------------------------------ | ------- |
| `layers`           | array | List of layer indexes. This config will apply if any layer in the list is active.                |         |
| `input-processors` | array | List of input processors (with parameters) to apply to input events                              |         |
| `inherit`          | flag  | Whether to first apply the base input processors before the processors specific to this override |         |

## Input Split

TODO: Complete description

### Devicetree

Applies to: `compatible = "zmk,input-split"`

Definition file: [zmk/app/dts/bindings/zmk,input-split.yaml](https://github.com/zmkfirmware/zmk/blob/main/app/dts/bindings/zmk%2Cinput-split.yaml)

| Property           | Type   | Description                                                         | Default |
| ------------------ | ------ | ------------------------------------------------------------------- | ------- |
| `device`           | handle | Input device handle                                                 |         |
| `input-processors` | array  | List of input processors (with parameters) to apply to input events |         |
