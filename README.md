# Hue Control
Home Assistant scripts for fine control over Philips Hue bulbs

## Pre-requisites

- Home Assistant
- [HACS](https://hacs.xyz/)
- [zha_toolkit](https://github.com/mdeweerd/zha-toolkit)

## Note for HA `2024.9.x` and later
There's a bug in zha_toolkit that prevents it from working with HA `2024.9.x` and later, that has not yet been fixed. Details can be found [here](https://github.com/mdeweerd/zha-toolkit/issues/261).

Until it is fixed, you can apply the `zha_toolkit_2024-09.patch` file, from the `~/config` directory via `git apply zha_toolkit_2024-09.patch`

## Overview

### READ
**ALL** five of these scripts must be implemented for this to work properly. You absolutely can use them individually, but the `set_light_color.yaml` script calls out to other scripts to keep things tidy, since there's some color-space conversion involved in the top-level script.

### [`set_light_color.yaml`](./scripts/set_light_color.yaml)
This is the primary script, and the one intended to be used Sets the light color based on one of the following inputs:
- Hue & Saturation
- RGB
- XY Color
- Color Temperature

Also accepts a brightness value. Providing a `transition_time` will cause the light to transition over that time. Providing more than one `light_entity` will cause the script to run for each entity.

Example usage:
```yaml
action: script.set_light_color
data:
  light_entity: light.kitchen_light
  hue: 120
  saturation: 100
  brightness: 255
  transition_time: 5
```

```yaml
action: script.set_light_color
data:
  light_entity: light.kitchen_light
  red: 255
  green: 0
  blue: 0
  brightness: 255
```

```yaml
action: script.set_light_color
data:
  light_entity:
    - light.kitchen_light
    - light.living_room_light
  color_temp: 2500
  brightness: 255
```

```yaml
action: script.set_light_color
data:
  light_entity: light.kitchen_light
  x: 0.5
  y: 0.5
  brightness: 255
```

### [`set_hue_light_color_and_brightness.yaml`](./scripts/set_hue_light_color_and_brightness.yaml)
This script accepts a hue, saturation, and brightness value. Spawned via the main script.

### [`set_hue_light_xy_and_brightness.yaml`](./scripts/set_hue_light_xy_and_brightness.yaml)
This script does the same as the above, but via an XY color.

### [`set_hue_light_color_temperature.yaml`](./scripts/set_hue_light_color_temperature.yaml)
This script is similar to the above, but via a color temperature value.

### [`set_hue_light_brightness.yaml`](./scripts/set_hue_light_brightness.yaml)
This script is similar to the above, but only accepts a brightness value.

