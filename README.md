:coffee: [Buy Me A Coffee](https://buymeacoffee.com/mvroosmalen)! :coffee:
# ESPHome-Duepi
The Duepi EVO climate platform is a reverse engineered implementation of the app which is controlling Pellet stove heaters using a Duepi Evo Wifi module. With this module it is possible to control your pellet stove with **HomeAssistant**. This is in no way associated with the company Duepi and comes with no guarantees or warranty. Use at your own risk. Optionally one can send a four character code to the stove to test out new commands

![image](https://github.com/user-attachments/assets/37a8dd07-30b7-46e1-8ba0-1c56234960a2)
![Screenshot_2025-02-21_19-00-56](https://github.com/user-attachments/assets/50f06f76-f7b8-4078-a9bc-d7b59a99f2d2)


## Prerequisites
- Hardware: Wemos D1 flashed with ESPHome. This device has a 5V input and integrated CH340 for easy flashing. 

## Functionality
- Control target temperature (10-35°C).
- Control system on/off.
- Control fan speed (quite, low, middel, medium, high)
- Reset errors (ie out of pellet)
- Automation possible
- Send custom commands
- PCB temperature
- Full history of temperature, fanspeed etc.
- DUEPI firmware detected
- running time stove reported (total life time and after cleaning reset), helps to get an early warning before stove signal the three beeps ;-)

### Configuration
Paste **pelletstove.yaml** in ESPHome flashed device and <ins>change the underlined parts</ins>:  
encryption:  
..key: <ins>!secret encryption_key</ins>  
wifi:  
..networks:  
....- ssid: <ins>!secret wifi_ssid</ins>  
......password: <ins>!secret wifi_password</ins>  
....- ssid: <ins>!secret wifi_ssid1</ins>  
......password: <ins>!secret wifi_password1</ins>  
ota:  
..- platform: esphome  
....password: "<ins>CreateYourOwn</ins>"  
ap:  
..ssid: "Pelletkachel Fallback Hotspot"  
..password: "<ins>PelletKachel</ins>"  
  
Compile and install on the Wemos. Connect the correct pins of the Wemos to the DEUPI board:
![image](https://github.com/user-attachments/assets/2958a20d-82da-41a6-a7fe-a692134b9652)  
![image](https://github.com/user-attachments/assets/4cef9ac5-132b-4bb8-838a-5a8e09bb705e)  
![image](https://github.com/user-attachments/assets/f2125298-5b24-4814-8c65-a8f1f51754c9)  
tx_pin: GPIO-01  
rx_pin: GPIO-03  
GND-pin  
5V-pin  

### In Home assistent to activate custom commands for DUEPI stoves: 
  1) Create text helper <pelletkachel_command> (length 4 characters (min and max))
  2) Create automation: (with action select your esphome......write)      
        
![Image](https://github.com/user-attachments/assets/87e80384-265d-46bc-ab80-0f229b88fc11) <br />
Note replace **pelletkachel** in action: esphome.**pelletkachel**_write with name of ESPHome yaml<br />

## Confirmed working with:  
- Artel watt 9
- Duroflame Rembrand  
- Qlima Viola 85 S-Line 



Huge thanks goes to aceindy who found the solution for the HACS **HomeAssistant** integration, which I used to create this solution!

[Buy Me A Coffee](https://buymeacoffee.com/mvroosmalen)! :coffee:
