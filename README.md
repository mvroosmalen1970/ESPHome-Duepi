# ESPHome-Duepi
The Duepi EVO climate platform is a reverse engineered implementation of the app which is controlling Pellet stove heaters using a Duepi Evo Wifi module. With this module it is possible to control your pellet stove with HomeAssistant. This is in no way associated with the company Duepi and comes with no guarantees or warranty. Use at your own risk.

Prerequisites
Hardware
Wemos D1 flashed with ESPHome. This device has a 5V input and integrated CH340 for easy flashing. 

Functionality
Control target temperature.
Control system on/off.
Control fan speed (quite, low, middel, medium, high)

Configuration
download and store uart_read_line_sensor.h in the ESPHome directory (/homeassistant/esphome)
Paste pelletstove.yaml in ESPHome flashed device

Confirmed working with:
  Duroflame Rembrand


Huge thanks go to pascal_bornat@hotmail.com who found the strings to control the EVO board and interfaced it to Jeedom

Buy Me A Coffee! 
