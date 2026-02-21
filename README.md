# ESPHome BLE Control for Dometic FreshJet 1700

Control a Dometic FreshJet 1700 air conditioner over BLE using ESPHome and expose it to Home Assistant via MQTT Climate.

This project provides a fully working ESPHome configuration that:

- Connects to a Dometic air conditioning unit over BLE
- Publishes MQTT Discovery for Home Assistant
- Exposes a proper Climate entity
- Supports fan modes, sleep preset, and light brightness
- Uses retained MQTT state

---

## Features

- ✅ Climate entity (cool / heat / fan_only / auto / dry / off)
- ✅ Fan modes (low / medium / high / turbo / auto)
- ✅ Sleep preset (only valid in Cool and Heat)
- ✅ Independent light control (Off / 50% / 100%)
- ✅ Retained MQTT state handling
- ✅ MQTT Discovery compatible
- ✅ No custom HA integration required

---

## Hardware Requirements

- ESP32 (tested on ESP32 DevKit)
- Dometic FreshJet 1700 (BLE version)
- Home Assistant with MQTT broker enabled (You will need the latest greatest HA and ESPHome > 2026.2.0)

---

## Installation

1. Install ESPHome.
2. Install the MQTT App and configure a user
3. Copy `dometicac.yaml` into your ESPHome config directory.
4. Create a `secrets.yaml` file.
5. Adjust your WiFi and MQTT credentials.
6. Determine the mac address of your Dometic unit. You can do this using nrf Connect on a phone though I believe iOS will not expose the mac address for some security reasons but Android should!
7. Flash the ESP32.
8. Restart Home Assistant.

The Climate entity will appear automatically via MQTT discovery.

---

## secrets.yaml Example

```yaml
wifi_ssid: "YourSSID"
wifi_password: "YourPassword"

mqtt_user: "mqtt"
mqtt_password: "mqttpassword"
```

---

## MQTT Topics

| Topic | Description |
|-------|------------|
| camper/dometic/status | Availability |
| camper/dometic/mode | Current HVAC mode |
| camper/dometic/fan | Current fan speed |
| camper/dometic/current_temp | Measured temperature |
| camper/dometic/target_temp | Target temperature |
| camper/dometic/preset | Preset (none / sleep) |
| camper/dometic/set_mode | Mode command |
| camper/dometic/set_fan | Fan command |
| camper/dometic/set_temp | Temperature command |
| camper/dometic/set_preset | Preset command |

All state topics are retained to ensure proper HA startup behaviour.

---

## BLE Registers Used

| Register | Function |
|----------|----------|
| 0x01 | Power |
| 0x02 | Fan mode |
| 0x03 | HVAC mode |
| 0x04 | Target temperature |
| 0x05 | Light power |
| 0x06 | Light brightness |
| 0x0A | Current temperature |
| 0x1B | Sleep mode |

---

## Sleep Preset Logic

- Sleep is only valid in **Cooling** and **Heating** modes.
- If selected in other modes, it automatically resets to `none`.

---

## Light Behaviour

The Dometic unit only supports:

- Off
- 50%
- 100%

Brightness values are automatically snapped to valid levels.

---

## Known Limitations

- Light brightness is restricted to 50% and 100% (device limitation)
- Tested primarily on FreshJet 1700 BLE variant that has the four operational modes. If you only have cooling you will need to edit as appropriate.
- Note I could only get a Climate entity exposed by using MQTT. I even tried creating an external component but the new Home Assistant and ESPHome would not allow the BLE nor the climate entity properly. Feel free to suggest a differnt method!

---

## License

MIT License

---

## Disclaimer

This project is not affiliated with or endorsed by Dometic.  
Use at your own risk.

---

<img width="561" height="573" alt="image" src="https://github.com/user-attachments/assets/085caf8b-fc69-473a-9bc8-3142b9342a56" />
---
<img width="513" height="68" alt="image" src="https://github.com/user-attachments/assets/be98113c-ae03-45b1-b2e9-d4b56e321134" />


