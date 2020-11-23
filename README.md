# VSD-Physical-Design-Workshop
This is a repository on Physical Design Workshop conducted by VLSI System Design(VSD) Corporation on VSD-IAT platform.

## Day 1

First day was to see the chip from the top level without diving into it. 

### Basic Intoduction of a chip
- Package: QFN-48 (QFN stands for Quad Flat No leads)
- Inside the package at the center we have the chip
- Using PADS we can send the signal inside the chip
- Foundary IPs - PLL, SRAM, ADC, DAC, etc
- Macros - RISC-V SOC, SPI, etc

### RISC-V ISA
An introduction to RISC-V architeture:
- Translation from high-level language to machine code: C -> ASM -> binary code (compiler + assembler)
- RISC-V Architecture can be considered as a specification. And RTL(picorv32 CPU core) implements this specification.
- Then RTL to layout is a standard RTL2GDS flow.
PicoRV32 - Clifford Wolf's open source implementation of a RISC-V RV32IMC.
- I stands for BAsic Integer Instruction Set.
- M stand s for Hardware Multiplier adn Divider.
- C stands for compressed instrucitons(16 bit).
SoC is the complete system on a chip. Ex: Raven SOC = PicoRV32(options selected) + RAM blocks + SPI Flash Controller + UART + GPIO + I/O Address Decoder + IRQ Routing

### Design tools
The goal is to show the complete RTL2GDS flow using opensource tools.List of tools used are:
List of Tools:
•Yosys RTL Synthesis
•blifFanout- High fanout net (HFN) synthesis
•graywolf- Placement
•qrouter- Detailed routing
•Magic- VLSI Layout tool
•Netgen- LVS
•OpenTimer and OpenSTA- Static timing analysis tool

Last but not least, Qflow is presented as a complete tool chain (using tools as Yosys and graywolf) for complete RTL2GDS flow.

There is also a gui called as qflow gui.
- vsdflow invokes qflow.

### LABs:

**Cloning the vsdflow repo**
Using the link below:
git clone https://github.com/kunalg123/vsdflow.git
AFter cloning what we will see something as below:
![](Images/after%20cloning.PNG)
**Checking whether the tools are correctly installed or not**
Yosys Installed properly:
![](Images/yosys%20installed.PNG)
The "yosys>" symbol shows that yosys is installed correctly.
GrayWolf Installed properly:
![](Images/graywolf%20installed.PNG)
Magic Installed properly:
![](Images/magic%20installed%20correctly.PNG)
STA Istalled properly:
![](Images/sta%20installed%20correctly.PNG)
OpenTimer Installed properly:
![](Images/opentimer%20installed.PNG)
Qrouter Installed properly:
![](Images/qrouter%20installed%20correctly.PNG)
Path where graywolf is installed:
![](Images/where%20graywolf%20installed.PNG)
Path where sta is installed:
![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/which%20sta.PNG)
 

## Day 2

Second day presents the Chip floorplanning as the preparation before automatic placement and routing. Also presents the requirement of library cells and it design flow.

### Chip Floorplaning
Second workshop day begins with **Chip Floorplanning concept**. Some steps are introduced as part of Chip Floorplanning phase:
1) Define width and height of core and die based on standard cell dimensions
- Place all standard cells inside the core
- notions of utilization factor and aspect ratio.
2) Define location of pre-placed cells
- separate circuits in to blocks or modules
- select available IP's
   * The arrangement of the cells (IP's, blocks, modules) is referred as floorplanning.
   * pre-placed cells are placed in user-defined locations before automated place and route.
3) Surround pre-placed cells with decoupling capacitors.
   * Noise margin concept is presented (NMh for '1' and NMl for '0')
- Vdd or Vss could drop without decoupling caps between Vdd and Vss;
   * Decoupling caps. are huge caps between Vdd and Vss and need to be placed near the circuit/block.
4) Power planning
- If many blocks discharges from '1' to '0' at same time in a single ground cause a bump (Gnd bounce). If from '0' to '1' a voltage droop (for single Vdd).
- Istead of single supply lines, use multiple arrays of power supply (Vdd and Vss points). 
5) Pin placement
- Connectivity is described using VHDL or verilog;
- try to put pins near blocks;
- bigger PADs for clock pins.
6) Logical Cell placement blockage --> add PAD ring blocks.

**After floorplanning. We are ready for placement and routing steps**

