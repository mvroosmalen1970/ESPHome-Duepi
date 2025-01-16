[Buy Me A Coffee](https://buymeacoffee.com/mvroosmalen)! :coffee:
# ESPHome-Duepi
The Duepi EVO climate platform is a reverse engineered implementation of the app which is controlling Pellet stove heaters using a Duepi Evo Wifi module. With this module it is possible to control your pellet stove with HomeAssistant. This is in no way associated with the company Duepi and comes with no guarantees or warranty. Use at your own risk.

![image](https://github.com/user-attachments/assets/37a8dd07-30b7-46e1-8ba0-1c56234960a2)
![image](https://github.com/user-attachments/assets/40634a7e-6a66-4fc2-8591-994915ee75c1)

Prerequisites
Hardware
Wemos D1 flashed with ESPHome. This device has a 5V input and integrated CH340 for easy flashing. 

Functionality
Control target temperature.
Control system on/off.
Control fan speed (quite, low, middel, medium, high)
Reset errors (ie out of pellet)
Automation possible 

Configuration
download and store uart_read_line_sensor.h in the ESPHome directory (/homeassistant/esphome)
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

Confirmed working with:  
  Duroflame Rembrand  


Huge thanks goes to aceindy who found the solution for home assitent intergation, which I used to create this solution!

[Buy Me A Coffee](https://buymeacoffee.com/mvroosmalen)! :coffee:
