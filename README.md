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
- Plan on using an Xilinx Arty-A7 or D10-Lite development board for initial development

Choosing Materials
- Choosing the FPGA
    - Ideally a low IO count. The minimum for multiplexing the 21x21 array is 42. The full amount for the accelerometer (LIS2DH12TR) is 12. The amount the original project uses is 60 (the full amount of the RP2350). Lower IO count typically means smaller size and easier assembly.
    - Ideally a non BGA package, preferably QFP or TQFP (for easier attachment and lower PCB cost).
    - Enough Logic Elements to run the simulation.
    - Ideally non-volatile, to not have to deal with adding flash memory in.
    - Small enough to leave room for the rest of the circuit.
    - Ideally also compatable with Vivado / Vitis (same as test board).
    - This is the hardest point of planning this project, as finding FPGAs fitting these requirements is difficult.
- Will use the same LEDs used for the original project, 0402 LEDs, unless having to do hand assembly, in that case will use 0602 LEDs or 0603 LEDs.
- Will use the same accelerometer as the original project, an LISDH12TR.
- The FPGA will also require voltage regulators as most require differing voltages for the different voltage rails.
- Other miscellaneous components that we don't know to account for yet.

PCB Design
- After having the complete part requirements, can then work on building the PCB design.

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

![Preliminary BOM](https://github.com/WebTWJ/corahl-project-amp/blob/main/preliminary-bill-of-materials.png)

- *This is on the higher end, it’s unlikely that we will need one with this many logic elements
- **Not including miscellaneous items that we don’t know to account for yet (i.e. voltage regulators, resistors, oscillator, battery)
- ***Self assembly could lower costs, however that is difficult due to the TQFP and VFLGA components and we currently lack the necessary skills and equipment
- ****In case of  hand assembly, a different size LED, such as 0602 or 0603, simplify process

## Timeline
Preliminary:
![Preliminary Timeline](https://github.com/WebTWJ/corahl-project-amp/blob/main/preliminary-timeline.png)

## Useful Links

- https://github.com/Nicholas-L-Johnson/flip-card
- https://course.ece.cmu.edu/~ece500/projects/s22-teame1/

## Log

<!-- Your Text Here. You may work with your mentor on this later when they are assigned -->
