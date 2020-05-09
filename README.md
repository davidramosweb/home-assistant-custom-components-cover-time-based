# Cover Time Based Component RF (trigger script) version
Cover Time Based Component for your [Home-Assistant](http://www.home-assistant.io) based on [davidramosweb's Cover Time Based Component](https://github.com/davidramosweb/home-assistant-custom-components-cover-time-based), modified for covers triggered by RF commands.

With this component you can add a time-based cover. You have to set triggering scripts to open, close and stop the cover. Position is calculated based on the fraction of time spent by the cover travelling up or down. You can set position from within Home Assistant using service calls.

You can adapt it to your requirements, actually any cover system could be used which uses 3 triggers: up, stop, down. The idea is to embed your triggers into scripts which can be hooked into this component as below. For example, you can use an RF-Bridge running Tasmota firmware to send out RF codes (like in example shown below), with RF-based motor roller shutters to do this.

## Installation
[![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg?style=for-the-badge)](https://github.com/custom-components/hacs)
* Install using HACS, or manually: copy all files in custom_components/cover_rf_time_based to your <config directory>/custom_components/cover_rf_time_based/ directory.
* Restart Home-Assistant.
* Create the required scripts in scripts.yaml.
* Add the configuration to your configuration.yaml.
* Restart Home-Assistant again.

### Usage
To use this component in your installation, you have to set RF-sending scripts to open, close and stop the cover (see below), and add the following to your configuration.yaml file:

### Example configuration.yaml entry

```
cover:
  - platform: cover_rf_time_based
    devices:
        my_room_cover_time_based:
        name: My Room Cover
        travelling_time_up: 36
        travelling_time_down: 34
        close_script_entity_id: script.rf_myroom_cover_down
        stop_script_entity_id: script.rf_myroom_cover_stop
        open_script_entity_id: script.rf_myroom_cover_up
        aliases: #optional
          - my_room_cover_time_based
```
### Example scripts.yaml entry
This example assumes that you're using an MQTT-RF bridge running [Tasmota open source firmware](https://tasmota.github.io/docs/devices/Sonoff-RF-Bridge-433/). Of course you can customize based on what ever other way to trigger these 3 type of movements.
```
'rf_myroom_cover_down':
  alias: 'RF send MyRoom Cover DOWN'
  sequence:
  - service: mqtt.publish
    data:
      topic: 'cmnd/rf-bridge-1/backlog'
      payload: 'rfraw XXXXXXXXX....XXXXXXXXXX;rfraw 0'

'rf_myroom_cover_stop':
  alias: 'RF send MyRoom Cover STOP'
  sequence:
  - service: mqtt.publish
    data:
      topic: 'cmnd/rf-bridge-1/backlog'
      payload: 'rfraw XXXXXXXXX....XXXXXXXXXX;rfraw 0'

 'rf_myroom_cover_up':
  alias: 'RF send MyRoom Cover UP'
  sequence:
  - service: mqtt.publish
    data:
      topic: 'cmnd/rf-bridge-1/backlog'
      payload: 'rfraw XXXXXXXXX....XXXXXXXXXX;rfraw 0'
```

### Icon customization
For proper icon display (opened/closed) customization can be added to `configuration.yaml` based of what type of covers you have, either one by one, or for all covers at once:

```
homeassistant:
  customize_domain: #for all covers 
     cover:
      device_class: shutter
  customize: #for each cover separately
    cover.my_room_cover_time_based:
      device_class: shutter
```
More details in [Home Assistant device class docs](https://www.home-assistant.io/docs/configuration/customizing-devices/#device-class).

### Some tips when using this component with Tasmota RF bridge in automations
Since there's no feedback from the cover about its current state, state is assumed based on the last command sent, and position is calculated based on the fraction of time spent travelling up or down. You need to measure time by opening/closing the cover using the original remote controller, not through the commands sent from Home Assistant (as they may introduce some delay).

Tasmota is able to send out the RF commands very quickly. If some of your covers 'miss' the commands occassionally (you can see that from the fact that the state shown in Home Assistant does not correspond to reality), it may be that those cover motors do not understand the codes when they are sent 'at once' from Home Assistant. They are sent very quickly one after another and Tasmota can cope with that, however this can be too much for the motors themselves.
To prevent that, make sure you **don't use** [cover groups](https://www.home-assistant.io/integrations/cover.group/) containing multiple covers provided by this integration, and also in automation **don't include multipe covers separated by commas** in one service call. You should create separate service calls for each cover, moreover, add 1 second delay between them:
```
- alias: 'Covers down when getting dark'
  trigger:
    - platform: numeric_state
      below: 400
      for: "00:05:00"
      entity_id: sensor.outside_light_sensor
  action:
    - service: cover.close_cover
      entity_id: cover.room_1
    - delay: '00:00:01'
    - service: cover.close_cover
      entity_id: cover.room_2
    - delay: '00:00:01'
    - service: cover.set_cover_position
      data:
        entity_id: cover.room_3
        position: 20
    - delay: '00:00:01'
    - service: cover.set_cover_position
      data:
        entity_id: cover.room_4
        position: 30
```
