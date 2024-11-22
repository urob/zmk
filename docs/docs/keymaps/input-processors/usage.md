---
title: Input Processor Usage
sidebar_label: Usage
---

Input processors are used by assigning them to a given [input listener](../../features/pointers.md#input-listeners). A base set of processors is assigned to a listener, and then overrides can be set that are only active when certain [layers](../index.mdx#layers) are active. The following assumes you are adding processors to the `&trackpad` device which is set up with a `&trackpad_listener`:

### Base Processors

Base processors are assigned in the `input-processors` property, and when events are generated, the events are process in the sequence in the order the processors are listed. For example, if you wanted your trackpad to always scale the values to increase the movements, you would assign the [scaler](scaler.md#pre-defined-instances) to the property:

```dts
#include <input/processors.dtsi>

&trackpad_listener {
    input-processors = <&zip_xy_scaler 3 2>;
}
```

### Layer Specific Overrides

Additional overrides can be added that only apply when the associated layer is active. For example, to make the trackpad work as a scroll device when your layer `1` is active, nest a child node on the listener and set the `layers` and `input-processors` properties:

```dts
#include <input/processors.dtsi>

&trackpad_listener {
    input-processors = <&zip_xy_scaler 3 2>;

    scroller {
        layers = <1>;
        input-processors = <&zip_xy_to_scoll_mapper>;
    };
}
```

:::note

Overrides are processed in the order they are declared, from top to bottom, _not_ in any way tied to the layers specified in the `layers` property.

:::

If you want to have later overrides, or your base processors applied _after_ your overrides, add the `inherit` property to your child node, e.g.:

```dts
#include <input/processors.dtsi>

&trackpad_listener {
    input-processors = <&zip_xy_scaler 3 2>;

    scroller {
        layers = <1>;
        input-processors = <&zip_xy_to_scoll_mapper>;
        inherit;
    };
}
```
