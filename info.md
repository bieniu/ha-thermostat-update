## Configuration example
```yaml
- id: update_thermostats
  alias: 'Update Thermostats'
  trigger:
    platform: state
    entity_id:
      - climate.thermostat_kitchen
      - climate.thermostat_bedroom
      - climate.thermostat_bathroom
      - sensor.temperature_kitchen
      - sensor.temperature_bedroom
      - sensor.temperature_bathroom
  condition:
    condition: template
    value_template: >-
      {% if "thermostat" in trigger.entity_id and trigger.to_state.attributes.current_temperature == none %}
         true
      {% elif "sensor" in trigger.entity_id %}
         true
      {% else %}
         false
      {% endif %}
  action:
    service: python_script.thermostat_update
    data_template:
      heat_stat: 'auto'
      idle_state: 'idle'
      idle_heat_temp: 10
      thermostat: >-
         {% if "thermostat" in trigger.entity_id %}
            {{ trigger.entity_id }}
         {% else %}
            climate.thermostat_{{ trigger.entity_id | replace('sensor.temperature_', '') }}_heating_1
         {% endif %}
      sensor: >-
         {% if "sensor" in trigger.entity_id %}
            {{ trigger.entity_id }}
         {% else %}
            sensor.temperature_{{ (trigger.entity_id | replace('climate.thermostat_', '')) | replace('_heating_1', '') }}
         {% endif %}
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
