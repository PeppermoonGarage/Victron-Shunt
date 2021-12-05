# Victron-SmartShunt

This will be to read Victron Smartshunt data over Ve.Direct using a D1 Mini ESP8266
YAML starts here:


esphome:
  name: d1mini-2
  platform: ESP8266
  board: d1_mini

mqtt:                                                                                                 
  broker: 192.168.1.101                                                                                  
  port: 1883                                                                                         
  username: mqttuser
  password: reallygoodpassword
  topic_prefix: d1mini-SmartShunt
  
# Enable logging
logger:

# Enable Home Assistant API
api:

external_components:
  - source: github://KinDR007/VictronSmartShunt-ESPHOME@main
    refresh: 0s

substitutions:
  lower_devicename: "smartshunt"
  devicename: "SmartShunt_500A"
  config_version: "v2021.12.04.001"
  # wifi_fast_connect: "false"
  # wifi_reboot_timeout: 0s
  accuracy: "2"

ota:
  password: "eae798b8879353f1dc56ba8319e086fb"

wifi:
  networks:
  - ssid: "SSID-Name"
    password: "SSID-Password"


  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "D1Mini-2 Fallback Hotspot"
    password: "reallygoodpassword"

captive_portal:

time:
  - platform: homeassistant

uart:
  id: uart0
#  tx_pin: D8  #D8 not used here
  rx_pin: D7
  baud_rate: 19200
  stop_bits: 1
  data_bits: 8
  parity: NONE
  rx_buffer_size: 256

victron_smart_shunt:
  uart_id: uart0

sensor:
#  - platform: wifi_signal
#    name: "${devicename} WiFi Signal Sensor"
#    id: rssi_sensor
#    update_interval: 15s

#  - platform: template
#    name: "Wifi Quality"
#    unit_of_measurement: "%"
#    accuracy_decimals: 0
#    icon: "mdi:wifi"
#    update_interval: 15s
#    lambda: |-
#        int quality;
#        const int rssi = id(rssi_sensor).state;
#          if (rssi <= -100) { quality = 0;}
#          else if (rssi >= -50)
#          {  quality = 100; }
#          else
#          {  quality = 2 * (rssi + 100); }
#          return quality;
#  - platform: uptime
#    name: "${devicename} Uptime"
#    id: uptime_s
#    update_interval: 5s

  - platform: victron_smart_shunt
    battery_voltage:
      name: "Battery Voltage"
      id: bv
      accuracy_decimals: ${accuracy}

    battery_current:
      name: "Battery Current"
      id: bc
      accuracy_decimals: ${accuracy}

    fw_version:
      name: "fw"
      id: fw

    pid:
      name: "pid"
      id: pid

    instantaneous_power:
      name: "instantaneous power"
      id: instantaneous_power
      accuracy_decimals: ${accuracy}

    time_to_go:
      name: "time to go"
      id: time_to_go

    consumed_amp_hours:
      name: "consumed amp hours"
      id: consumed_amp_hours
      unit_of_measurement: Ah
      accuracy_decimals: ${accuracy}

    min_battery_voltage:
      name: "Min battery voltage"
      id: min_battery_voltage
      accuracy_decimals: ${accuracy}

    max_battery_voltage:
      name: "Max battery voltage"
      id: max_battery_voltage
      accuracy_decimals: ${accuracy}

    amount_of_charged:
      name: "Amount of charged"
      id: amount_of_charged
      filters:
        - multiply: 0.001
      unit_of_measurement: kWh
      accuracy_decimals: ${accuracy}

#    bmv_alarm_text:
#      name: "BMV alarm"
#      id: bmv_alarm

#    bmv_text:
#      name: "bmv - text"
#      id: bmv_pid

    last_full_charge:
      name: "Time since last full charge"
      id: last_full_charge

    deepest_discharge:
      name: "Depth of the deepest discharge"
      id: deepest_discharge
      unit_of_measurement: Ah
      accuracy_decimals: ${accuracy}

    last_discharge:
      name: "Depth of the last discharge"
      id: last_discharge
      unit_of_measurement: Ah
      accuracy_decimals: ${accuracy}

    discharged_energy:
      name: "Amount of discharged energy"
      id: discharged_energy
      filters:
        - multiply: 0.001
      unit_of_measurement: kWh
      accuracy_decimals: ${accuracy}

    state_of_charge:
      id: state_of_charge
      name: "SoC"
  


