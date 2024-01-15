# ESP32-C3-super-mini
Playing around with the ESP32C3 Super Mini board:
- connected to the W5500 ethernet module, using the fantastic work of Jeroen Van Oort described in https://github.com/esphome/esphome/pull/4424
- connected to a I²C BMP280 temperature/pressure sensor

# Hardware wiring schema
<img src="/../main/pictures/esp32c3-super-mini-w5500-bmp280.png" width="40%" alt= "Schematic" height="40%">

# ESPHome config
``` yaml
esphome:
  name: esp32c3-super-mini
  friendly_name: esp32c3-super-mini

esp32:
  board: esp32-c3-devkitm-1
  framework:
    type: arduino

web_server:
  port: 80

# Enable logging
logger:
  level: DEBUG

# Enable Home Assistant API
api:

ota:

external_components:
- source:
    type: git
    url: https://github.com/JeroenVanOort/esphome/
    ref: eth-w5500
  components:
  - ethernet

ethernet:
  type: W5500
  mosi_pin: GPIO10
  miso_pin: GPIO09
  clk_pin: GPIO08
  cs_pin: GPIO5
  reset_pin: GPIO04
  interrupt_pin: GPIO03
  clock_speed: 25MHz

i2c:
  scl: GPIO07
  sda: GPIO06
  scan: True
  id: bus_a

sensor:
  - platform: bmp280
    temperature:
      name: "Temperatuur"
      unit_of_measurement: °C
      accuracy_decimals: 1
    pressure:
      name: "Luchtdruk"
      unit_of_measurement: hPa
      accuracy_decimals: 0
    i2c_id: bus_a
    address: 0x76

```
