# Thermostat Update
This script updates Z-Wave thermostat entity (e.g. Danfoss 014G0013) state and current temperature
from external sensor in the [Home Assistant](https://home-assistant.io/).

This script use Home Assistant [python_script](https://www.home-assistant.io/components/python_script/) component.

You can install this script via [HACS](https://custom-components.github.io/hacs/) or just download `thermostat_update.py` file and save it in `/config/python_scripts` folder.

Go to [HA community](https://community.home-assistant.io/t/update-current-temperature-for-z-wave-thermostats/32834) for support and help.

## Configuration example
```yaml
service: python_script.thermostat_update
data:
  thermostat: climate.thermostat_kitchen
  sensor: sensor.temperature_kitchen
  heat_stat: 'auto'
  idle_state: 'idle'
  idle_heat_temp: 10
```
## Script configuration
key | optional | type | default | description
-- | -- | -- | -- | --
`thermostat` | False | string | | thermostat `entity_id`
`sensor` | True | string | | sensor `entity_id`
`heat_state` | True | string | `heat` | name of heating state, changing this from default value will broke compatibility with HomeKit
`idle_state` | True | string | `off` | name of idle state, changing this from default value will broke compatibility with HomeKit
`idle_heat_temp` | True | float | `8` | temperature value between `idle` and `heat` states
`state_only` | True | boolean | `false` | with `state_only` set to `true` script will update only state of the thermostat
