# Sensirion Raspberry Pi I2C SEN44 Driver

This document explains how to set up the SEN44 sensor module to run on a Raspberry Pi using the provided code.

<center><img src="images/SEN4x.png" width="500px"></center>

## Setup Guide

### Connecting the Sensor

Your sensor has the five different connectors: VCC, GND, SDA, SCL, SEL (the sixth connector will not be used for now).
Use the following pins to connect your SEN44:

*SEN44*  |    *Raspberry Pi*           |   *Jumper Wire*   |
 :------: | :-------------------------: | :---------------: |
VCC    |        Pin 2 (5V)           |   Red             |
GND    |        Pin 6                |   Black           |
SDA    |        Pin 3                |   Green           |
SCL    |        Pin 5                |   Yellow          |
SEL    |        Pin 9 (GND for I2C)  |   Blue            |

<center><img src="images/GPIO-Pinout-Diagram.png" width="900px"></center>

<center><img src="images/SEN44_pinout.png" width="300px"></center>

| *Pin* | *Name* | *Description* | *Comments* |
|-------|--------|---------------|------------|
| 1     | VDD    | Supply Voltage | 5V ±10%
| 2     | GND    | Ground |
| 3     | RX     | UART: Receiving pin for communication | TTL 5V and LVTTL 3.3V compatible
|       | SDA    | I2C: Serial data input / output | TTL 5V and LVTTL 3.3V compatible
| 4     | TX     | UART: Transmission pin for communication | TTL 5V and LVTTL 3.3V compatible
|       | SCL    | I2C: Serial clock input | TTL 5V and LVTTL 3.3V compatible
| 5     | SEL    | Interface select | Leave floating or pull to VDD to select UART
|       |        |  | Pull to GND to select I2C
| 6     | NC     | Do not connect |

### Raspberry Pi

- [Install the Raspberry Pi OS on to your Raspberry Pi](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up)
- [Enable the I2C interface in the raspi-config](https://www.raspberrypi.org/documentation/configuration/raspi-config.md)
- Download the driver for the [Sensirion Github Page](https://github.com/Sensirion/raspberry-pi-i2c-sen44) and extract
  the `.zip` on your Raspberry Pi
- Compile the driver
    1. Open a [terminal](https://www.raspberrypi.org/documentation/usage/terminal/?)
    2. Navigate to the driver directory. E.g. `cd ~/raspberry-pi-i2c-sen44`
    3. Run the `make` command to compile the driver

       Output:
       ```
       rm -f sen44_i2c_example_usage
       cc -Os -Wall -fstrict-aliasing -Wstrict-aliasing=1 -Wsign-conversion -fPIC -I. -o sen44_i2c_example_usage  sen44_i2c.h sen44_i2c.c sensirion_i2c_hal.h sensirion_i2c.h sensirion_i2c.c \
       	sensirion_i2c_hal.c sensirion_config.h sensirion_common.h sensirion_common.c sen44_i2c_example_usage.c
       ```
- Test your connected sensor
    - Run `./sen44_i2c_example_usage` in the same directory you used to compile the driver.

      Output:
      ```
      Serial number: C081C2651224B6FE
      Firmware: 0.4, Hardware: 6.0
      Mass concentration pm1p0: 1 µg/m³
      Mass concentration pm2p5: 5 µg/m³
      Mass concentration pm4p0: 9 µg/m³
      Mass concentration pm10p0: 11 µg/m³
      Voc index: 0.0
      Ambient humidity: 36.77 %RH
      Ambient temperature: 26.20 °C
      Mass concentration pm1p0: 4 µg/m³
      Mass concentration pm2p5: 6 µg/m³
      Mass concentration pm4p0: 7 µg/m³
      Mass concentration pm10p0: 8 µg/m³
      Voc index: 0.0
      Ambient humidity: 36.77 %RH 
      Ambient temperature: 26.20 °C
      ...
      ```

## Troubleshooting

### Initialization failed

- Ensure that you connected the sensor correctly: All cables are fully plugged in and connected to the correct pin.
- Ensure that I2C is enabled on the Raspberry Pi. For this redo the steps on
  "Enable the I2C interface in the raspi-config" in the guide above.
- Ensure that your user account has read and write access to the I2C device. If it only works with user
  root (`sudo ./sen44_i2c_example_usage`), it's typically due to wrong permission settings.
