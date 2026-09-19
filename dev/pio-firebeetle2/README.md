# Utiliser la DFRobot FireBeetle 2 ESP32-C6 avec PlatformIO (framework Arduino)

## Prérequis

- PlatformIO installé (via l'extension VSCode ou `pip install platformio`)
- Accès à un dépôt de plateforme PioArduino :
  - [pioarduino/platform-espressif32](https://github.com/pioarduino/platform-espressif32)

## Ajouter la définition de carte FireBeetle 2 ESP32-C6

1. **Placez le fichier de définition de carte dans le dossier `boards/` de votre dépôt de plateforme personnalisé.**

   Exemple : `boards/dfrobot_firebeetle2_esp32c6.json`
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

Ce fichier peut être téléchargé depuis https://github.com/epsilonrt/platform-espressif32/blob/develop/boards/dfrobot_firebeetle2_esp32c6.json

2. **Configurez votre projet PlatformIO**

   Dans votre `platformio.ini` :
   ```ini
   [env:dfrobot_firebeetle2_esp32c6]
   platform = https://github.com/pioarduino/platform-espressif32.git
   board = dfrobot_firebeetle2_esp32c6
   framework = arduino
   ```

3. **Compilez et téléversez comme d'habitude**
   ```sh
   pio run
   pio run --target upload
   ```

## Ressources

- [Wiki DFRobot FireBeetle 2 ESP32-C6](https://wiki.dfrobot.com/SKU_DFR1075_FireBeetle_2_Board_ESP32_C6/)
- [Cœur Arduino-ESP32 (prise en charge du C6)](https://github.com/espressif/arduino-esp32)
- [Documentation PlatformIO](https://docs.platformio.org/)

---

*Guide rédigé avec l'assistance de GitHub Copilot.*
