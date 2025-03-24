# What do these automations (not) do?
- `adaptive-lighting-prime-color-temp.yaml`
The aim was not to reinvent adaptive lighting, but to address a minor annoyance of mine, when turning off lights in the morning and turning them back on again late in the evening and be greeted with a `color_temp` transition from rather cold to warm.

How? By periodically setting the `color_temp` as calculated by adaptive lighting to lights that are part of an adaptive lighting instance _and_ currently OFF.
This makes sure that when the light is turned on, it will already have the most recent `color_temp` that was calculated by adaptive lighting's instance.

The interval is set to 5 minutes, so if you turn a light off and on again within that period, the `color_temp` will not be updated.

This approach works for Zigbee enabled bulbs, that support the `execute_if_off` feature for `color_options` - be aware that some lights do not support this.

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
1. Replace the hyphen on the first line with a single whitespace:
   ```
   - id: adaptive-lighting-prime-color-temp
   ```
     after removal:
   ```
     id: adaptive-lighting-prime-color-temp
   ```
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
