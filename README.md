# Cover Time Based Component
Cover Time Based Component for your Home-Assistant (http://www.home-assistant.io)

With this component you can add a time-based cover. You can set a switch to open and another to close the cover. 
For example, you can use an Aqara Wall Switch Double Key and standard 4 wire motor roller shutters to do this.

<a href='https://ko-fi.com/A0A12HZT1' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://cdn.ko-fi.com/cdn/kofi4.png?v=2' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>

## Installation
* Copy all files in custom_components/cover_time_based to your <config directory>/custom_components/cover_time_based/ directory.
* Restart Home-Assistant.
* Add the configuration to your configuration.yaml file.

### Usage
To use this component in your installation, add the following to your configuration.yaml file:

### Example configuration.yaml entry

```
cover:
  - platform: cover_time_based
	devices:
	  room_rolling_shutter:
	   name: Room Rolling Shutter
	   travelling_time_down: 23
	   travelling_time_up: 25
	   open_switch_entity_id: switch.wall_switch_right
	   close_switch_entity_id: switch.wall_switch_left
	   aliases:
	    - room_rolling_shutter
```
