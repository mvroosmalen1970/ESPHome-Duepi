:coffee: [Buy Me A Coffee](https://buymeacoffee.com/mvroosmalen)! :coffee:
# ESPHome-Duepi
The Duepi EVO climate platform is a reverse engineered implementation of the app which is controlling Pellet stove heaters using a Duepi Evo Wifi module. With this module it is possible to control your pellet stove with **HomeAssistant** or **Homey** or official **MyDPRemote** (or alternative) app. <br />
<br />
<img width="437" height="550" alt="image" src="https://github.com/user-attachments/assets/013d3f29-9061-4317-97e5-6874fd74bb2b" />
![Screenshot_2025-02-21_19-00-56](https://github.com/user-attachments/assets/50f06f76-f7b8-4078-a9bc-d7b59a99f2d2)

## New:
- moved all coding to seperate files so future updates are used directly when you flash ESPHome updates and included one main package file for smooth future updates.
  **If you are updating please update pelletstove.yaml with at least these changes (as we made some major changes!!)**
```
substitutions:
  min_temp: "10"     #set minimum temperature for climate
  max_temp: "35"     #set maximum temperature for climate
  temp_unit: "°C"    #set climate unit, use "°C" or "°F"
  tx_pin_esp: '1'    #TX pin on esp module
  rx_pin_esp: '3'    #RX pin on esp module

packages:
  duepi:
    url: https://github.com/mvroosmalen1970/ESPHome-Duepi.git
    file: components/packages.yaml
    ref: main 
```
- If you like to test a beta version change **ref: main** to **ref: mvroosmalen1970-patch-xx** in pelletstove.yaml (xx is the patch number)

## Prerequisites
- Hardware: Wemos D1 or ESP32-C3 flashed with **ESPHome**. These devices have a 5V input and integrated CH340 for easy flashing.
- ESPHome installed in **Homey** or **HomeAssistent**
<br />

## Functionality
- Control target temperature (10-35°C). This can be modified. <br />
- Control system on/off.<br />
- Control fan speed (quite, low, middel, medium, **high**) <img width="90" height="32" alt="image" src="https://github.com/user-attachments/assets/1048d35c-573d-4a67-a25d-c7a30c085270" /> <br />
- Reset errors (ie out of pellet) <br />
- Automation possible using any of the reported **Sensors** or **Controls**  <br />
- Optional official app **MyDPRemote** (or alternative) compatibility mode on TCP port 2000 <br />
- Hidden installer mode for advanced service parameter access <br />
- PCB temperature <br />
- Full history of all **Sensors** or **Controls**  (ie temperature, fanspeed....) <br />
- DUEPI firmware detected <br />
- running time stove reported (total life time and after cleaning reset), helps to get an early warning before stove sends a signal (the three beeps on startup 😊) <br />
- Optional ability to send a four character code to the stove to test out new commands. <br />

### Configuration
Install ESPHome in HomeAssistent or Homey using one of the links below: <br />
https://homey.app/en-us/app/nl.inversion.esphome/ESPhome/ <br />
https://my.home-assistant.io/redirect/config_flow_start?domain=esphome <br />
<br />

Paste **pelletstove.yaml** in ESPHome and change:
- !secret encryption_key <br />
- !secret wifi_ssid <br />
- !secret wifi_password <br />
- CreateYourOwn <br />
- PelletKachelPWD <br />
with your own settings / preferences and flash device.
```
encryption:
  key: !secret encryption_key
wifi:
  networks:
    - ssid: !secret wifi_ssid
      password: !secret wifi_password
ota:
  - platform: esphome
    password: "CreateYourOwn"
ap:
  ssid: "Pelletkachel Fallback Hotspot"
  password: "PelletKachelPWD"

substitutions:
  min_temp: "10"     #set minimum temperature for climate
  max_temp: "35"     #set maximum temperature for climate
  temp_unit: "°C"    #set climate unit, use "°C" or "°F"
  tx_pin_esp: '1'    #TX pin on esp module
  rx_pin_esp: '3'    #RX pin on esp module
```
Compile and install on the Wemos. Connect the correct pins of the Wemos to the DEUPI board:
![image](https://github.com/user-attachments/assets/2958a20d-82da-41a6-a7fe-a692134b9652)
![image](https://github.com/user-attachments/assets/4cef9ac5-132b-4bb8-838a-5a8e09bb705e)
![image](https://github.com/user-attachments/assets/f2125298-5b24-4814-8c65-a8f1f51754c9) <img width="375" height="164" alt="image" src="https://github.com/user-attachments/assets/6f5d6a06-a65b-42c0-be17-6f6b2117be50" />

tx_pin: GPIO-01
rx_pin: GPIO-03
GND-pin
5V-pin

please report any unknown codes in the debug sensor with a possible explanation of what you think its related to. This will help others and robustness of this code.

If this sounds to complicated contact me for possibilies

### In Home assistent to activate custom commands for DUEPI stoves:
  1) Create text helper <pelletkachel_command> (length 4 characters (min and max))
  2) Create automation: (with action select your esphome......write)
```
alias: send command to pellet kachel
description: ""
triggers:
  - trigger: state
    entity_id:
      - input_text.pelletkachel_command
conditions: []
actions:
  - action: esphome.pelletkachel_write
    data:
      command: "{{ states('input_text.pelletkachel_command') }}"
mode: queued
max: 10
```
Note replace **pelletkachel** in action: esphome.**pelletkachel**_write with name of ESPHome yaml<br />
### Optional activate Official app compatibility mode:
 1) Set <img width="340" height="44" alt="image" src="https://github.com/user-attachments/assets/eb09b55d-9c77-4616-ab7e-aa74ef422038" /><br />
 2) In the MyDPRemote app: Press the setting (wheel top right), choose local and set IP address (192.168.1.xxx or pelletstove.local), port (2000) and give a name <br />
  <img width="115" height="177" alt="image" src="https://github.com/user-attachments/assets/8b293272-f4b0-4282-9289-9ecd4ac9b62d" /> <br />
While the app is connected, Home Assistant temporarily pauses UART control and resumes automatically when the app disconnects. 
### Optional activate installer mode:
 1) Set <img width="309" height="41" alt="image" src="https://github.com/user-attachments/assets/ac82e49b-09df-4a33-9d75-7eb0ec873f45" /><br />
 2) Retreive parameter settings from your stove by running this yaml automation (or manually by entering numbers in installer parameter)
