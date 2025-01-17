# STM32_mega
STM32F407 based Arduino Mega replacement for speeduino
![alt text](https://pazi88.kuvat.fi/kuvat/Projektikuvat/Random%20projektit/speeduino/20201201_085147.jpg?img=smaller)

## Speeduino board support.

This processor board has been made mainly for the BMW PnP boards that are based on speeduino V0.4 design. Not all arduino Mega pins are connected so support other speeduino boards varies.
 ```diff 
! Official V0.3: Supported - not tested/verified
! Official V0.4: Supported - not tested/verified
+ BMW PnP boards: Supported - tested/verified
- 2001-05 MX5 PNP: Not supported
! 1996-97 MX5 PNP: Supported - not tested/verified
! 1989-95 MX5 PNP: Supported - not tested/verified
- NO2C: Not supported
- UA4C: Partly supported (No Fuel pump/tacho)
```

## Setting up the DIP-switches.

![alt text](https://github.com/pazi88/STM32_mega/blob/main/Pics/DIP_switches.jpg?raw=true)

- If you wish to use the native USB on STM32, set the two USB switches to "On" and FTDI switches to "Off". No Serial0 available in this mode (bluetooth dongles don't work etc.)
- If you wish to use the FTDU USB to Serial converter, set the two USB switches to "Off" and FTDI switches to "On". Serial0 is available on this mode (requires different binary file to work)
- By setting PWR switch to "On" position, the speeduino board is powered by USB. This is usefull for bench testing etc. but for regular use is recommended to have this in "Off" position.
- RST DIP switch is not used or available in some board revisions.


## Easy way of downloading new speeduino FW to this board

1. Install https://www.st.com/content/st_com/en/products/development-tools/software-development-tools/stm32-software-development-tools/stm32-programmers/stm32cubeprog.html
2. Download one of the precombiled Speeduino binaries for this board, located here: https://github.com/pazi88/STM32_mega/tree/main/Speeduino%20binary%20files
3. Press down Boot0 button and connect usb cable to the board. Once connected, release Boot0 button. Alternately press down Boot0 and reset buttons simultaneously when usb is already connected. You should see new device "STM32 Bootloader"
4. Run STM32 Cube Programmer. (In windows, exe is is located at: C:\Program Files (x86)\STMicroelectronics\STM32Cube\STM32CubeProgrammer\bin )
5. From right, select "USB" and click "Connect"
6. Click "Open file" and browse the previously downloaded binary file.
7. Click "Download"
8. Now you should be able to connect to the board using the USB cable and use Tuner Studio. After first FW upload, you can use the "STM32 commands" menu in TS to enter Boot0.

Alternatively you can connect ST-Link dongle to the 4-pins on the PCB labeled "ST-Link" and select "ST-LINK" in the STM32 Cube Programmer. In this way, there is no need to go to Boot0 mode.

## More difficult way of using this board by compiling your own code

To be able to compile and upload for this board, follow these steps:

### Install tool chain
1. Install the arduino IDE
2. Install the stm32duino core from: https://github.com/stm32duino/Arduino_Core_STM32 (version 1.9.0)
3. Install https://www.st.com/content/st_com/en/products/development-tools/software-development-tools/stm32-software-development-tools/stm32-programmers/stm32cubeprog.html
4. Replace the F407xx variant files in STM32 core with these files: https://github.com/pazi88/STM32_mega/tree/main/STM32_core%20files

### Build code
1. Download 202012 or 202103 Speeduino FW release
2. Open the code in Arduino IDE.
3. In arduino IDE select Tools->Board: GENERIC STM32F4 series
4. In Arduino IDE select Tools->Board part number: "BLACK F407VE"
5. In Arduino IDE select Tools->USART support: Enabled (generic serial)
6. In Arduino IDE select Tools->USB support: CDC (generic serial)
7. Open the globals.h file located in project folder
8. Look at line 55..58 and replace line "//#define USE_SPI_EEPROM PB0" with "#define USE_SPI_EEPROM PE1" /*Use M25Qxx SPI flash */
9. Open the init.ino file located in project folder
10. Look at line 1320.. ish and remove the STM32 specific pin mappings for v0.4 board (otherwise the SMT32 mega will lockup)
11. Build the speeduino project. 

### Upload
1. Press down Boot0 button and connect usb cable to the board. Once connected, release Boot0 button. Alternately press down Boot0 and reset buttons simultaneously when usb is already connected. You should see new device "STM32 Bootloader"
2. In Arduino IDE select Upload method: "STM32CubeProgrammer DFU" 
3. Upload the code

Alternatively you can connect ST-Link dongle to the 4-pins on the PCB labeled "ST-Link" and select Upload method"STM32CubeProgrammer SWD". In this way, there is no need to go to Boot0 mode.

After these steps you should be able to connect to the board using the USB cable and use Tuner Studio. After first FW upload, you can use the "STM32 commands" menu in TS to enter Boot0.
