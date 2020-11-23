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
- M stands for Hardware Multiplier adn Divider.
- C stands for compressed instrucitons(16 bit).
SoC is the complete system on a chip. Ex: Raven SOC = PicoRV32(options selected) + RAM blocks + SPI Flash Controller + UART + GPIO + I/O Address Decoder + IRQ Routing

### Design tools
The goal is to show the complete RTL2GDS flow using opensource tools.List of tools used are:
List of Tools:
-Yosys RTL Synthesis
-blifFanout- High fanout net (HFN) synthesis
-graywolf- Placement
-qrouter- Detailed routing
-Magic- VLSI Layout tool
-Netgen- LVS
-OpenTimer and OpenSTA- Static timing analysis tool

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
 
qfow gui:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/qflow%20gui.PNG)

**Other hands-on**

Finding the output area:

Commands:

cd outdir_spi_slave

qflow display spi_slave

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/area%20in%20micron%20D1SK4%20-%20MCQ7.PNG)

% Ratio of flipflop/total logic:

Creating the outdir_spi_slave

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/outdir_spi_slave%20created.PNG)

Commands for using qflow

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/commands%20for%20using%20qflow.PNG)

Running correctly....

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/running%20correctly.PNG)

Synthesis preparation settings

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/synthesis%20preparation%20settings.PNG)

Synthesis completed

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/synthesis%20completed.PNG)

Statistics after synthesis

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/picorv32%20statistics%20after%20synthesis.PNG)

Calculation of percentage

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D1SK4%20-%20MCQ8%20percentage.PNG)

Number of files inside outdir_spi_slave:

commands:

cd vsdflow

./vsdflow spi_slave_design_details.csv

ls -ltr outdir_spi_slave/

ls -ltr outdir_spi_slave | wc

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/16%20files.PNG)

Total 556 that we see is not a file. So, total number of files is 17-1 = 16.

## Day 2

This day is focussed on to complete floor planning.

### Chip Floorplaning
1) Define width and height of core and die based on standard cell dimensions
- Place all standard cells inside the core
- Aspect Ratio = Height/Width
- Utiliztion Factor = Area occupied by the netlist/Total Area of the core 
2) Define location of pre-placed cells
- All of them can be implemented once and can be instantiatd multiple times on to a netlist
- The arrangement of the cells (IP's, blocks, modules) is referred as floorplanning
- pre-placed cells are placed in user-defined locations before automated place and route
- Examples of preplaced cells are memories, clock gating cells, comparator, mux. 
3) Surround pre-placed cells with decoupling capacitors.
- Noise margin(NMh for '1' and NMl for '0')
- NMh = Voh - Vih
- NMl = Vil - Vol
- Vdd or Vss could drop without decoupling caps between Vdd and Vss;
- Decoupling capacitors are huge capacitors between Vdd and Vss and need to be placed near the circuit/block.
4) Power planning
- If many blocks discharges from '1' to '0' at same time in a single ground cause a bump (Gnd bounce). If from '0' to '1' a voltage droop (for single Vdd).
- Istead of single supply lines, use multiple arrays of power supply (Vdd and Vss points). 
5) Pin placement
- Connectivity is described using VHDL or verilog;
- try to put pins near blocks;
- bigger PADs for clock pins.
6) Logical Cell placement blockage --> add PAD ring blocks.

### Placement 
1) Before placement is required to bind netlist with physical cells. 

So, we need a **library**, that is a collection of blocks/cells with different flavors, sizes, Vt(Threshold Voltage), etc.

2) Place the cells

3) Optimize the placement
 - Check signal integrity
 - Add buffers for long paths

### Cell Design Flow (standard cells)

**Inputs:** PDKs, rules (DRC e LVS) and tech files, SPICE models, libraries and user defined specifications (supply voltages, layers, cell sizes, etc).

