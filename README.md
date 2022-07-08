If you like my Work Buy Me a Coffee! 🌹❤😍💙

[ ![Download](https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/images/Paypal.png) ](https://www.paypal.me/samiartmedia)

**This repository contains settings for Klipper, KlipperScreen and Neopixel for FLSUN Super Racer with necessary scripts and macros using MKS Robin Nano V3.0 motherboard.**<br /><br />

<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/images/repository-flsun.png" />

## References
<img align="center" width=100 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/images/klipper.png" />

- https://www.klipper3d.org/
- https://github.com/th33xitus/kiauh
- https://github.com/jordanruthe/KlipperScreen
- https://www.klipper3d.org/Pressure_Advance.html
- https://www.klipper3d.org/Resonance_Compensation.html
- https://www.klipper3d.org/Measuring_Resonances.html
- https://github.com/digitalninja-ro/klipper-neopixel

## Hardware
- The recommended hardware is a Raspberry Pi 2, Raspberry Pi 3, or Raspberry Pi 4 https://www.klipper3d.org/FAQ.html
- This installation is based on MKS Robin Nano V3.0 with TMC2209 stepper drivers and Raspberry Pi4 Touchscreen Display  "Work in Progress for other motheroards."
- Raspberry Pi 4 Touchscreen Display https://klipperscreen.readthedocs.io/en/latest/Hardware/
- ADXL 345 for input shaper test (optional) https://www.klipper3d.org/Measuring_Resonances.html

## Installation
- Prepare your OS image (I'm using Raspberry Pi OS Legacy (Buster version) to use omx-player to run my splash screen on startup). https://www.raspberrypi.com/software/ , or you can use Raspberry PI Imager
<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/images/buster-version.jpg" />
 
- Find the Raspberry Pi IP (you can use fing app on your phone).
- Easy way to install Klipper with Kiauh: https://github.com/th33xitus/kiauh
    - You need to install git first, on SSH run this command: `sudo apt-get install git -y`.
    - After git is installed, use the following commands in the given order to download and execute the script:

```
cd ~

git clone https://github.com/th33xitus/kiauh.git

./kiauh/kiauh.sh
```
### Install with Kiauh script in the following order:
  #### 1.Klipper
  #### 2.Moonraker
  #### 3.Fluidd or Mainsail or both using a different port for the 2nd install, example: Port 81 (I added themes for Mainsail)
  #### 4.KlipperScreen


- On your browser type your Raspberry Pi IP to open Fluidd interface then import:
    - .theme (folder)
    - printer.cfg
    - klipperScreen.conf
    - macros.cfg
    - led_progress.cfg (if you have neopixel installed) 
<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/images/Import_files.jpg" />   

### Build firmware and flash your motherboard
- Open SSH to compile the microcontroller code, start by running these commands: 

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

<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/images/robin%20nanoV3%20Klipper.PNG" />
- Press "Q" to exit, and then "Y" to save and run make command to flashing image for your printer motherboard.


```
make
```

If you have a problem to flash your firmware enter this command:

```
make clean
git pull
make
```

- Open WinSCP App (https://winscp.net/eng/download.php), connect with your Raspberry Pi IP and navigate to:
- /home/pi/klipper/out
<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/images/winscp.PNG" />

  - Copy and paste "klipper.bin" firmware to your desktop and rename the file to "Robin_nano_v3.bin".
  - Format your microSD card (be careful, do not use the one present in the Raspberry Pi) in FAT32 with allocation unit size 4096 (max capacity 32GB) then paste "Robin_nano_v3.bin" file in the root of your  microSD card.
  - With printer off, insert the microSD card into the dedicated port on the motherboard and turn on the printer.
  - Flash procedure starts (without displaying anything on the screen) and lasts a few seconds.
  - Connect your Raspberry Pi to your printer using USB cable.
  - Comeback to your SSH session then type:

```
ls /dev/serial/by-id/*
```

It should report something similar to the following:


```
/dev/serial/by-id/usb-Klipper_stm32f407xx_4D0052000950325436303720-if00
```
   - Each printer have a unique serial
   - Copy your serial then paste it in printer.cfg
<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/images/mcu.PNG" />

```
[mcu]
serial: /dev/serial/by-id/usb-Klipper_stm32f407xx_4D0052000950325436303720-if00#Configuration of the primary micro-controller Use ls /dev/serial/by-id/*
baud: 250000
restart_method: command
```

- If everything is correct, you can connect your Raspberry Pi with your printer.

### Calibration

## 1 - Z-Offset "You can do this with Fluidd, Mainsaill terminal or via KlipperScreen"
   - Home 
   - Make sure to connect bed level probe before to start the following command.
   - On your terminal run `PROBE_CALIBRATE` command. Printer will probe 5 times then stop at 20 mm of the bed.
   - Remove bed level probe.
   - Paper test: This involves placing a piece of plain paper between the bed and the nozzle so that the sheet of paper slightly scrapes the nozzle.
   - Use the TESTZ command to request the nozzle to move closer to the paper. For example, `TESTZ z=-1` to go down, `TESTZ z=1` to go up. It's also possible to use `TESTZ Z=+` or `TESTZ Z=-` to "bisect" the last position that is to move to a position halfway between two positions.
   - When you got the right Z-Offset, run `ACCEPT` then `SAVE_CONFIG` commands or `ABORT` command to stop the calibration procedure.
## 2 - DELTA_CALIBRATE  
  - Home.
  - Make sure to connect bed level probe before to start the following command.
  - Run `DELTA_CALIBRATE` command and wait until finish then `SAVE_CONFIG`.
## 3 - BED_MESH_CALIBRATE
  - Home
  - Make sure to connect bed level probe before to start the following command.
  - Run `BED_MESH_CALIBRATE` command and wait until finish then `SAVE_CONFIG`.
## 4 - Print 1st layer test using "1st_layer_test0.2.gcode" [ ![Download](https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/STL/1st_layer_test0.2.gcode) ] 
  - Adjust your Z-Offset while printing.
  - When it's done, run `Z_OFFSET_APPLY_PROBE` then `SAVE_CONFIG` commands to save the new Z-Offset.
  - If you still have problem with your Z-Offset, after G28 line add your Z-Offset value to your Start-Gcode, Ex:`SET_GCODE_OFFSET Z=0.35`.
  <img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/images/Z-offset%20update.PNG" />  

### Install Samtech3d Themes For Klipper Screen
  - Navigate to `/home/pi/KlipperScreen/styles` using WinSCP App then drag and drop "Samtech-red & Samtech-blue" folders.
  - In KlipperScreen Go to Configuration / Settings and scroll down to Icon Theme "Samtech-red & Samtech-blue".

<img align="center" width=600 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/images/config.PNG" />
<img align="center" width=600 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/images/screen parametres.PNG" />
<img align="center" width=600 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/images/icon theme.PNG" />

### Neopixel Installation

<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/images/MKS_Robin_Nano_V3_led.jpg" />

Or 

<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/images/MKS_Robin_Nano_V3_led2.jpg" />

  - Add `led_progress.cfg` then included in your printer.cfg `[include led_progress.cfg]`

  - Add this lines to your printer.cfg

  ```
  [neopixel neo]
  pin: PB2
  chain_count:15
  color_order: GRB
  initial_RED: 0.0
  initial_GREEN: 0.0
  initial_BLUE: 0.0
  initial_WHITE: 0.0

  ```
  - Add this lines to your KlipperScreen.conf

  ```
#####################################################
########## My Klipper Screen Neopixel  ##############
#####################################################

[menu __main Lights] #Lights
name: Lights
icon: light

[menu __main Lights Lights_On]
name: Lights On
icon: light
method: printer.gcode.script
params: {"script":"neopixel_on"}
[menu __main Lights Lights_Off]
name: Lights Off
icon: light
method: printer.gcode.script
params: {"script":"neopixel_off"}
[menu __main Lights blue]
name: blue
icon: lightB
method: printer.gcode.script
params: {"script":"blue_on"}

[menu __main Lights red]
name: red
icon: lightR
method: printer.gcode.script
params: {"script":"red_on"}
[menu __main Lights green]
name: green
icon: lightG
method: printer.gcode.script
params: {"script":"green_on"}     

[menu __main Lights extruder_temp]
name: Glow
icon: extruder
method: printer.gcode.script
params: {"script":"extrudertemp"}  
[menu __main Lights extruder_progress]
name: Progression
icon: extruder
method: printer.gcode.script
params: {"script":"extruderprogress"}  

[menu __main Lights bedtemp]
name: Glow
icon: bed
method: printer.gcode.script
params: {"script":"bedtemp"}  
[menu __main Lights bedprogress]
name: Progression
icon: bed
method: printer.gcode.script
params: {"script":"bedprogress"}  
[menu __main Lights percentprogress]
name: Progression
icon: percentage
method: printer.gcode.script
params: {"script":"percentprogress"}  
[menu __main Lights percentglow]
name: Glow
icon: percentage
method: printer.gcode.script
params: {"script":"percentglow"}  
[menu __main Lights speedprogress]
name: Progression
icon: speed+
method: printer.gcode.script
params: {"script":"speedprogress"} 
```
 - Add this lines to your macros.cfg

 ```
#############################################
######### NeoPixel By Samtech 3D#############
#############################################
[gcode_macro neopixel_on]
gcode:
          SET_LED LED=neo RED=1 GREEN=1 BLUE=1
[gcode_macro neopixel_off]
gcode:
          NEOPIXEL_DISPLAY LED="neo" TYPE=clear
          
[gcode_macro blue_on]
gcode:
          SET_LED LED=neo RED=0 GREEN=0 BLUE=1
[gcode_macro red_on]
gcode:
          SET_LED LED=neo RED=1 GREEN=0 BLUE=0   
[gcode_macro green_on]
gcode:
          SET_LED LED=neo RED=00 GREEN=1 BLUE=0  
[gcode_macro extrudertemp]
gcode:
          neopixel_display LED=neo TYPE=extruder_temp MODE=glow
[gcode_macro extruderprogress]
gcode:
          neopixel_display LED=neo TYPE=extruder_temp MODE=progress     

[gcode_macro bedtemp]
gcode:
          neopixel_display LED=neo TYPE=bed_temp MODE=glow  
[gcode_macro bedprogress]
gcode:
          neopixel_display LED=neo TYPE=bed_temp MODE=progress          
[gcode_macro percentprogress]
gcode:
          neopixel_display LED=neo TYPE=print_percent  MODE=progress        
[gcode_macro percentglow]
gcode:
          neopixel_display LED=neo TYPE=print_percent  MODE=glow   
[gcode_macro speedprogress]
gcode:
          neopixel_display LED=neo TYPE=printer_speed    MODE=progress  

[gcode_macro speedglow]
gcode:
          neopixel_display LED=neo TYPE=printer_speed    MODE=glow 

```
<img align="center" width=800 src="https://github.com/Samtech3D/FLSUN-SR-Klipper-Klipper-Screen-Neopixel-Theme/blob/main/images/neopixel2.jpg" />
  

