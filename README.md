# klipper Config

> Sapphire Pro using **BigTreeTech SKR 1.4** & the RPI as a secondary MCU configured with 3d touch see Bltouch offsets & mesh bed levelling sections, please note: Extruder is running in direct drive mode. configured with [Klipper](https://github.com/KevinOConnor/klipper), [Fluidd](https://github.com/cadriel/fluidd) & [Moonraker](https://github.com/Arksine/moonraker)


##### Section below applies to SKR to PI serial UART connection only

Using GPIO [See vid here for wiring](https://www.youtube.com/watch?v=AtW3GqkKUz8-Q&t=14m39s) Connect RX , TX GND pins from TFT header to PI UART GPIO pins 14 & 15 ensuring RX & TX are crossed, see here for the [PI UART PINOUT](https://pinout.xyz/pinout/pin8_gpio14). To power the PI from the printers connect 5v to the Pin 2 on the PI
  
  Software Config
  * Swapping ports used by GPIO and Bluetooth
  * `sudo nano /boot/config.txt` & append `dtoverlay=pi3-miniuart-bt`
  * Disable the serial console
  * `sudo nano /boot/cmdline.txt` Look for following string (text) and delete `console=serial0,115200`

  Raspi-config stuff
  * `sudo raspi-config`
  * Go to 'Interfacing Options'
  * Then P6 - Serial
  * Then No
  * Then Yes
  * Then go down to finish and reboot.

  Rebuild your Klipper MCU firmware
  * unselect "Use USB for communication (instead of serial). 
  * Flash updated firmware to your board

  Update your printer.cfg:
  * serial: /dev/ttyAMA0

  Useful python scripts
  [See vhere for additional RPI scripts ](https://github.com/sajrashid/RpiPythonScripts)


