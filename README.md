# T35_FPGA_MODULE
<b>[S100Computers](http://s100computers.com/ "S100Computers") T35 FPGA Module Design</b>

<b>NOTE: See [Important Wire Mod needed for Version 1b](#important-wire-mod-needed-for-version-1b) details at the end of page!</b>

<b>Table of Contents:</b>
- [Project Overview](#project-overview)
  - [T35 FPGA Module Image](#t35-fpga-module-image)
  - [T35 FPGA Module Features DIagram](#t35-fpga-module-features-diagram)
- [Toolchain](#toolchain)
- [Project Organization](#project-organization)
- [JTAG Programming Adapters](#jtag-programming-adapters)
  - [USB Adapter Supply Noise Fix](#usb-adapter-supply-noise-fix)
  - [Olimex JTAG Adapter Wire Mods](#olimex-jtag-adapter-wire-mods)
  - [FTDI USB Adapter to Olimex JTAG Header Connections](#ftdi-usb-adapter-to-olimex-jtag-header-connections)
- [Installing the FTDI Zadig USB Drivers](#installing-the-ftdi-zadig-usb-drivers)
  - [Installing the Linux USB Driver](#installing-the-linux-usb-driver)
  - [Installing the Windows USB Driver](#installing-the-windows-usb-driver)
- [Quick Start](#quick-start)
  - [Quick Start Prep](#quick-start-prep)
  - [JTAG Programming](#jtag-programming)
  - [SPI Flash Programming](#spi-flash-programming)
- [Design Files](#design-files)
  - [Schematic](#schematic)
  - [Pin Assignment Spreadsheet](#pin-assignment-spreadsheet)
- [Important Wire Mod needed for Version 1b](#important-wire-mod-needed-for-version-1b)
- [Jumper Assignments](#jumper-assignments)
  - [Jumper Intended Use](#jumper-intended-use)
  
## Project Overview ##

This is a custom T35 FPGA project module for [S100Computers](http://s100computers.com/ "S100 Computers").  This module was designed to augment the EP4CE10 based FPGA originally in use on the various FPGA S-100 Boards in the S100Computers inventory.

The module pictured below is in the same basic footprint as the original, but with added pins (shown in white-filled rectanges surroundng the new pins).

Added features in comparison to the original EP4CE10 module include:
  - 50 Additional GPIO Pins for a total of 214 I/Os (Module is shipped with pins unmounted for added flexibility)
  - 3x the Logic Elements (31,680 vs 10,320)
  - 3.5x the Embedded FPGA RAM (180K vs 51K)
  - Adds in LVDS Capability (up to 800Mbps)
  - Adds 128MByte DDR3 SDRAM
  - Up to four configuration images (jumper selectable)
  - Seven Segment LED Display for custom status or diagnostic output

### T35 FPGA Module Image ###

![picture alt](Docs/T35_FPGA_Module.jpg "T35_FPGA_Module")

### T35 FPGA Module Features Diagram ###

![picture alt](Docs/T35_FPGA_Module_Diagram.jpg "T35_FPGA_Module_Diagram")

## Toolchain ##

The FPGA Tool-chain used for this project is the [EfinityIDE](https://efinixinc.com/products-efinity.html "Efinity IDE Toolchain") from [Efinix Inc](https://efinixinc.com/ "Efinix").

The Efinity IDE Toolchain is currently available with the purchase of an approved Efinity Development Board (see Efinix website for more details).

## Project Organization ##

Projects for this board are generally stored on the root of the host system hard drive in the \S100Projects\ directory (e.g. - C:\S100Projects\).  The user does not need to replicate this directory structure, but there may be some project specific links that will need to be adjusted to move a project to a different directory organization.

## JTAG Programming Adapters ##

The T35 FPGA Module uses a mini 5x2 JTAG Header with 0.05" (1.27mm) pin spacing.  Mating cables and a JTAG Adapter PCB are available from Olimex that can be adapted for use in programming the T35 FPGA Module.  The parts needed to implement this JTAG Programmer are available from [Digikey](https://www.digikey.com) and [Mouser](https://www.mouser.com):

  | <b>Digikey Part No</b> | <b>Description</b>         | <b>Manf</b>    | <b>Manf Part No</b>   |
  |-----------------|---------------------|---------|----------------|
  | 1188-1016-ND    | JTAG Adapter        | Olimex  | ARM-JTAG-20-10 |
  | 769-1106        | USB Adapter         | FTDI    | C232HM-DDHSL-0 |
  | S2021EC-10-ND   | Male to Male Header | Sullins | PRPC010DABN-RC |


  | <b>Mouser Part No</b>     | <b>Description</b>         | <b>Manf</b>     | <b>Manf Part No</b>   |
  |--------------------|---------------------|----------|----------------|
  | 909-ARM-JTAG-20-10 | JTAG Adapter        | Olimex   | ARM-JTAG-20-10 |
  | 895-C232HM-DDHSL-0 | USB Adapter         | FTDI     | C232HM-DDHSL-0 |
  | 649-77313-198-20LF | Male to Male Header | Amphenol | 77313-198-20LF |

### USB Adapter Supply Noise Fix ###

The Olimex JTAG Adapter will need to be modified to fix a supply noise issue on the FTDI USB Adapter (where up to 700mV of noise is present on the FTDI 3.3V supply pin), and also to allow for the use of the Olimex JTAG Adapter with the T35 JTAG pinout.

To fix the supply noise issue, a 100uF electrolytic cap must be added between pins 2 and 6 on the 20-pin female header:

![picture alt](Docs/Olimex_JTAG_100uf_Mod.jpg "Olimex JTAG Supply Noise Fix")

### Olimex JTAG Adapter Wire Mods ###

R2 on the Olimex JTAG Adapter (which is actually implemented as a trace/short) needs to be removed to avoid interfering with the Chip Select signal  of the configuration flash on the T35 FPGA Module (pin 9 on the 10-pin Olimex header).  See the [Olimex JTAG Schematic](https://www.olimex.com/Products/ARM/JTAG/ARM-JTAG-20-10/resources/ARM-JTAG-20-10_latest-schematic.pdf "Olimex JTAG Adapter").

![picture alt](Docs/Olimex_JTAG_Wire_Mod.jpg "Olimex Adapter Wire Mods")

### FTDI USB Adapter to Olimex JTAG Header Connections ###

To connect the USB Adapter to the Olimex JTAG, first plug in the 20-pin male to male header into the Olimex Female 20-pin header.  Next plug the USB Adapter pin header cables into the following pins.  When done you should have a color sequence of wires as shown above.

  | JTAG Adapter Pin No | Color  | Signal Name     | T35 JTAG Pin No |
  |---------------------|--------|-----------------|-----------------|
  | Pin 2               | RED    | +3.3V Supply    | Pin 1           |
  | Pin 5               | YELLOW | JTAG TDI        | Pin 8           |
  | Pin 6               | BLACK  | GROUND          | Pins 3 and 5    |
  | Pin 7               | BROWN  | JTAG TMS        | Pin 2           |
  | Pin 9               | ORANGE | JTAG TCK        | Pin 4           |
  | Pin 11 (Optional)   | PURPLE | QSPI_CSN        | Pin 9           |
  | Pin 13              | GREEN  | JTAG TDO        | Pin 6           |
  | Pin 15              | GREY   | CRESET          | Pin 10          |

## Installing the FTDI Zadig USB Drivers ##

### Installing the Linux USB Driver: ###

(from Efinix AN006)
The following instructions explain how to install a USB driver for Linux operating systems.
  1. Disconnect your board from your computer.
  2. In a terminal, use these commands:
```
    > sudo <installation directory>/bin/install_usb_driver.sh
    > sudo udevadm control --reload-rules
```
<b>Note:</b> If your USB JTAG Adapter was connected to your computer before you executed these commands, you need to disconnect and re-connect it.

### Installing the Windows USB Driver ###
  1. Download <b>Zadig</b> from [zadig.akeo.ie](https://zadig.akeo.ie/ "zadug.akeo.ie")
  2. Copy the <b>Zadig</b> application from the 'Downloads' to a working directory.
  3. Right click the <b>Zadig</b> application and select "Run as Administrator".
  
  <b>NOTE:Zadig can have problems identifying the C232HM-DDHSL-0 when other FTDI based USB devices are plugged in.  Efinix recommends unplugging other FTDI devices while configuring the C232HM-DDHSL-0, after which you should be able to plug the other devices back in.</b>

![picture alt](Docs/Zadig-Run-Admin.jpg "Zadig_Run_as_Administrator")

  4. Choose <b>Options->List All Devices</b> and turn off <b>Options->Ignore Hubs or Composite Parents</b>.

![picture alt](Docs/Zadig-Options-Done.jpg "Zadig_Options_Done")

  5. Select <b>C232HM-DDHSL-0</b> from the drop down list.

![picture alt](Docs/Zadig-C232HM-Select.jpg "Zadig_Select_C232HM_Select")

  6. Select <b>libusbK</b> from the drop down list.

![picture alt](Docs/Zadig-Install-Settings.jpg "Zadig_Install_Settings")

  7. Click <b>Reinstall Driver</b> or <b>Replace Driver</b>.

Proceed to the <b>Quick Start</b> section below to verify correct operation of the JTAG Programmer.
  
# Quick Start #
(After toolset and driver install).

## Quick Start Prep ##

  1. Create a new directory called <i>T35seg7</i> under the <i>C:/S100Projects/</i> directory (e.g. - <i>C:/S100Projects/T35seg7/</i> ).
  2. Download (Clone) the [T35seg7 Test Project](https://github.com/s100projects/T35seg7 "T35seg7 Test Project") to the <i>C:/S100Projects/T35seg7/</i> directory.
  This is a good initial project to use on existing [S100Computers](http://s100computers.com/ "S100Computers") S-100 FPGA Boards as it does not drive any of the legacy WaveShare GPIOs; it only drives the GPIOs associated with the seven segment display (GPIOL_72 to GPIOL_81).  This avoids any signal conflicts with exisiting designs.
  The purpose of this FPGA project is to count and display the numbers from 0 to 'F' (hexadecimal) on the seven segment display on one second boundaries.
  
  3. Start the Efinity toolchain and load the <i>C:/S100Projects/T35seg7.xml</i> project.
  4. Compile the T35seg7 project by clicking on the ![picture alt](Docs/EFX_Compile_Btn.jpg "Compile_Button") icon. (Wait for completion)
  5. Prepare for SPI FLash Programming by downloading and copying the [jtag_spi_flash_loader.bit](https://github.com/s100projects/T35_Flash_Programmer "T35_Flash_Programmer") file into the <i>/outflow/</i> directory.  Doing this will allow the Efinity Programmer to find the JTAG Bridge bitstream file automatically when performing SPI Programming.
  6. Connect the JTAG Adapter to the T35 JTAG port and plug the cable into a USB port.  <b>(NOTE: No other connections are necessary as the T35 FPGA Module can derive its power from either the module interface pins or the JTAG Cable).</b>
  Be careful to align the red stripe of the JTAG cable to the left and make sure that the JTAG pins are properly aligned <b>(it is possible to plug this connector in being one row off).</b>  When done the JTAG cable should hang straight down to the Olimex JTAG Adapter without looping (if looped, you may have the cable ends reversed).
  
 ![picture alt](Docs/T35seg7_proj.jpg "T35_Seven_Segment_Display_Project")
 
  7. Open the Efinity Programmer by clicking on the ![picture alt](Docs/EFX_Program_Btn.jpg "Prog_Button") icon.

  There are two types of programming that can be done with the T35 FPGA Module:
  - JTAG Programming (this targets the FPGA configuration RAM directly, but does not save it in Flash).
  - SPI Flash Programming (this targets the SPI Configuration Flash for confiuration auto-load on power on).
    
## JTAG Programming ##

  1. Click the "File Select" Button ![picture alt](Docs/File_Select.jpg "File_Select") and select the <i>T35seg7.bit</i> configuration bitstream from the open file dialog box.
  2. Under "Programming Mode" select "JTAG" from the drop-down menu.
  3. Click the "Program Button" ![picture alt](Docs/Prog_Start_Btn.jpg "Prog_Button") to program the T35 via JTAG.

![picture alt](Docs/Programmer-Start.jpg "JTAG_Programming")

Once programmed, you should see the T35 transition into <b>User</b> mode:

![picture alt](Docs/Successful_JTAG_Programming.jpg "Successful_JTAG_Programming")

## SPI Flash Programming ##

  1. Click the "File Select" Button ![picture alt](Docs/File_Select.jpg "File_Select") and select the <i>T35seg7.hex</i> configuration bitstream from the open file dialog box.
  2. Under "Programming Mode" select "SPI Active using JTAG Bridge" from the drop-down menu.
  3. Use the "File Select" button in the "Auto configure JTAG Bridge Image" to select the downloaded <i>jtag_spi_flash_loader.bit</i> in the outflow directory.
  (Efinity should have already recognized the bridge image in the outflow directory, but if it doesn't, make sure to select it before proceeding).
  4. Click the "Program Button" ![picture alt](Docs/Prog_Start_Btn.jpg "Prog_Button") to program the T35 via JTAG.
  The programmer will now erase and then program the T35seg7 bitstream image into the flash.

![picture alt](Docs/SPI_Programming_Settings.jpg "JTAG_Programming")

  After programming, you should see a "Flash verify Successful!" message when done:
  
![picture alt](Docs/Successful_SPI_Programming.jpg "SPI_Programming_Successful")  

<b>NOTE: Since the jtag_spi_flash_loader.bit bitstream is still loaded into the T35's configuration RAM after programming, you will need to press the reset button on the module to have the T35 load the new configuration from the flash.</b>

# Design Files #

## Schematic ##

  [T35 FPGA Module Schematic](Docs/fpga_coret35_v1c_FINAL_with_wire_mod.pdf "T35 FPGA Module Schematic")

## Pin Assignment Spreadsheet ##

  [T35 FPGA Module Pin Assignments](Docs/COREEP4CE10pinsUpdated.xlsx "T35 FPGA Module Schematic")
  (with special thanks to Terry Fox!)

## Jumper Assignments ##

  | <b>Jumper Ref Des</b> | <b>Function</b>                       | <b>Default Setting</b>       |
  |----------------|--------------------------------|-----------------------|
  | <b>J1</b> - OSC DIS   | On Board Oscillator Disable    | <b>Open</b> (Osc Enabled)    |
  | <b>J3</b> - CBSEL1    | Config Binary Select:          | <b>Open</b> (Image 0)        |
  | <b>J4</b> - CBSEL0    | [00, 01, 10, 11]               | <b>Open</b> (Image 0)        |
  | <b>J7</b> - QSPI/SPI  | QSPI vs SPI I/F Config         | <b>Open</b> (QSPI)           |
  | <b>J10</b> - 5V-3V EN | 5V to 3V SMPS Enable           | <b>Open</b> (5V-3V Disabled) |
  | <b>J11</b> - JTAG     | JTAG Connector for Programming |         <b>N/A</b>           |
  
  <b>NOTE: J7 is only used for initial manufacturing board qualification and testing of new Configuration Flash Devices.  Leave OPEN for normal module use.</b>
  
### Jumper Intended Use ###

  - <b>J1</b> -    Allows for disabling the on-board oscillator to allow for the use of an external oscillator on J6 pin 23.
  - <b>J3/J4</b> - Allows for programming and selection (at boot) of multiple configuration binary images.
  - <b>J7</b> -    Used for qualifciation of new configuration flashes (not normally used by users - leave open)
  - <b>J10</b> -   Allows for enabling the on-board 5V to 3.3V DC-DC Converter (SMPS) for hosts that DO NOT SUPPLY 3.3V to the T35 Module.
  - <b>J11</b> -   10 pin 0.050" Mini JTAG Header for programming and configuration.
  
# Important Wire Mod needed for Version 1b #

There is an important single wire modification needed for complete compatibility with the WaveShare pinout due to a missing PCB net on DC1D pin 148 (please see schematic above for details).

![picture alt](Docs/T35_FPGA_v1bc_wire_mod.jpg "T35_FPGA_v1bc_wire_mod")
