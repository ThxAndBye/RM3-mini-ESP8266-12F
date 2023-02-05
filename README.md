# RM3-mini-ESP8266-12F
Replace the original uC PCB from the BroadLink RM3 mini with a ESP8266-12F

# BOM
|Part       |Name                   |Amount  |
|-----------|-----------------------|--------|
|R1-R2      |0603 5.1kΩ             |2x      |
|R3-R6      |0603 10kΩ              |4x      |
|R7         |0603                   |1x      |
|C1-C2      |0603 100nF             |2x      |
|C3-C4      |D5mm P2.5mm radial 47µF|2x      |
|C5         |0603 10µF              |1x      |
|D4         |0603 LED               |1x      |
|J1         |TYPE-C-31-M-12         |1x      |
|J3-J4      |2.54mm pin header      |9x      |
|U1         |ESP8266-12F            |1x      |
|U2         |AMS1117 3.3V           |1x      |
|RST & FLASH|6mm Push button        |2x      |

# PCB
## Front
![PCB front view](/RM3%20mini%20ESP%20front.png)

## Back
![PCB front view](/RM3%20mini%20ESP%20back.png)

## Example ESPHome configuration
This example receives all IR codes and prins them to the log.
It also has the commands configured for the AUDIOPHONICS PRE-03.

```
esphome:
  name: "em3-mini"
  platform: esp8266
  board: esp01_1m

# Enable logging
logger:

# Enable Home Assistant API
api:

ota:


wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "Esphome-Web-09F729"
    password: "QghgvaHWUIw2"

captive_portal:

light:
  - platform: status_led
    id: status
    name: "Status LED"
    pin: GPIO2
    
remote_receiver:
  pin:
    number: GPIO14
    inverted: true
  dump: all

remote_transmitter:
  pin: GPIO13
  carrier_duty_percent: 50%

button:
  - platform: template
    name: "Volume Up"
    on_press:
      - remote_transmitter.transmit_nec:
          address: 0xFF00
          command: 0xEF10
  - platform: template
    name: "Volume Down"
    on_press:
      - remote_transmitter.transmit_nec:
          address: 0xFF00
          command: 0xEB14
  - platform: template
    name: "Mute"
    on_press:
      - remote_transmitter.transmit_nec:
          address: 0xFF00
          command: 0xED12
  - platform: template
    name: "Switch input left"
    on_press:
      - remote_transmitter.transmit_nec:
          address: 0xFF00
          command: 0xEE11
  - platform: template
    name: "Switch input right"
    on_press:
      - remote_transmitter.transmit_nec:
          address: 0xFF00
          command: 0xEC13  
```
