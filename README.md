## Introduction

ESP32 firmware sample project for OTA update using ThingsBoard and for sending telemetry to ThingsBoard.  
See all details in [ESP32 OTA using ThingsBoard](https://thingsboard.io/docs/samples/esp32/ota/) tutorial.

ThingsBoard is an open-source server-side platform that allows you to monitor and control IoT devices. It is free for both personal and commercial usage and you can deploy it anywhere. If this is your first experience with the platform we recommend to review what-is-thingsboard page and getting-started guide.

ESP32 is a series of low-cost, low-power SOC microcontrollers with integrated self-contained Wi-Fi and dual-mode Bluetooth.
This sample application allow you to deliver a new firmware images to EPS32 with using ThingsBoard and OTA.

## Setup

Create an alias for easy-use of the ESP-IDF (in ~/.zshrc):
    
    alias get_idf='. $HOME/esp/esp-idf/export.sh'

Next, from the VS code terminal configure the project:

    get_idf
    rm -r build sdkconfig
    idf.py set-target esp32s3
    idf.py menuconfig

 Follow the instructions at [ESP32 OTA using ThingsBoard](https://thingsboard.io/docs/samples/esp32/ota/), but change the following:
  - 'Serial flasher config > Flash size' = 8 MB (for the ESP32-S3 N8).
  - 'Serial flasher config > 'idf.py monitor' baud rate = 115200 bps
 
 Next, build and flash the project:

    idf.py build
    idf.py -p /dev/cu.usbserial-1110 flash monitor

### Certificate

The initially provided certificate was not valid anymore, so this had to be replaced. First get the new public certificate:

    openssl s_client -showcerts -connect raw.githubusercontent.com:443

Replace 'server_certs/ca_cert.pem' with the certificate chain in the output. Now, any new binary built and flashed should allow
OTA updates from raw.githubusercontent.com over port 443.

    idf.py clean build
    idf.py -p /dev/cu.usbserial-1110 flash monitor

Ensure that any new binary images are also built with this certificate. Otherwise you could expect the initial OTA update to be OK, but
the new image will fail with the following error when attempting another OTA update:

    I (5704) tb_ota: Target firmware version: v1.2
    I (5704) tb_ota: Firmware URL: https://raw.githubusercontent.com/bluelabsw/esp32-ota-poc/master/firmware/esp32_ota-v1.2.bin
    E (5774) esp-tls-mbedtls: mbedtls_ssl_handshake returned -0x2700
    I (5774) esp-tls-mbedtls: Failed to verify peer certificate!
