This script updates Z-Wave thermostat entity (e.g. Danfoss 014G0013) state and current temperature
from external sensor.

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
## Script arguments
key | optional | type | default | description
-- | -- | -- | -- | --
`thermostat` | False | string | | thermostat `entity_id`
`sensor` | True | string | | sensor `entity_id`
`heat_state` | True | string | `heat` | name of heating state, changing this from default value will broke compatibility with HomeKit
`idle_state` | True | string | `off` | name of idle state, changing this from default value will broke compatibility with HomeKit
`idle_heat_temp` | True | float | `8` | temperature value between `idle` and `heat` states
`state_only` | True | boolean | `false` | with `state_only` set to `true` script will update only state of the thermostat
