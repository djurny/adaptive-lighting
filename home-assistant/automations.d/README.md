# How to add the automation to home assistant?
## Using the frontend
1. Browse to your home assistant frontend.
1. Click 'Settings' on the left side bar.
1. Click 'Automations & scenes'.
1. Click '+ CREATE AUTOMATION' button at the bottom right.
1. Click 'Create new automation >'
1. Click the overflow menu (three dots) at the top right.
1. Click 'Edit in YAML'.
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
1. Click 'SAVE'.
1. Click 'RENAME' in the pop-up window.
1. Click 'Settings' on the left side bar.
1. Click 'Automations & scenes'.
1. Confirm that the automation is listed as 'Prime adaptive lighting color temperature'.
## Using `!include_dir_merge_list`
