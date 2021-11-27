# JG-Maker-Artist-D-Pro-levelling-with-BLTouch-AUTO_BED_LEVELING_LINEAR-

JG Maker Artist-D Pro levelling with BLTouch

This project a simple modifying the original Marlin code.

https://github.com/JGMaker3dofficial/ArtistD-Pro.git

The features:

Switched off the ZMax pin and use for BLTouch
Setting the necessary changes
Extra modifying for the different filaments
BLTouch holder:
https://www.thingiverse.com/thing:4922096

ZMax pin:
You need to use one of the pin on the motherboard (MKS Robin Pro) for the BLTouch without the modify the original cable joints. On the board you have 6 dedicated 3 pin port for the endstops. This 3D printer use onli 3 ports (X-Y-Z min port) for the G28 base levelling. In the original Marlin code declared all joints with name. First you need to choice a port want to use. I used the Z max.

File: ........\ArtistD-Pro-master\Marlin\src\pins\stm32f1\pins_MKS_ROBIN_PRO.h

Switch OFF the pin declaration with two slash "//" in row of 78

//#define Z_MAX_PIN PC4

You have now an undeclared pin (PC4).

File:...........\ArtistD-Pro-master\Marlin\Configuration.h

Switch OFF the plug declaration with two slash "//" in row of 651 (in the original code is switched off, but need to check)

//#define USE_ZMAX_PLUG

Switch OFF the "Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN" with two slash "//" in row of 862

//#define Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN

Switch ON the Z_MIN_PROBE_PIN (deleted the two slash from the front) and settled the pin (with the free PC4 port) in row of 880

#define Z_MIN_PROBE_PIN PC4 // Pin 32 is the RAMPS default

Switch OFF the PROBE_MANUALLY with two slash "//" in row of 894 (you can't use BL-Touch and PROBE_MANUALLY together, but in the menu it stays alive)

//#define PROBE_MANUALLY

Switch ON the BLTOUCH (deleted the two slash from the front) in row of 918

#define BLTOUCH

Set the NOZZLE_TO_PROBE_OFFSET in row of 990

#define NOZZLE_TO_PROBE_OFFSET { 25, -35, -1 }

Switch ON the AUTO_BED_LEVELING_LINEAR (deleted the two slash from the front) in row of 1238

#define AUTO_BED_LEVELING_LINEAR

Set the GRID_MAX_POINTS_X as more points (because this bed is big) in row of 1286

#define GRID_MAX_POINTS_X 5 //original was 3

Now, is the BLTouch modification in the code is finished.

The holder setting:

When you install the holder, set -1mm differential between the nozzle and the BLTouch pulled in pin. The final setting is easy, just set the bed first with limit switches G28 and run the G29 auto bed levelling command. Re check the Z position of the nozzle. If is not correct you can to modify the offset with M851 command:

M851 Z-1,6 (it's only sample value)

In the future if you change the nozzle you need to re setting this value, but enogh modify the Z offset not necessary recalibration the bed wit screws. All G code need to including after the G28 command a G29 as well for the use BLTouch. You can to write in all slicer this.

I use many other filaments as well, so the original temperature setting is not enough. You can to setting a lowest minimum melt point or setting a higher melt point for your special filaments. I setted 130°C minimum and 275°C maximum temperature for all filaments.

File:...........\ArtistD-Pro-master\Marlin\Configuration.h

Set the maximum temperature in row of 484-485

#define HEATER_0_MAXTEMP 275 //original 250
#define HEATER_1_MAXTEMP 275 // original 250

Set the minimum temperature i row of 592

#define EXTRUDE_MINTEMP 130 //original 170
