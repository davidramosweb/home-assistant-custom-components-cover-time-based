# Cover Time Based Component (RF version)
Cover Time Based Component for your Home-Assistant (http://www.home-assistant.io) based on https://github.com/davidramosweb/home-assistant-custom-components-cover-time-based , modified for RF covers.

With this component you can add a time-based cover. You have to set RF-sending scripts to open, close and stop the cover. 
For example, you can use an RF-Bridge running Tasmota firmware to send out RF codes, with RF-based motor roller shutters to do this.

## Installation
* Copy all files in custom_components/cover_rf_time_based to your <config directory>/custom_components/cover_rf_time_based/ directory.
* Restart Home-Assistant.
* Create the required scripts in scripts.yaml.
* Add the configuration to your configuration.yaml.
* Restart Home-Assistant again.

### Usage
To use this component in your installation, you have to set RF-sending scripts to open, close and stop the cover (see below), and add the following to your configuration.yaml file:

### Example configuration.yaml entry

```
cover:
  - platform: cover_time_based
    devices:
        my_room_cover_time_based:
        name: My Room Cover
        travelling_time_down: 34
        travelling_time_up: 36
        open_script_entity_id: script.rf_myroom_cover_up
        close_script_entity_id: script.rf_myroom_cover_down
        stop_script_entity_id: script.rf_myroom_cover_stop
        aliases:
          - my_room_cover_time_based
```
### Example scripts.yaml entry
This example assumes that you're using an MQTT-RF bridge running Tasmota open source firmware (https://tasmota.github.io/docs/devices/Sonoff-RF-Bridge-433/). Of course you can customize based on what ever other way to trigger these 3 type of movements.
```
'rf_myroom_cover_up':
  alias: 'RF send MyRoom Cover UP'
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

'rf_myroom_cover_down':
  alias: 'RF send MyRoom Cover DOWN'
  sequence:
  - service: mqtt.publish
    data:
      topic: 'cmnd/rf-bridge-1/backlog'
      payload: 'rfraw XXXXXXXXX....XXXXXXXXXX;rfraw 0'
```
