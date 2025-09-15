# Using DFRobot FireBeetle 2 ESP32-C6 with PlatformIO (Arduino Framework)

## Prerequisites

- PlatformIO installed (via VSCode extension or `pip install platformio`)
- Access to a PioArduino platform repository:
  - [pioarduino/platform-espressif32](https://github.com/pioarduino/platform-espressif32)

## Adding the FireBeetle 2 ESP32-C6 Board Definition

1. **Add the board definition file to the `boards/` directory of your custom platform repo.**

   Example: `boards/dfrobot_firebeetle2_esp32c6.json`
   ```json
   {
     "build": {
       "core": "esp32",
       "extra_flags": [
         "-DARDUINO_DFROBOT_FIREBEETLE_2_ESP32C6",
         "-DARDUINO_USB_MODE=1",
         "-DARDUINO_USB_CDC_ON_BOOT=1"
       ],
       "f_cpu": "160000000L",
       "f_flash": "80000000L",
       "flash_mode": "qio",
       "mcu": "esp32c6",
       "variant": "dfrobot_firebeetle2_esp32c6"
     },
     "connectivity": [
       "wifi",
       "bluetooth",
       "zigbee",
       "thread"
     ],
     "debug": {
       "openocd_target": "esp32c6.cfg"
     },
     "frameworks": [
       "arduino",
       "espidf"
     ],
     "name": "DFRobot FireBeetle 2 ESP32-C6",
     "upload": {
       "flash_size": "4MB",
       "maximum_ram_size": 327680,
       "maximum_size": 4194304,
       "require_upload_port": true,
       "speed": 460800
     },
     "url": "https://wiki.dfrobot.com/SKU_DFR1075_FireBeetle_2_Board_ESP32_C6/",
     "vendor": "DFRobot"
   }
   ```
  
This file can be downloaded from https://github.com/epsilonrt/platform-espressif32/blob/develop/boards/dfrobot_firebeetle2_esp32c6.json


2. **Configure your PlatformIO project**

   In your `platformio.ini`:
   ```ini
   [env:dfrobot_firebeetle2_esp32c6]
   platform = https://github.com/pioarduino/platform-espressif32.git
   board = dfrobot_firebeetle2_esp32c6
   framework = arduino
   ```

3. **Build and upload as usual**
   ```sh
   pio run
   pio run --target upload
   ```

## Resources

- [DFRobot FireBeetle 2 ESP32-C6 Wiki](https://wiki.dfrobot.com/SKU_DFR1075_FireBeetle_2_Board_ESP32_C6/)
- [Arduino-ESP32 Core (C6 support)](https://github.com/espressif/arduino-esp32)
- [PlatformIO Documentation](https://docs.platformio.org/)

---

*Guide generated with GitHub Copilot assistance.*