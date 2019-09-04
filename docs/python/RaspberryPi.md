---
layout: default
title: RaspberryPi
nav_order: 7
parent: Python
---

## RaspberryPi Application Docs
### Using I2C_tools
You have an I2C device attached to your Raspberry Pi, and you want to know how to
check that it’s attached and to find its I2C address.

``` bash
sudo apt-get install i2c-tools
sudo i2cdetect -y 1
```

### stream cctv
```bash
./mjpg_streamer -i "./input_uvc.so" -o "./output_http.so -w ./www"
```