**Design steps:** circuit design, layout design, characterization (timing, noise, power, .libs, functions, etc).
- GUNA software for characterization
- Timing characterization is based on threshold definitions, propagation delays and transition times.

**Outputs:** CDL, GDSII, LEF, extracted SPICE netlist (.cir)

### LABs
Completion of the preparation step:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/preparation%20step%20done.PNG)

Technology Settings:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/technology%20settings.PNG)

Completion of the Synthesis step:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/synthesis%20done.PNG)

Next is the placement step. Here we have to make the density as 0.7(Utilization Ratio), Aspect Ratio a 1 and rest as default.

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/placement%20settings.PNG)

Now we have to arrange the pins:

For that we create a new group for clk and resetn.

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/create%20your%20own%20group.PNG)

Group Created

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/my_pin_grouping%20created(we%20can%20see%20at%20the%20bottom).PNG)

Pin Settings for clk and reset(Keep both of them at the bottom and make both of them fixed)

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/pin%20settings%20of%20clk%20and%20reset.PNG)

Now, completing the placement

The start of the placement can be seen by the boxes coming as below

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/these%20boxes%20coming%20proves%20starting%20of%20placement.PNG)

While placement is running.....

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/intermediate%20step.PNG)

Placment completed

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/placement%20done.PNG)

Area in micron:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/area%20in%20micron.PNG)

## Day 3

Third day is based on the design and characterization of library cell using MAGIC layout tool and ngspice.

### SPICE Simulations (pre-layout)
- SPICE deck / netlisting
- ngspice intro with simulation commands (source, run, setplot, display, plot)
- Static behaviour evaluation (example of CMOS INV robustness)
 1. Switching thereshold (Vm)
- Dynamic behaviour evaluation
 2. Propagation delay (rise and fall)

### Art of Layout - Euler´s path and stick diagram
- Pull-up + Pull-down networks
- Pre-layout SPICE simulations
- Layout design only using stick diagram as well as using Euler´s path, then stick diagram.
- Without using Euler's path there will be lots of diffusion breaks and contacts. And it may lead to lot's of confusion.
- Euler's path is best method for a error free stick diagram.
- Euler´s path is the first step to input gate re-order (that is an optimization for stick diagram).

### CMOS Fabrication Process

A 16-mask CMOS process is used as example to show how all layout regions are related to the fabrication process.

### LABs

Git cloning ngspice lab:

command:

cd

git clone https://github.com/kunalg123/ngspice_labs.git

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/after%20git%20cloning%20ngspice%20labs.PNG)

PMOS and NMOS Width:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/pmos%20and%20nmos%20width.PNG)

Clearly we can see the pmos width is 0.5 and the nmos width is 0.375

Finding the ratio between Wp and Wn:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/finding%20out%20wp%2Cwn%20ratio.PNG)

Finding the Switching threshold:

Command:

cd

cd ngspice_labs

ngspice inv.spice

There will be terminal like below

ngspice 1 ->

On the above ngspice terminal, type below commands

run

setplot dc1

plot out in

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/opening%20vtc(mcq%208).PNG)

This will open a plot with CMOS VTC and Blue 45 degree line

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/CMOS%20VTC%20and%20Blue%2045%20degree%20line.PNG)

Click on the intersection of Blue line and CMOS VTC. The x0 point is the switching threshold

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/shows%20x0%20value%20lies%20between%201.0%20to%201.1.PNG)

This shows x0 lies in between 1.0 and 1.1

Editing width of pmos to 0.75u in leafpad:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/after%20changing%20pmos%20width%20to%200.75u.PNG)

Plot after pmos width changed

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/plot%20after%20pmos%20width%20changed.PNG)

Finding the x0 point

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/pmos%20width%20changes%20points.PNG)

This is in between 1.1 and 1.2

Finding the rise delay:

Input and Output plot together

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/mcq%2011.PNG)

Expanded plot

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/Expanded%20plot%20for%20calculating%20rise%20delay.PNG)

