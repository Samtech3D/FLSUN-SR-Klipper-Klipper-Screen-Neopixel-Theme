If you like my Work Buy Me a Coffee! 🌹❤😍💙

[ ![Download](https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/imgaes/Paypal.png) ](https://www.paypal.me/samiartmedia)

**This repository contains settings for Klipper-Klipper screen and neopixel For FLSUN Super Racer with the necessary scripts and macros using robin nano V3 Motherboard "**<br /><br />

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
- This install based on MKS Robin Nano V3.0 with tmc2209 stepper drivers and Raspberry Pi 7 Touchscreen Display  "Work In Progress For other Boards"
- Raspberry Pi 7 Touchscreen Display https://klipperscreen.readthedocs.io/en/latest/Hardware/
- ADXL 345 for input shaper test (optional) https://www.klipper3d.org/Measuring_Resonances.html

## Installation
- Prepping your OS image (i'm using raspbian buster version to use omx-player driver on startup to run my splash  screen )
- Find Rapberry pi Ip (you can use fing app on your phone)
- Easy way to install klipper with Kiahu https://github.com/th33xitus/kiauh
    - you need to install git first so run on ssh `sudo apt-get install git -y`.\
    - After git is installed, use the following commands in the given order to download and execute the script:

```
cd ~

git clone https://github.com/th33xitus/kiauh.git

./kiauh/kiauh.sh

```
### install in order with kiauh script
  #### 1.Klipper
  #### 2.Moonraker
  #### 3.Fluidd or Mansail Or Both using different port for the 2nd install exp:81(I add theme For Mainsail )
  #### 4.Klipper Screen


- On your browser tape your rpi Ip to open fluidd interface then import:
    - .theme (folder)
    - printer.cfg
    - klipperScreen.conf
    - macros.cfg
    - led_progress.cfg (if you have neopixel installed) 
<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/imgaes/Import_files.jpg" />   

### Build firmware and flash your modherboard
- Open ssh to compile the micro-controller code, start by running these commands  

```
cd ~/klipper/
make menuconfig

```


### [*] Enable extra low-level configuration options
### Micro-controller Architecture (STMicroelectronics STM32)  --->
  ### STMicroelectronics STM32
  ### Processor model (STM32F103)  --->
  ### Bootloader offset (48KiB bootloader (MKS Robin Nano V3))  --->
  ### Clock Reference (8 MHz crystal)  ---> 
  ### Communication interface (USB (on PA11/PA12))  --->

<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/imgaes/robin%20nanoV3%20Klipper.PNG" />
- Press "Q" to exit, and then "Y" to save then run make to flashing image for your printer motherboard


```
make

```
If you have problem to flash you firmware enter this command :

```
make clean
git pull
make

```

- Open WinSCP App "https://winscp.net/eng/download.php"then connect with your rapberry pi Ip and navigate to:
- /home/pi/klipper/out
<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/imgaes/winscp.PNG" />

  - /home/pi/klipper/out
  - copy and paste "klipper.bin"to your desktop and change the name to "Robin_nano_v3.bin"
  - format your SD card (Attention it is not the one which is in the raceberry) use 32bit 4096ko then paste"Robin_nano_v3.bin" 
  - Put sd card on your printer then turn-on and wait about 30s
  - connect your raspberry with your printer using usb cable

- Comback to ssh then tape :

```
ls /dev/serial/by-id/*

```

It should report something similar to the following:


```

/dev/serial/by-id/usb-Klipper_stm32f407xx_4D0052000950325436303720-if00

```
   - Each printer have  unique serial

- Copy your serial then past'it in printer.cfg 
<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/imgaes/mcu.PNG" />


```
[mcu]
serial: /dev/serial/by-id/usb-Klipper_stm32f407xx_4D0052000950325436303720-if00#Configuration of the primary micro-controller Use ls /dev/serial/by-id/*
baud: 250000
restart_method: command
```

- If everythings is correct now to can connect your Mcu with you printer