```
alias: Pelletkachel full Parameter Scan
description: Runs through all 107 parameters 
triggers: []
conditions: []
actions:
  - repeat:
      count: 107
      sequence:
        - action: esphome.pelletkachel_read_param
          data:
            param: "{{ repeat.index }}"
        - delay:
            hours: 0
            minutes: 0
            seconds: 3
            milliseconds: 0
mode: single
```
 3) Parameters are reported in Installer status (diagnostic):<br /><img width="576" height="597" alt="image" src="https://github.com/user-attachments/assets/aa50fdff-72d6-45fb-b2a6-e2b137a8bae8" /><br />
 4) Update parameters by filling the fields:<br />
<img width="305" height="172" alt="image" src="https://github.com/user-attachments/assets/a27802d8-fdae-4b05-a85c-da82fff0e404" /><br />
 5) Write parameters by activating <img width="287" height="41" alt="image" src="https://github.com/user-attachments/assets/5995bbcc-d131-4f1a-a9ec-54353da5120a" /><br />
<br />
## Confirmed working with:
- Artel watt 9
- Artel MeNext
- Duroflame Rembrand
- Eva Calòr Thelma (DUEPI EVO VM88 Board). Maybe with some limitations
- Qlima Viola 85 S-Line
- Qlima Delmara 88 S-Line
- Interstove Milano
- NextElite


Huge thanks goes to aceindy who found the solution for the HACS **HomeAssistant** integration, which I used to create this solution!

[Buy Me A Coffee](https://buymeacoffee.com/mvroosmalen)! :coffee:
