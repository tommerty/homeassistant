# Smart Room Lighting with Manual Override

## Features

- **Automatic On/Off:** Lights turn on automatically when occupancy is detected and it's dark enough, and turn off (or dim) when the room is unoccupied.
- **Ambient Light Awareness:** Optionally uses an illuminance sensor to only turn on lights when needed.
- **Manual Override:** Physical Zigbee switches can override automation, keeping lights in sync with switch state.
- **Multi-Light & Multi-Switch Support:** Easily control multiple lights and switches together.
- **Smart Dimming:** After a configurable time (e.g., nighttime), unoccupied rooms dim instead of switching off completely.
- **Partial Lighting:** Specific lights can be kept on when the room is unoccupied (before dim time).

---

## Inputs

| Input                          | Description                                                                     | Default   |
|------------------------------- |---------------------------------------------------------------------------------|-----------|
| **Occupancy Sensor**           | The motion/occupancy sensor for the room                                        | *(none)*  |
| **Illuminance Sensor**         | (Optional) Light sensor to determine if lights are needed                       | *(blank)* |
| **Illuminance Threshold**      | Lux value below which lights will turn on                                       | 20        |
| **Main Lights**                | List of main smart lights to control                                            | *(none)*  |
| **Zigbee Switches**            | Physical Zigbee switches that control the lights                                | *(none)*  |
| **Lights to Keep On Unoccupied**| Lights that should stay on when the room is unoccupied (before dim time)        | *(none)*  |
| **Full Brightness**            | Brightness (%) when room is occupied or manually turned on                      | 100       |
| **Dim Brightness**             | Brightness (%) for night mode dimming                                           | 10        |
| **Dim After Time**             | Time after which, when unoccupied, lights will dim instead of turning off       | 20:00:00  |

---

## How It Works

1. **When Motion Detected & Room is Dark**
   - All selected lights and switches turn on at full brightness.

2. **When No Motion**
   - **Before "Dim After Time":** Most lights and switches turn off. Optionally, keep selected lights on.
   - **After "Dim After Time":** All lights and switches dim to the specified level. Optionally, keep selected lights on at dimmed level.

3. **Manual Override via Switches**
   - **Switch ON:** All lights and switches turn on at full brightness.
   - **Switch OFF:**  
     - **Before "Dim After Time":** All lights and switches turn off.  
     - **After "Dim After Time":** All lights and switches dim to the specified level.

4. **Synchronization**
   - Lights and switches remain in sync, so manual or automatic changes are always reflected across all devices.

---

## Example Use Cases

- Living rooms or bedrooms with both smart bulbs and wall switches.
- Offices where you want lights to dim at night instead of turning off completely.
- Hallways or bathrooms where certain lights should remain on for safety after hours.

---

## Installation

1. Copy `motion-light-controller.yaml` into your Home Assistant `automations/blueprints` directory.
2. In Home Assistant, go to **Settings > Automations & Scenes > Blueprints** and import the blueprint.
3. Create a new automation using the blueprint, filling in the required inputs.

---

## Tips

- If you don’t have an illuminance sensor, the automation will only use motion/occupancy.
- “Zigbee Switches” should be the entities for your physical wall switches (e.g., Zigbee, Z-Wave).
- “Lights to Keep On When Unoccupied” is useful for safety or accent lighting.

---

---

## Example automation usage

```yaml
alias: Kitchen Lighting Controls
description: Smart lighting control for kitchen with occupancy and manual override
use_blueprint:
  path: tommerty/motion-light-controller.yaml
  input:
    occupancy_sensor: binary_sensor.everything_presence_lite_e01504_occupancy
    illuminance_sensor: sensor.everything_presence_lite_e01504_illuminance
    illuminance_threshold: 20
    main_lights:
      - light.kitchen_door
      - light.kitchen_window
    zigbee_switches:
      - light.kitchen_light_switch_light
      - light.kitchen_light_switch_light_2
    lights_to_keep_on_unoccupied: []
    full_brightness: 100
    dim_brightness: 10
    dim_after_time: "20:00:00"
```