Some tips are provided to use 'qflow gui' to setup placement (using pin arrangement UI).

Before introduction of Cell Design flow, some motivation is provided about **Placement** phase.

### Placement 
1) Before placement is required to bind netlist with physical cells. 

So, you need a **library**, that is a collection of blocks/cells with different flavors, sizes, *vt*, etc...).

2) Place the cells

3) Optimize the placement
 - check signal integrity;
 - add buffers for long paths;

4) After optimization, setup the timimg analysis with ideal clock. **we return to this on Day 4**

### Cell Design Flow (standard cells)

**Inputs:** PDKs, rules (DRC e LVS) and tech files, SPICE models, libraries and user defined specifications (supply voltages, layers, cell sizes, etc...).

**Design steps:** circuit design, layout design, characterization (timing, noise, power, .libs, functions, etc...).
- GUNA software for characterization
- timing characterization is based on threshold definitions, propagation delays and transition times.

**Outputs:** CDL, GDSII, LEF, extracted SPICE netlist (.cir)

### Hands-on Labs

Two way hands-on labs are performed in day 2. First one is from learning concepts, to use qflow gui for preparation and placement, as follows:

    Create my_picorv32 directory
    Enter in my_picorv32 directory
    Create source synthesis layout

    cp ../verilog/picorv32.v source/.
    Qflow gui &

    Select technode osu018
    Select verilog module picorv32

    Set stop

    Prep: run
    Synthesis: run

    Placement settings: init. Density 0.7
    Arrange Pins: Auto Group and apply
    Arrange Pins: New Group and create my_pin_grouping for resetn and clk pins. Check only left box. 
    
as we can seen in image below. 
    
