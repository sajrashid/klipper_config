# klipper Config

> Sapphire Pro using **BigTreeTech SKR 1.4** & the RPI as a secondary MCU configured with 3d touch see Bltouch offsets & mesh bed levelling sections, please note: Extruder is running in direct drive mode. configured with [Klipper](https://github.com/KevinOConnor/klipper), [Fluidd](https://github.com/cadriel/fluidd) & [Moonraker](https://github.com/Arksine/moonraker)
> 

**Get USB Id**
* `ls /dev/serial/by-id/*`
* update the printer.cfg with the output, eg: *`serial:/dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0`*

**Set-up RPI as secondary MCU**
* Install the rc script
* `cd ~/klipper/`
* `sudo cp ./scripts/klipper-mcu.service /etc/systemd/system/`
* `sudo systemctl enable klipper-mcu.service`
* Select `Linux process`
* `make menuconfig`
* `sudo service klipper stop`
* `make flash`
* `sudo service klipper start`
* `sudo usermod -a -G tty pi` ** only required if  If klippy.log reports a “Permission denied” error


**Calibrate Z-offset**
* `PROBE`
* Place a piece of paper under the nozzle
* `PROBE_CALIBRATE`
* `TESTZ Z=-.5` (moves nozzle 0.5 closer to the paper/bed)
* `ACCEPT` once correct offset
* `SAVE_CONFIG` will restart the MCU


##### Section below only applies if using SKR to PI serial UART connection 

Using GPIO [See vid here for wiring](https://www.youtube.com/watch?v=AtW3GqkKUz8-Q&t=14m39s) Connect RX , TX & GND pins from TFT header to PI UART GPIO pins 14 & 15 and any Gnd pin **ensuring RX & TX are crossed**, see here for the [PI UART PINOUT](https://pinout.xyz/pinout/pin8_gpio14). To power the PI from the SKR connect 5v to Pin 2 on the PI
  
  **Software Config**
  * Swapping ports used by GPIO and Bluetooth
  * `sudo nano /boot/config.txt` & append `dtoverlay=pi3-miniuart-bt`
  * Disable the serial console
  * `sudo nano /boot/cmdline.txt` Look for following string (text) and delete `console=serial0,115200`

  **Pi serial config**
  * `sudo raspi-config`
  * select `*Interfacing Options*`
  * `*P6 - Serial*`
  * `*No*`
  * `*Yes*`
  * Exit and reboot.

  Rebuild your Klipper MCU firmware
  * unselect Use `*USB for communication*` (instead of serial)
  * Flash updated firmware to your board

  Update your printer.cfg:
  * *`serial: /dev/ttyAMA0`*

  Related python scripts
  [See here for additional RPI scripts ](https://github.com/sajrashid/RpiPythonScripts)


