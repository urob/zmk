---
title: Pointing Devices
sidebar_label: Pointers
---

ZMK's pointer support builds upon the Zephyr [input API](https://docs.zephyrproject.org/3.5.0/services/input/index.html) to offer pointer/mouse functionality with various hardware. A limited number of input drivers are available in the Zephyr 3.5 version currently used by ZMK, but additional drivers can be found in [external modules](../../features/modules.mdx) for a variety of hardware.

## Input Device

The starting point to any integration of a physical pointing device is the device node itself. The specifics of where this node goes will depend on the specific hardware. _Most_ pointing hardware uses either SPI or I2C for communication, and will be nested under a properly configured bus node, e.g. `&pro_micro_i2c` or for a complete onboard setup, `&i2c3`. See the documentation on [pin control](./pinctrl.mdx) if you need to configure the pins for an I2C or SPI bus.

For example, if setting up a device that uses SPI, you may have a node like:

```dts
&pro_micro_spi {
    status = "okay";
    cs-gpios = <&pro_micro 19 GPIO_ACTIVE_LOW>;

    glidepoint: glidepoint@0 {
        compatible = "cirque,pinnacle";
        reg = <0>;
        spi-max-frequency = <1000000>;
        status = "okay";
        dr-gpios = <&pro_micro 5 (GPIO_ACTIVE_HIGH)>;

        sensitivity = "4x";
        sleep;
        no-taps;
    };
};
```

The specifics of the properties required to set for a given driver will vary; always consult the devicetree bindings file for the specific driver to see what properties can be set.

## Listener

Every input device needs an associated listener added that listens for events from the device and processes them before sending the events to the host using a HID mouse report. See [input listener configuration](../../config/pointers.md#input-listener) for the full details. For example, to add a listener for the above device:

```dts
/ {
    glidepoint_listener {
        compatible = "zmk,input-listener";
        device = <&glidepoint>;
    };
};
```

## Input Processors

Some physical pointing devices may be generating input events that need some adjustment before being sent to hosts. For example a trackpad might be integrated into a keyboard rotated 90° and need the X/Y data adjusted appropriately. This can be accomplished with [input processors](../../keymaps/input-processors/index.md). You could enhande the above listener with:

```dts
#include <dt-bindings/zmk/input_transform.h>

/ {
    glidepoint_listener {
        compatible = "zmk,input-listener";
        device = <&glidepoint>;
        input-processors = <&zip_xy_transform (INPUT_TRANSFORM_XY_SWAP | INPUT_TRANSFORM_X_INVERT | INPUT_TRANSFORM_Y_INVERT)>;
    };
};
```

## Split

Pointing devices are supported on split peripherals, with some additional configuration using the [input split];(../../config/pointers.md#input-split). All split pointers are identified using a unique integer value, which is specified using the `reg` property and in the `@#` suffix for the node. If adding multiple peripheral pointers, be sure that each is given a unique identifier.

### Shared

Both peripheral and central make use of a `zmk,input-split` device, which functions differently depending on where is is used. To avoid duplicating work, this node can be defined in a common `.dtsi` file that is included into both central and peripheral `.overlay`/`.dts` files. Second, the input listener for the central side is added here, but disabled, so that keymaps (which are included for central and peripheral builds) can reference the listener to add input processors without issue.

:::note

Input splits need to be nested under a parent node that properly sets the `#address-cells` and `#size-cells` values appropriately. These settings are what allow us to use a single integer number for the `reg` value.

:::

```dts
/ {
    split_inputs {
        #address-cells = <1>;
        #size-cells = <0>;

        glidepoint_split: glidepoint_split@0 {
            compatible = "zmk,input-split";
            reg = <0>;
        };
    };

    glidepoint_listener: glidepoint_listener {
        compatible = "zmk,input-listener";
        status = "disabled";
        device = <&glidepoint_split>;
    };
};
```

### Peripheral

On the peripheral, first include the shared file, and then update the input split to reference the pointer device that should be proxied:

```dts
#include "common.dtsi"

&glidepoint_split {
    device = <&glidepoint>;
};
```

If needed, the split can also have basic input processors applied to fix up the input data before it is sent to the central:

```dts
&glidepoint_split {
    device = <&glidepoint>;

    input-processors = <&zip_xy_transform (INPUT_TRANSFORM_XY_SWAP | INPUT_TRANSFORM_X_INVERT | INPUT_TRANSFORM_Y_INVERT)>;
};
```

### Central

On the central, the input split acts as an input device, receiving events from the peripheral and raising them locally. First, include the shared file, and then enabled the [input listener](#listener) that is created, but disabled, in our shared file:

```dts
#include "common.dtsi"

&glidepoint_listener {
    status = "okay";
};
```