![q1_2](https://github.com/britovski/PhyDesign_WS/blob/main/images/l22.PNG)

    Then, Run placement. 

After that, is possible to see placement running with graywolf on below image screen

![q2_2](https://github.com/britovski/PhyDesign_WS/blob/main/images/l23.PNG)

**Second lab is guided by MCQs, and the goal is to measure layout area, as follows:**

Type below command

    cd
    cd vsdflow/my_picorv32
    qflow display picorv32 &
    
This will open layout and tkcon window In the layout window, select whole chip using below steps

    Take cursor to bottom left
    Left mouse click
    Take cursor to top right
    Right mouse click
    Press Shift+i

This will select the whole layout Now in tkcon window, type below command

    box

What is the area of you chip in microns?

![q3_2](https://github.com/britovski/PhyDesign_WS/blob/main/images/l21.PNG)

As we can see on image above, the answer is '812062.19 microns.'


## Day 3

Third day is focused on the design and characterization of one library cell using MAGIC layout tool and ngspice.

### SPICE Simulations (pre-layout)
- SPICE deck / netlisting
- ngspice intro with simulation commands (source, run, setplot, display, plot...)
- static behaviour evaluation (example of CMOS INV robustness)
 1. switching thereshold (*Vm*)
- dynamic behaviour evaluation
 2. propagation delay (rise and fall)

### Art of Layout - Euler´s path and stick diagram
- pull-up + pull-down networks (concept introduced by building a complex logic circuit with 6 inputs and 1 output).
- pre-layout SPICE simulations (you can use virtual machine or cygdrive on windows for ngspice and MAGIC)
- discussion of stick diagram only vs using Euler´s path.
 * stick only needs lots of contacts/metal connections and diffusion breaks;
 * with Euler´s path is the best option.
- Euler´s path is the first step to input gate re-order (that is an optimization for stick diagram).
 * PMOS + NMOS network graphs (numbering nodes and edges as transistor labels).
- Do stick diagram with gates re-ordered.
- Abstract level layout can be done using optimized stick diagram.

### MAGIC Labs
- stick diagram to layout or abstract level layout to real layout dimensions;
- check tech file/design rules;
- derive actual dimensions;
- prepare script to create layout in MAGIC (*draw_fm.tcl*);
- final layout and labeling

        > magic -T 'techfile'
        put script commands in tkcon to create layout step-by-step
        or use 
        > source draw_fm.tcl
        finish layout
        label and save
        
- Extract SPICE with parasitics, typing below commands in tkcon

        > extract all
        > ext2spice

### Post-layout simulation
- use .ext file from MAGIC extraction
- simulate and compare with pre-layout

### CMOS Fabrication Process

A 16-mask CMOS process is used as example to show how all layout regions are related to the fabrication process.

### Hands-on Labs

For the third day labs, the MCQs guide to use ngspice_labs repo @ github.

**First step is to clone ngspice_labs repo**

    git clone https://github.com/kunalg123/ngspice_labs.git
 
After that, some tasks are guided, as to check transistor sizes

    cd ngspice_labs
    cat inv.spice

What is the width of PMOS and NMOS transistors?

![q1_3](https://github.com/britovski/PhyDesign_WS/blob/main/images/33.PNG)

As we can see in image above, the answer is '0.5 for PMOS and 0.375 for NMOS', respectively.

Then, we go through simulations

Go to labs, open terminal
Type below commands

    cd ngspice_labs
    ngspice inv.spice

There will be terminal like below

    ngspice 1 ->

On the above ngspice terminal, type below commands

    run
    setplot dc1
    plot out in

This will open a plot with CMOS VTC and Blue 45 degree line
Click on the intersection of Blue line and CMOS VTC.
Go to terminal
What does "x0" value lies between?

![q2_3](https://github.com/britovski/PhyDesign_WS/blob/main/images/34.PNG)

As we can seen in image above, the answer is '1.0v-1.1v'

Then, go to labs, open terminal
Type below command

    leafpad inv.spice

Edit the width of PMOS to 0.75u
Press Ctrl+s to save the file and exit the file
Repeat experiment given in D3SK1 - MCQ8 (previous MCQ)
Where does the switching threshold lies between?

![q3_3](https://github.com/britovski/PhyDesign_WS/blob/main/images/35.PNG)

As we can seen in image above, the answer is '1.1v-1.2v'

Go to labs, open terminal
Open file called "inv_tran.spice" using below command

    leafpad inv_tran.spice

Change PMOS width to 0.75u, Save and Close

Type below commands for transient simulations

    ngspice inv_tran.spice
    ngspice 1 -> run
    ngspice 1 -> setplot tran1
    ngspice 1 -> plot out in

What is the rise delay? Refer to Lesson 4 of D3SK1 to learn how to calculate rise delay

![q3_4](https://github.com/britovski/PhyDesign_WS/blob/main/images/39corr.PNG)

As we can seen in image above, the answer is approximately '76ps'.

Go to labs, open terminal
Type below commands

    cd ngspice_labs
    magic -T min2.tech

This will open magic layout window and tkcon window
Go to tkcon window and type below command

    source draw_fn.tcl

In layout window, how many nsubstratecontact and how many polysilicon strips you observe?

![q3_5](https://github.com/britovski/PhyDesign_WS/blob/main/images/37.PNG)

As we can seen in image above, the answer is '8 e 6', respectively.

Go to labs, open terminal
Type below command

    cd ngspice_labs
    magic -T min2.tech fn_postlayout.mag &

What is the area of this design?

![q3_6](https://github.com/britovski/PhyDesign_WS/blob/main/images/38.PNG)

As we can seen in image above, the answer is '4489 microns.'

After that, the MCQ guide to perform the post-layout simulation of the previously presented logic circuit, doing parasitic extraction and then editing the file with the same simulation commands from pre-layout simulation.

![q3_7_1](https://github.com/britovski/PhyDesign_WS/blob/main/images/41.PNG)

Question: What is the value of X0 at intersection rising & falling waveform and intersection of horizontal blue line?

![q3_7_2](https://github.com/britovski/PhyDesign_WS/blob/main/images/42.PNG)

As we can seen in image above and performing calculations, the answer is 'around 1.58ns and 2.27ns respectively'.
* There was no blue line, but we figure out that is calculated in reference to the the Vdd/2 X value.


## Day 4

Day 4 is focused on timing analysis and some concepts of clock tree synthesis. First is shown timing modeling using delay tables and then is shown how timing analysis is performed with ideal and real clocks.

### Timing Modeling

Timing modeling is performed with the use of delay tables, that combines *input slews* and *output load* values.

In order to model timing for clock tree structures, is necessary to take care with:
- at every level, each node drive same load;
- identical buffers need to be used at same level.

### Timing analysis (with ideal clock)

If *T* is the clock period and *Teta* is the combinational logic delay, is necessary that *T > Teta*.

Considering setup time *S* for a capture FF, *Teta < (T-S)*.

Considering clock jitter, is necessary to take care with setup time uncertainty *SU*, so *Teta < (T-S-SU)*.

### Clock Tree Synthesis (CTS)

The goal of CTS is to put clock skew as low as possible (ideally *0 ps*).

Some techniques are used to achieve a good CTS:
- H-Tree technique (midpoints to derive clock);
- Buffering (since H-Tree do not avoid long paths, we need to put buffers);
- Net shielding (to avoid crosstalk/glitches).

### Timing analysis (with real clock)

In real timing analysis is necessary to consider clock delays.

Considering the topology of a launch FF connected to a combinational logic circuit and then with a capture FF, *Delta1* been the delay from *CLK* to launch FF, and *Delta2 been the delay from *CLK* to capture FF, *(Teta + Delta1) < [(T + Delta2) - S - SU]*. The first term is the Data Arrival Time (DAT) and the second term is the Data Required Time (DRT). *DRT- DAT* is known as *Slack* and need to be *0* or positive.

You can also do timing analysis considering hold time *H* istead of setup time. In this case, *(Teta + Delta1) > [(T + Delta2) + H + HU]*.

### Hands-on Labs

The labs are performed to analyze .lib file and also to setting up files for 'sta'.

Some inspections of the below file are done

    /usr/local/share/qflow/tech/osu018/osu018_stdcells.lib

Then, labs goes to setting up files for timing analysis.

Type below command

    cd vsdflow/my_picorv32
    leafpad picorv32.sdc

Type below lines in the file picorv32.sdc file which you have just opened above

    create_clock -name clk -period 2.5 -waveform {0 1.25} [get_ports clk]

Save and close the above file

Now type below command

    leafpad prelayout_sta.conf

Type below lines in prelayout_sta.conf file which you have just opened above

    read_liberty /usr/local/share/qflow/tech/osu018/osu018_stdcells.lib
    read_verilog synthesis/picorv32.rtlnopwr.v
    link_design picorv32
    read_sdc picorv32.sdc
    report_checks

Save and close the above file

Now type below command

    sta prelayout_sta.conf

What is the SLACK value you see?

![q3_4](https://github.com/britovski/PhyDesign_WS/blob/main/images/l43.PNG)

As we can see in the image above, the answer is '-0.56'.

Repeat all steps.
NOTE - If you have already done that, then you will see below 'sta' terminal like below

    %

Type below command

    report_checks -digits 4

What is the data arrival time?

![q3_4](https://github.com/britovski/PhyDesign_WS/blob/main/images/l44.PNG)

As we can see in the above image, the answer is '2.9001'.

## Day 5

Final workshop day is focused on routing and other steps for final SoC Design.

### Routing
Explanation of Maze routing - Lee´s Algorithm
- creates a routing grid;
- find best route from a source to a target;
- automated process*;

### DRC
- know typical rules;
- check for DRC violations;
- make a DRC clean.

### Parasitics Extraction
- Every physical via is represented at least as an RC circuit;
- SPEF / IEEE 1481-1999 (Representation format);

### Pending: signoff timing analysis (complete timing analysis using SPEF) and power analysis

### Some tips of Qflow on PnR
- Pin placemente/add groups. Try to relax constraints if get errors;
- STA, result includes max. clock
- Routing, that creates parasitic file for post-route STA
- Migration, just converts results into file format used by open galaxy
- Using edit layout (*s* for top level cell selection, and *shift + f* to expand cell view).

### Hands-on Labs

The labs were focused on timing analysis before and after timing analysis.

Open terminal
Type below commands

    cd vsdflow/my_picorv32
    qflow route picorv32
    qflow sta picorv32
    qflow backanno picorv32
    leafpad log/sta.log

What is the pre-layout frequency?
Hint - Search for case-sensitive keyword "MHz"

![q1_5](https://github.com/britovski/PhyDesign_WS/blob/main/images/l54.PNG)

As we can see in above image, the answer is '314 MHz'.

Then, doing routing...

![q2_5](https://github.com/britovski/PhyDesign_WS/blob/main/images/l52.PNG)

and open the below file

    log/post_sta.log

What is post-layout frequency?

![q1_5](https://github.com/britovski/PhyDesign_WS/blob/main/images/l55.PNG)

As we can see in above image, the answer is '294 MHz'.

So, after routing, a drop in '20 MHz' can be observed for the maximum clock frequency.


## Final notes

Good to learn physical design skills from digital side and to know the basic opensource tools flow.

## Acknowledgement

Kunal Ghosh, Co-founder (VSD Corp. Pvt. Ltd)
