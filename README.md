# ESPHome configs and LVGL examples for cheap touchscreen devices

## Supported Devices
* Sunton `ESP32-2432S028R` 2.8" with resistive touch and USB micro-B.
* Sunton `ESP32-8048S043` 4.3" with capactivive touch and USB-C.
* Sunton `ESP32-8048S050` 5.0" with capactivive touch and USB-C.
* Elecrow `DIS05035H` CrowPanel 3.5" (v2.2) with resistive touch and USB-C.

## File Structure
If all you are looking for is a device-specific config then look no further than the `devices/` directory. The YAML files in there are clean and free from anything not related to the devices themselves. They are intended to be used as [Packages](https://esphome.io/components/packages.html) in a higher-level YAML, which allows for device-specific YAML and common YAML to be kept in separate files, avoiding duplicate code and making it easier to update groups of devices. 

The example YAML files in the root of this repo demonstrate how to use each device's YAML with some common YAML, including a resolution-specific (but not device-specific) LVGL dashboard. 

## Advanced YAML Techniques
Aside from the ESPHome Packages feature used to separate device-specfic YAML from common YAML config, there are some other potentially unfamiliar techniques in use here. For example, the files within `dashboards/` use [YAML anchors and aliases](https://ref.coddy.tech/yaml/yaml-anchors) which help reduce code duplication. I use anchors and aliases instead of `styles` and `style_definitions` as they support reuse of anything I want instead of being restricted to just styles, and because they override the default `theme` when used. There is a bug or perhaps an odd design choice that prevent `styles`/`style_definitions` from overriding `theme`. I define my anchors within a made-up key called `.styles` as anything prefixed with a dot will not cause errors when parsed by ESPHome.

The `entities/` directory is another example of a technique I am using to reduce code duplication. Rather than fully define every Home Assistant entitiy that I would like to get the state of, I create Package templates for each type (`light`, `switch`, etc). These templates make some assumptions as to what LVGL widgets exist and attempt to update their icons or colors based on entity state. This greatly reduces duplcate code that would normally exist in a dashboard. Unfortunately, use of Packages cannot be nested, so these templates get pulled in at the top-level YAML config, rather than within the dashboard configs. 