We have to check for the input and output values at peak_value/2 or 1.25

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/rise%20delay%20calculation.PNG)

We can see this somewhat around to 80ps. The answer in the mcq is 76ps which is closest to this. This is a human error because we cannot select the exact precise point.

Finding out the intersection point of the plot and the 1.25 line:

Commands:

cd

cd ngspice_labs

ngspice fn_prelayout.spice

ngspice 1 -> run

ngspice 1 -> setplot tran1

ngspice 1 -> plot out 1.25

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D3SK2%20-%20MCQ1%20output.PNG)

This is value is around 1.6e-09

X0 at intersection of horizontal blue line and middle falling waveform:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D3SK2%20-%20MCQ2%20output.PNG)

Finding the pulse width:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D3SK2%20-%20MCQ3%20output.PNG)

This is around 650ps

Finding the number of nsubstrate contact and polysilicon strips:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D3SK3%20-%20MCQ3.PNG)

Clearly we have 6 nsubstrate conatact and 6 polysilicon strips

Finding the area of the design:

Commands:

cd

cd ngspice_labs

magic -T min2.tech fn_postlayout.mag &

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D3SK3%20-%20MCQ4%20layout.PNG)

Area can be found out by selecting the whole layout and the executing the command box in the tkcon

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D3SK3%20-%20MCQ4%20area.PNG)

So, the area is 4489 micron.

Finding out x0 at intersection of rising & falling waveform and intersection of horizontal blue line in the postlayout spice:

Postlayout simulation waveform

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/postlayout%20simulation%20waveform.PNG)

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D3SK3%20-%20MCQ5.PNG)

This is around 1.58ns and 2.27ns respectively.

## Day 4

Day 4 is focused on Static timing analysis and some concepts of clock tree synthesis. 

First timing analysis is done using delay tables, then using ideal clocks, then using real clocks.

### Timing Modeling

Timing modeling is performed with the use of delay tables. Depending upon the input slew and output load we can get the delay for the buffer or any other gate.

In order to model timing for clock tree structures, is necessary to take care with:
- at every level, each node drive same load;
- identical buffers need to be used at same level.

### Timing analysis (with ideal clock)

If T is the clock period and td is the combinational logic delay, then T>td. 

If the setup time for the capture flipflop is S, the T > td + S. Otherwise there will be setup time violation.

If the jitter is considered then T > td + S + SU. Otherwise there wil be violation.

Jitter is a temporary timing problem which can be removed if the semiconductor temperature and power noise is maintained correctly.

### Clock Tree Synthesis (CTS)

The main reason for preforming CTS is to remove clock skew. Ideally it should be 0.

Some techniques are used to achieve a good CTS
- H-Tree technique (midpoints to derive clock)
- Using buffers (since H-Tree do not avoid long paths, we need to put buffers)
- Net shielding (to avoid crosstalk/glitches)

### Timing analysis (with real clock)

For timing analysis with real clocks we will consider all the delays, even the clock delays.

### LABs

Finding the input rise and fall slew:

command:

cd

git clone https://github.com/kunalg123/ngspice_labs

cd ngspice_labs

cat inv_tran.spice

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/rise%20and%20fall%20delay.PNG)

Clearly we can see both are 10ps.

Finding out output load and rise delay:

Command:

cd

cd ngspice_labs

cat inv_tran.spice

ngspice inv_tran.spice

ngspice plot

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D4SK1%20-%20MCQ7%20plot.PNG)

Expanded plot

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/plot%20to%20find%20rise%20delay%20in%20D4SK1%20-%20MCQ7.PNG)

Points

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D4SK1%20-%20MCQ7%20points.PNG)

Calculation

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D4SK1%20MCQ7%20answer.PNG)

Changing the output load to 20fF and calculating the rise delay:

Plot

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D4SK1%20-%20MCQ8%20plot.PNG)

Calculation

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D4SK1%20-%20MCQ8%20calulated.PNG)

