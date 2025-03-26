# What do these automations (not) do?
## `adaptive-lighting-prime-color-temp.yaml`
The aim was not to reinvent adaptive lighting, but to address a minor annoyance of mine, when turning off lights in the morning and turning them back on again late in the evening and be greeted with a `color_temp` transition from rather cold to warm.

How? By periodically setting the `color_temp` as calculated by adaptive lighting to lights that are part of an adaptive lighting instance _and_ currently OFF.
This makes sure that when the light is turned on, it will already have the most recent `color_temp` that was calculated by adaptive lighting's instance.

The interval is set to 5 minutes, so if you turn a light off and on again within that period, the `color_temp` will not be updated.

This approach works for Zigbee enabled bulbs (through MQTT), that support the `execute_if_off` feature for `color_options` - be aware that some lights do not support this.

There is also some action for LiFX bulbs, but they really do not need this; they already start with the color temperature that is given at `light.turn_on`, without transition. (Same goes for WiZ connected lights.)

In order for the automation to list all figure out which lights are under adaptive lighting control, the adaptive lighting switch setting `include_config_in_attributes` needs to be enabled. For your convenience, the automation also tries to do this for you when the `default` choice is executed.

### `color_options`: `execute_if_off`
In order to set the `color_temp` when the lights are off, the `execute_if_off` feature in `color_options` needs to be enabled. The automation will not do this periodically, rather, you will have to manually trigger the automation. This will execute the `default` choice of the automation, which will enable the `execute_if_off` feature in `color_options` in all Zigbee lights that are controlled by any adaptive lighting instance (and that support setting `color_temp`). You can trigger this configuration also when lights become "available", by adding triggers to the automation. In the YAML there are three `triggers` that will execute the `default` choice:
1. home assistant startup

   This _might_ work, but I have not seen it work properly in my setup. Once home assistant has started, the MQTT integration is not always ready to accept any commands yet.
1. When lights are turned off

   This should work if you populate the `entity_id`s of the lights that you have configured in adaptive lighting instasnces.
1. When a light becomes 'online'

   This _should_ work when for example the MQTT integration is ready, all MQTT/Zigbee controlled lights leave the 'Unavailable' or 'Unknown' state, the `default` choice will run and configure all lights to make the automation work.

You can also decide to uncomment the part in the automation where the `{"color_options": {"execute_if_off": true}}` is `mqtt.publish`ed, so that it will ensure the light will accept setting the `color_temp` while being off. In normal circumstances, you would only need to publish `{"color_options": {"execute_if_off": true}}` once, after the light gets re-paired to zigbee2mqtt or when zigbee2mqtt restarts. It's up to your personal preference.

# How to add the automation to home assistant?
## Using the frontend
1. Browse to your home assistant frontend.
1. Click `Settings` on the left side bar.
1. Click `Automations & scenes`.
1. Click `+ CREATE AUTOMATION` button at the bottom right.
1. Click `Create new automation >`
1. Click the overflow menu (three dots) at the top right.
1. Click `Edit in YAML`.
1. Click in the edit window.
1. Remove all template boilerplate text.
1. Paste the contents of the automation.
1. **Remove** the first line:
   ```
   - id: adaptive-lighting-prime-color-temp
   ```
   (Ref [Setup timing out](https://github.com/djurny/adaptive-lighting/issues/3), https://github.com/djurny/adaptive-lighting/issues/3#issuecomment-2754917583.)
1. Click `SAVE`.
1. Click `RENAME` in the pop-up window.
1. Click `Settings` on the left side bar.
1. Click `Automations & scenes`.
1. Confirm that the automation is listed as `Prime adaptive lighting color temperature`.
## Using `!include_dir_merge_list`
1. Create a folder in your home assistant's filesystem root called `automations.d`
1. Copy the automation YAML file in the newly created folder.
1. Edit your home assistant's `configuration.yaml` by adding the following:
   ```
   automation managed:     !include_dir_merge_list automations.d/
   automation ui:          !include automations.yaml
   ```
1. Click `Deloper tools` on the left sidebar.
1. Click `YAML`.
1. Click `AUTOMATIONS` in the `YAML configuration reloading` list.
1. Click `Settings` on the left side bar.
1. Click `Automations & scenes`.
1. Confirm that the automation is listed as `Prime adaptive lighting color temperature`.
---
