# Thermostat Update
[![GitHub Release][releases-shield]][releases]
[![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg)](https://github.com/custom-components/hacs)
[![Community Forum][forum-shield]][forum]
[![Buy me a coffee][buy-me-a-coffee]](https://www.buymeacoffee.com/QnLdxeaqO)


This script updates Z-Wave thermostat entity (e.g. Danfoss 014G0013) state and current temperature
from external sensor in the [Home Assistant](https://home-assistant.io/).

This script use Home Assistant [python_script](https://www.home-assistant.io/components/python_script/) component.

You can install this script via [HACS](https://custom-components.github.io/hacs/) or just download `thermostat_update.py` file and save it in `/config/python_scripts` folder.

Go to [HA community](https://community.home-assistant.io/t/update-current-temperature-for-z-wave-thermostats/32834) for support and help.

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
      heat_state: 'auto'
      idle_state: 'idle'
      idle_heat_temp: 10
      thermostat: >-
         {% if "thermostat" in trigger.entity_id %}
            {{ trigger.entity_id }}
         {% else %}
            climate.thermostat_{{ trigger.entity_id | replace('sensor.temperature_', '') }}
         {% endif %}
      sensor: >-
         {% if "sensor" in trigger.entity_id %}
            {{ trigger.entity_id }}
         {% else %}
            sensor.temperature_{{ (trigger.entity_id | replace('climate.thermostat_', '')) }}
         {% endif %}
```

## Configuration example for state only update
```yaml
- id: update_thermostats
  alias: 'Update Thermostats'
  trigger:
    platform: state
    entity_id:
      - climate.thermostat_kitchen
      - climate.thermostat_bedroom
      - climate.thermostat_bathroom
  action:
    service: python_script.thermostat_update
    data_template:
      heat_state: 'auto'
      idle_state: 'idle'
      idle_heat_temp: 10
      thermostat: '{{ trigger.entity_id }}'
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
`temp_only` | True | boolean | `false` | with `temp_only` set to `true` app will update only `current_temperature` of the thermostat

[releases]: https://github.com/bieniu/ha-thermostat-update/releases
[releases-shield]: https://img.shields.io/github/release/bieniu/ha-thermostat-update.svg?style=popout
[forum]: https://community.home-assistant.io/t/update-current-temperature-for-z-wave-thermostats/32834
[forum-shield]: https://img.shields.io/badge/community-forum-brightgreen.svg?style=popout
[buy-me-a-coffee]: https://img.shields.io/static/v1.svg?label=%20&message=Buy%20me%20a%20coffee&color=6f4e37&logo=buy%20me%20a%20coffee&logoColor=white