Using the stdcell library to find out various values:

Path

/usr/local/share/qflow/tech/osu018/osu018_stdcells.lib

We can use either vim, leafpad or less to findout the values. I used vim.

slew_upper_threshold_pct_fall:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D4SK2%20-%20MCQ6.PNG)

Clearly we get 80%

output_threshold_pct_rise :

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D4SK2%20-%20MCQ7.PNG)

2 variables of "delay_template_5x5":

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D4SK2%20-%20MCQ8.PNG)

The variables are total_net_output_capacitance and input_net_transition

The delay table below line number 2943 is for which cell:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D4SK2%20-%20MCQ9.PNG)

This is for INVX1.

Delay template that is used for INVX1:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D4SK2%20-%20MCQ10.PNG)

Delay template that is used for INVX1 is delay_template_5x5

Finding various parameters:

Command:

cd

cd vsdflow/my_picorv32

leafpad picorv32.sdc

We will type below lines in the file picorv32.sdc which we have just opened above

create_clock -name clk -period 2.5 -waveform {0 1.25} [get_ports clk]

We will save and close the above file

Now we will type the below command

leafpad prelayout_sta.conf

Next we will type the below lines in prelayout_sta.conf file which we have just opened above

read_liberty /usr/local/share/qflow/tech/osu018/osu018_stdcells.lib

read_verilog synthesis/picorv32.rtlnopwr.v

link_design picorv32

read_sdc picorv32.sdc

report_checks

We will save and close the above file

Now type below command

sta prelayout_sta.conf

Slack Value:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D4SK2%20-%20MCQ11%20slack.PNG)

We will type the below command

report_checks -digits 4

The data arrival time and data required time is:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/data%20arrival%20time.PNG)

Next we will type the below command

set_propagated_clock [all_clocks]

report_checks

SLACK value after clock propagation:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D4SK4%20-%20MCQ2.PNG)

Launch Clock Network Delay:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D4SK4%20-%20MCQ3%20lauch%20clock%20network%20delay.PNG)

Capture Clock Network Delay:

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D4SK4%20-%20MCQ4%20capture%20clock%20network%20delay.PNG)

Next we will type the below command to find out library hold time and hold slack:

report_checks -path_delay min -digits 4

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/D4SK4%20-%20MCQ5%20library%20hold%20time%20and%20hold%20slack.PNG)

It is around -9.4ps and 222.5ps respectively.

## Day 5

Final workshop day is focused on routing and other steps for final SoC Design.

### Routing
Explanation of Maze routing - Lee´s Algorithm
- It creates a routing grid
- It finds the best route from a source to a target
- It is an automated process

Optical Photolithography is used to build wires. So, we need light for this. 

### DRC
- DRC-Design Rule Check
- If there is a drc error we have to do drc clean
- Some critical drc checking points are signal short, via spacing, via width, wire width, wire pitch, wire spacing
- To remove signal short we usually use different metals where there is a short. Usually the above wire is wider than the below.

### Parasitics Extraction
- SPEF: Standard Parasitics Exchange Format
- IEEE 1481-1999
### LABs

The labs were focused on timing analysis before and after timing analysis.

Finding out the pre layout frequency:

Command:

cd

cd vsdflow/my_picorv32

qflow route picorv32

qflow sta picorv32

qflow backanno picorv32

leafpad log/sta.log

The routing will take around 40 minutes to complete.

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/maximum%20clock%20frequency.PNG)

This is around 314MHz

Finding out the post layout frequency:

We will use the below command:

log/post_sta.log

![](https://github.com/SaiSwaroopMishra/VSD-Physical-Design-Workshop/blob/main/Images/maximum%20freq%20after%20post%20layout.PNG)

This is around 294 MHz

So, after routing, a drop in '20 MHz' can be observed for the maximum clock frequency.

## Acknowledgement

Kunal Ghosh, Co-founder (VSD Corp. Pvt. Ltd)
