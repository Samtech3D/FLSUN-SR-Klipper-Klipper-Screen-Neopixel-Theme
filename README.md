If you like my Work Buy Me a Coffee! 🌹❤😍💙

[ ![Download](https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/imgaes/Paypal.png) ](https://www.paypal.me/samiartmedia)

**This repository contains settings for Klipper-Klipper screen and neopixel for FLSUN Super Racer with the necessary scripts and macros using robin nano V3 Motherboard.**<br /><br />

<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/imgaes/repository-flsun.png" />

## References
<img align="center" width=100 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/imgaes/klipper.png" />

- https://www.klipper3d.org/
- https://github.com/th33xitus/kiauh
- https://github.com/jordanruthe/KlipperScreen
- https://www.klipper3d.org/Pressure_Advance.html
- https://www.klipper3d.org/Resonance_Compensation.html
- https://www.klipper3d.org/Measuring_Resonances.html

## Hardware
- The recommended hardware is a Raspberry Pi 2, Raspberry Pi 3, or Raspberry Pi 4 https://www.klipper3d.org/FAQ.html
- This install based on MKS Robin Nano V3.0 with tmc2209 stepper drivers and Raspberry Pi4 Touchscreen Display  "Work in Progress for other Boards."
- Raspberry Pi 4 Touchscreen Display https://klipperscreen.readthedocs.io/en/latest/Hardware/
- ADXL 345 for input shaper test (optional) https://www.klipper3d.org/Measuring_Resonances.html

## Installation
- Prepping your OS image (I'm using raspbian buster version to use omx-player on startup to run my splash  screen ).
- Find the Raspberry Pi IP (you can use fing app on your phone).
- Easy way to install klipper with Kiahu https://github.com/th33xitus/kiauh
    - You need to install git first so run on ssh`sudo apt-get install git -y`.
    - After git is installed, use the following commands in the given order to download and execute the script:

```
cd ~

git clone https://github.com/th33xitus/kiauh.git

./kiauh/kiauh.sh
```
### install in order with kiauh script
  #### 1.Klipper
  #### 2.Moonraker
  #### 3.Fluidd or Mansail or Both using a different port for the 2nd install exp:81(I added themes for Mainsail )
  #### 4.Klipper Screen


- On your browser type your rpi Ip to open fluidd interface then import:
    - .theme (folder)
    - printer.cfg
    - klipperScreen.conf
    - macros.cfg
    - led_progress.cfg (if you have neopixel installed) 
<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/imgaes/Import_files.jpg" />   

### Build firmware and flash your motherboard
- Open ssh to compile the microcontroller code, start by running these commands  

```
cd ~/klipper/
make menuconfig
```

### [*] Enable extra low-level configuration options
### Microcontroller Architecture (STMicroelectronics STM32)  --->
  ### STMicroelectronics STM32
  ### Processor model (STM32F407)  --->
  ### Bootloader offset (48 KiB bootloader (MKS Robin Nano V3))  --->
  ### Clock Reference (8 MHz crystal)  ---> 
  ### Communication interface (USB (on PA11/PA12))  --->

<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/imgaes/robin%20nanoV3%20Klipper.PNG" />
- Press "Q" to exit, and then "Y" to save then run make to flashing image for your printer motherboard.


```
make
```

If you have a problem flashing your firmware enter this command :

```
make clean
git pull
make
```

- Open WinSCP App "https://winscp.net/eng/download.php"then connect with your raspberry pi Ip and navigate to:
- /home/pi/klipper/out
<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/imgaes/winscp.PNG" />

  - copy and paste "klipper.bin"to your desktop and change the name to "Robin_nano_v3.bin"
  - format your SD card (attention it is not the one which is in the raspberry) use 32 bit 4096ko then paste"Robin_nano_v3.bin" in your sd card
  - Put SD card on your printer then turn-on and wait about 30s.
  - connect your raspberry with your printer using USB cable
  - Comeback to your ssh session then type:

```
ls /dev/serial/by-id/*
```

It should report something similar to the following:


```
/dev/serial/by-id/usb-Klipper_stm32f407xx_4D0052000950325436303720-if00
```
   - Each printer have a unique serial
   - Copy your serial then paste it in printer.cfg 
<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/imgaes/mcu.PNG" />

```
[mcu]
serial: /dev/serial/by-id/usb-Klipper_stm32f407xx_4D0052000950325436303720-if00#Configuration of the primary micro-controller Use ls /dev/serial/by-id/*
baud: 250000
restart_method: command
```

- If everything is correct now you can connect your MCU with your printer.

### Calibration

## 1 - Z-Offset "You can do this with fluidd or mainsaill terminal or via klipper screen"
   - Home 
   - Install Prob
   - on your terminal type `PROBE_CALIBRATE` printer probe 5 times then stop at 20 mm of the bed at this moment remove your probe
   - paper test: It involves placing a regular piece of "copy machine paper between the printer's bed and nozzle then inspect the printer's nozzle and bed
   - Use the TESTZ command to request the nozzle to move closer to the paper. For example, `TESTZ z=-1` to go down `TESTZ z=1`   to go up it is also possible to use `TESTZ Z=+` or `TESTZ Z=-` to "bisect" the last position that is to move to a position halfway between two positions
   - when you got the right z-offset, run `ACCEPT` then `SAVE_CONFIG` "or ABORT command  To stop the calibration procedure".
## 2 - DELTA_CALIBRATE  
  - Home
  - prob mast be plugin
  - Run `DELTA_CALIBRATE` wait until finish then `SAVE_CONFIG`
## 3 - BED_MESH_CALIBRATE
  - Home
  - prob mast be plugin
  - Run `BED_MESH_CALIBRATE` wait until finish then `SAVE_CONFIG`
## 4 - Print 1st layer test use"1st_layer_test0.2.gcode" [ ![Download](https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/STL/1st_layer_test0.2.gcode) ] 
  - Adjust your z-offset while printing 
  - When you finish running `Z_OFFSET_APPLY_PROBE` to save the new z-offset. `SAVE_CONFIG`






