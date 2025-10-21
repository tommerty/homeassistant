# Smart bulb & smart switch automation

This is a simple automation that requires Hue or other smart bulbs, sensors for presence & luminance, and zibee switches.

Simply put, it helps automate your lights and ties together hue bulbs with zigbee switches for the ultimate convience giving you the best of both worlds.


## Features

- **Automatic On/Off:** Lights turn on automatically when occupancy is detected and it's dark enough, and turn off (or dim) when the room is unoccupied.
- **Ambient Light Awareness:** Uses an illuminance sensor to only turn on lights when needed.
- **Manual Override:** Physical Zigbee switches can override automation, keeping lights in sync with switch state.
- **Multi-Light & Multi-Switch Support:** Easily control multiple lights and switches together.
- **Smart Dimming:** After a configurable time (e.g., nighttime), unoccupied rooms dim instead of switching off completely.
- **Partial Lighting:** Specific lights can be kept on when the room is unoccupied (before dim time).

## Required
- Switches (Doesn't need to be Zigbee)
- Bulbs (Don't need to be Hue)
- Motion & luminance sensor (I'm using the Everything Presence Lite for this)

# Installation
## Step 1
- Head over to Settings > Devices & services > Helpers.
- Create a "Text" helper and call it whatever you like (though my yaml is expecting it to be called `kitchen_lights_status`)
- Set it to "Text" and give it a max lengh of something like 255.
- Create another helper but a datetime and call it whatever you want (my yaml is expecting `kitchen_last_manual_off`, but again you can change this later)

## Step 2
- Navigate over to Settings > Automations & scenes and click Create automation > Create new automation
- Click the three dots in the top left and click Edit in YAML
- Navigate over to the [automation.yaml](/automation/smart%20lighting/automation.yaml) and copy the contents and paste it back into the YAML editor on HA

You can rename it and do whatever you'd like here. You can go through the YAML and update the entity_id's that you require. For some context, these are what my entities are in case there's any confusion:

| entity_id | Description |
|---|---|
|`binary_sensor.everything_presence_lite_e01504_occupancy`| Occupancy sensor |
|`light.kitchen_light_switch_light`| Smart light switch 1|
|`light.kitchen_light_switch_light_2`| Smart light switch 2|
|`sensor.everything_presence_lite_e01504_illuminance`| illuminance |
|`light.kitchen`| Kitchen lights (room via Hue) |

These aren't variabled at this time, so you may need to go through the code and ensure all is up to date and configured to your liking.

### Step 3 (optional)
I created a simple Mushroom template card to get the data so you can see why the lights may be on or off in a human readable manner rather than digging through traces, and is why we created the helpers earlier. This is how I have mine configured:

![example image](/automation//smart%20lighting/example.jpg)

```yaml
type: custom:mushroom-template-card
primary: Auto lighting
secondary: |-
  {{ states('input_text.kitchen_lights_status') }}
  {{ relative_time(states[entity].last_changed) }} ago
    
icon: |-
  {% if is_state('light.kitchen', 'on') %}
        mdi:lightbulb-on
      {% else %}
        mdi:lightbulb-off
      {% endif %}
features_position: bottom
entity: light.kitchen
area: kitchen
multiline_secondary: true
grid_options:
  columns: full
color: |-
  {% if is_state('light.kitchen', 'on') %}
        amber
      {% else %}
        
      {% endif %}
badge_color: |-
  {% if is_state('automation.kitchen_smart_lighting_latest', 'on') %}
        green
      {% else %}
        red
      {% endif %}
badge_icon: |-
  {% if is_state('automation.kitchen_smart_lighting_latest', 'on') %}
        mdi:check
      {% else %}
        mdi:robot-off
      {% endif %}
```
Again, these will need to be updated depending on how you've named them.

You don't need Mushroom for this, you can simply just use Markdown or text blocks or whatever you'd like. I just like Mushroom for cards like this that can be very conditional and expandable.

# Notes
- I really made this for myself, and that's why I didn't put too much care into doing variables and the likes, though I may convert this to a template at some point.
- I really don't recommend going this approach. Either use smart switches or smart bulbs, mixing the two has been a headache. In the future I'll likely just replace the bulbs and swap out the switch for one with rotarys instead. All of this is because I bought these switches and had them installed, so I don't want to waste money replacing them while "they work".


Please feel free to share your thoughts on this and if you have any improvement ideas, I'm sure myself and my partner would grately appreciate it!