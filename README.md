# LITTLEFS

## LittleFS library for arduino-esp32

- A LittleFS wrapper for Arduino ESP32 of [Mbed LittleFS](https://github.com/ARMmbed/littlefs)
- Based on [ESP-IDF port of joltwallet/esp_littlefs](https://github.com/joltwallet/esp_littlefs) , thank you Brian!
- Functionality is similar to SPIFFS
- Either LITTLEFS or SPIFFS but not both simultaneously can be used in same Arduino project
- [Related PR in esp32 core development](https://github.com/espressif/arduino-esp32/pull/4096) 

#### Warning: Depends on ESP-IDF, esp32 core and esp_littlefs versions

- Tested on master [esp32-core #git b92c58d](https://github.com/espressif/arduino-esp32/commit/b92c58d74b151c7a3b56db4e78f2d3c90c16446f) and on core [release 1.0.4](https://github.com/espressif/arduino-esp32/releases/tag/1.0.4)
- For esp32-core, newer than release 1.0.4 (IDF v3.3 or later) you can comment this line of <b>esp_littlefs.c</b> to use the <b>vfs_littlefs_utime</b> feature:
```
#define CONFIG_LITTLEFS_FOR_IDF_3_2 
```

### Installation

- Copy <b>LITTLEFS</b> to Arduino IDE libraries
</br>(see File > Preferences > Sketchbook location). 

### Usage

- In your existing code, replace SPIFFS like this 
``` 
#define USE_LittleFS

#include <FS.h>
#ifdef USE_LittleFS
  #define SPIFFS LITTLEFS
  #include <LITTLEFS.h> 
#else
  #include <SPIFFS.h>
#endif 
 ```
### Differences with SPIFFS (and in this implementation)

- LittleFS has folders, you my need to tweak your code to iterate files in folders
- Root: /someting  = something, so attention to /
- Lower level littlefs library cannot mount on NULL as partition_label name, while SPIFFS can
- Lower level littlefs library does not need maxOpenFiles parameter
- Speed (LITTLEFS_Test.ino) 1048576 bytes written in 16238 ms / 1048576 bytes read in 918 ms
- Speed (SPIFFS_Test.ino)   1048576 bytes written in 65971 ms / 1048576 bytes read in 680 ms


### Arduino ESP32 LittleFS filesystem upload tool 

- Download the tool jar from [here](https://github.com/lorol/arduino-esp32littlefs-plugin/raw/master/src/bin/esp32littlefs.jar)
- In your Arduino sketchbook directory, create tools directory if it doesn't exist yet.
- Copy the tool into tools directory (the path should be ```<home_dir>/Arduino/tools/ESP32LittleFS/tool/esp32littlefs.jar```).
- You need the [mklittlefs tool](https://github.com/earlephilhower/mklittlefs)  Download the [release](https://github.com/earlephilhower/mklittlefs/releases) and copy it to 
packages\esp32\tools\mkspiffs\<mklittlefs rev. x.x.x>\ or on checkout (dev) environment to: packages\esp32\hardware\esp32\<release>\tools\mklittlefs\
- Restart Arduino IDE. 

## Credits and license

- This work is based on [Mbed LittleFS](https://github.com/ARMmbed/littlefs) , [ESP-IDF port of joltwallet/esp_littlefs](https://github.com/joltwallet/esp_littlefs) , [Espressif Arduino core for the ESP32, the ESP-IDF - SPIFFS Library](https://github.com/espressif/arduino-esp32/tree/master/libraries/SPIFFS)
- Licensed under GPL v2 ([text](LICENSE))

## To Do

- Supporting smarter different IDF versions 