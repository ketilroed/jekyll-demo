---
layout: page
title: Test
permalink: /test/
---

The objective of this assignment is to:
- Get familiar with the Quartus Prime design tool,
- write your first VHDL design,
- and program the FPGA.

One of the most simple form of FPGA design that can be made is to make an internal connection between two pins of the FPGA. The goal of this assignment is to use the 10 slide switches on the DE10-Lite board to turn on and off the red Light Emitting Diodes (LED).

More information about the DE1-SoC board can be found on the [DE10-Lite webpage] and in the [DE10-Lite User Manual].

For more information about Intel FPGAs and the Quartus Prime design software visit the [Intel documentation page] and [Quartus Prime user guide.]

To keep track of the work load please indicate the number of hours you have worked on this lab. assignment.


## Create the Quartus Prime Project

*   Create a new directory called *lab1* in your local Git repository directory
*   Start the Quartus Prime program.
*   Create a new Quartus project (from File -> New Project Wizard) using the following settings:
    *   Project directory: *lab1 in your git repository directory*
    *   Project name: *lab1*
    *   Name of top-level design entity: *lab1*
    *   Project Type: *Empty project*
    *   Device Family: *MAX 10*
    *   Device name: *10M50DAF484C7G*
    *   Add files: *Do not add any files at this stage*
    *   Under EDA Tool settings choose: *ModelSim-Altera and VHDL for Simulation.*

## Write your first VHDL design

*   Create a new directory called *src* inside your *lab1* directory
*   Open your favourite text editor (e.g. Notepad++), enter the VHDL description as shown below and add the appropriate statements for declaring the input and output ports and for controlling the LEDs using the switches. Save the file in the directory *src* and give it the name *lab1.vhd*
*   Add the new VHDL file to the project as shown below. (Project -> Add/Remove Files in Project). Locate the file, press *Add* and then press *OK*.

```vhdl
    library IEEE;
    use IEEE.std_logic_1164.all;

    entity lab1 is
      port (
          -- YOUR TASK:
          -- Insert the needed port declarations
          -- Use identifier names led and sw when naming the ports.
          );
        end entity lab1;

    architecture top_level of lab1 is

    begin

      -- YOUR TASK:
      -- Insert statement to assign all switches to the LEDs.

    end architecture top_level;
```

## Hardware constraints

The slides switches and the LEDs are hardwired to specific FPGA pins. It is therefore necessary to inform Quartus about which pins to use, that is, the address of the relevant pins.

The correct pin assignments can be found in the [DE10-Lite User Manual]. For example, the manual specifies that sw(0) is connected to the FPGA PIN C10 and led(0) is connected to PIN A8. Each pin can be assigned manually through the Quartus Prime pin assignment manager, however, a more elegant and time saving approach is to make the pin assignment using a Tcl scripting file.

* Create a new file called *de10-lite-pinning.tcl* in your *lab1* directory. Open the new Tcl-file in your favourite text editor (e.g. Notepad++) and enter the required pinning constraints. Examples for the first LED and SW pin assignment is shown below.

```tcl
        #Toggle switches
        set_location_assignment PIN_C10 -to sw[0]
        # YOUR TASK: Enter the pin assignments for the remaining SW pins

        #LED outputs
        set_location_assignment PIN_A8 -to led[0]
        # YOUR TASK: Enter the pin assignments for the remaining LED pins

        #To avoid that the FPGA is driving an unintended value on pins that are not in use:
        set_global_assignment -name RESERVE_ALL_UNUSED_PINS "AS INPUT TRI-STATED"
```

*   Add the new Tcl-file to the project (Project -> Add/Remove Files in Project).
*   Run the tcl script to activate the pin assignments (Tools -> Tcl Scripts). The Tcl-script should be visible under the project folder. Mark the file and click RUN. Verify that the correct assignments have been performed by opening the pin assignment editor (Assignments -> Assignment editor). You should see a similar list as shown below.
*   Whenever you make changes to the pinning file or include it in a new project, you will ned to (re-)run the Tcl-script.

## Compile the project:

*   Compile the project: (Processing -> Start Compilation)
*   During compilation of the project you will see some warnings related to missing constraint information, e.g. :
    *   Some pins have incomplete I/O assignments
    *   No clocks defined in design
    *   Synopsys Design Constraints File file not found.These warnings can for the moment be ignored.
*   If the compilation of the project was successful, Quartus has generated and SRAM object file (.sof) either in the project directory itself or in the directory output-files under the project directory. This is the programming file that will be downloaded to the FPGA.

## Program the FPGA:

* Make sure to connect the USB cable to the USB connector on the DE10-Lite.
*   Open the Quartus programmer (Tools -> Programmer).
*   If the field next to the button ”Hardware Setup” shows ”No Hardware”, make sure the USB cable is connect to both the PC and the DE1-SoC board, and that the power is turned on. Press the ”Hardware Setup and choose ”USB-Blaster \[USB-0\]” under the ”Currently selected hardware”. Press Close.
*   Double click on the *File* field of the listed device and select the correct programming file. Tick the box for ”Programming/Configure”.
*   To program the FPGA press Start.
*   Verify that your design works by changing the position of the toggle switches on the DE10-Lite board.


## Update the git repository:

When you have completed this part of the lab make sure to update the git repository:
```bash
    git add -A
    git commit -am "First FPGA project completed"
    git push origin master
    git tag -a first-fpga-project -m "First FPGA project completed"
    git push origin first-fpga-project
```
To check which files have been added to the git repository write the command
```bash
    git ls-files
```

You should see the following result:
```bash
    .gitignore
    .gitignore
    README.md
    lab1/de10-lite-pinning.tcl
    lab1/src/lab1.vhd
    lab1/lab1.qpf
    lab1/lab1.qsf
```

You have now created and programmed your first FPGA design.


[DE10-Lite webpage]: http://de10-lite.terasic.com
[DE10-Lite User Manual]: https://www.terasic.com.tw/cgi-bin/page/archive_download.pl?Language=English&No=1021&FID=a13a2782811152b477e60203d34b1baa
[Intel documentation page]: https://www.intel.com/content/www/us/en/programmable/documentation/lit-index.html
[Quartus Prime user guide.]: https://www.intel.com/content/www/us/en/programmable/documentation/yoq1529444104707.html

[DE10-Lite User Manual]: https://www.terasic.com.tw/cgi-bin/page/archive_download.pl?Language=English&No=1021&FID=a13a2782811152b477e60203d34b1baa
