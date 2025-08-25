## Members
Corey Ahl, Computer Engineering Student (Sophomore)
corahl@vt.edu

Lauren Castro, Computer Engineering Student (Sophomore)
lacastro@vt.edu

## Mentor
TBD

## Current Status
WAITING FOR APPROVAL

## Presentation
https://docs.google.com/presentation/d/1lxvC3uXCg7kJrOCfoKKMh7Mao0DsRBzygd3GKA6etf8/edit?usp=sharing

## Project Overview

The goal of the project is to create a version of [Nicholas L. Johnson's flip-card](https://github.com/Nicholas-L-Johnson/flip-card), which is a custom PCB in the form of a business card that runs a fluid impact particle simulation based on accelerometer data. This is displayed on a charlieplexed 21x21 LED array.

The working example of Nicholas Johnson's card is shown below:

![project-demonstration](https://github.com/WebTWJ/corahl-project-amp/blob/main/project-demonstration.gif)

We propose one main change to the Nicholas Johnson's project, running the fluid simulation on an FPGA instead of the microprocessor used originally, an RP2350.

### Design Overview
FPGA Programming
- Do this first to get an idea of the Logic Element requirements for the simulation
- Plan on using an Xilinx Arty-A7 development board for initial development

Choosing Materials
- Choosing the FPGA
    - Ideally a low IO count. The minimum for charlieplexing is 22, and the minimum for multiplexing is 42. The amount for the accelerometer (LIS2DH12TR) is 12. The amount the original project uses is 60 (the full amount of the RP2350).
    - Ideally a non BGA package, preferably QFP or TQFP (for easier attachment and lower PCB cost).
    - Enough Logic Elements to run the simulation.
    - Ideally non-volatile.
    - Small enough to leave room for the rest of the circuit.
    - Ideally also compatable with Vivado / Vitis (same as test board).
    - This is the hardest point of planning this project, as finding FPGAs fitting these requirements is difficult.
- Will stick with the same LEDs used for the original project, 0402 LEDs.
- Will probably stick with the same accelerometer as the original project, an LISDH12TR.
- The FPGA will also likely require voltage regulators as most require differing voltages for the different voltage rails and keeping this consistent is key. Some of the rails to consider are core voltage supply, IO voltage, auxiliary voltage, transceiver voltages, memory voltage (though this may differ depending on the specific FPGA).  The two important ones for this project is the difference between the core voltage power supply and what is needed for IO (which will power our LED array).

PCB Design
- After having all the part requirement, can then work on building the PCB design.

A capstone to this project could be converting the FPGA design to an ASIC design, which could then be actually made by attaching it to something like the [TinyTapeout project](https://tinytapeout.com/). This project gives everyone a small section of an ASIC to design however they want, with more space avaliable if paid for, and is then produced and can be bought by people who contributed to the project.

## Educational Value Added

This project is expected to cover aspects of PCB design, data processing, HDL coding, ASIC design, and overall project design.

## Tasks
<!-- Your Text Here. You may work with your mentor on this later when they are assigned -->

## Design Decisions

<!-- Your Text Here. You may work with your mentor on this later when they are assigned -->

## Design Misc

<!-- Your Text Here. You may work with your mentor on this later when they are assigned -->

## Steps for Documenting Your Design Process

<!-- Your Text Here. You may work with your mentor on this later when they are assigned -->

## BOM + Component Cost

<!-- Your Text Here. You may work with your mentor on this later when they are assigned -->

## Timeline

<!-- Your Text Here. You may work with your mentor on this later when they are assigned -->

## Useful Links

https://github.com/Nicholas-L-Johnson/flip-card

## Log

<!-- Your Text Here. You may work with your mentor on this later when they are assigned -->
