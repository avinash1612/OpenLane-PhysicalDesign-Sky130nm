# OpenLane-PhysicalDesign-Sky130nm
This repository is developed as part of 130nm Advanced Physical Design Project with help of VSD.


## Project details: 
Author: [Avinash Hegde](https://www.linkedin.com/in/avinashhegdek/)

Graduate Student, MS in Computer Engineering,
University of Cincinnati, Ohio.
email: kotaae@mail.uc.edu 

[download resume](https://bit.ly/Avinash_Resume)
  
### Skills:
Digital ASIC Design, RTL2GDS Flow, Verilog HDL, Tcl scripting, STA, Spice deck creation and simulation, Floor planning, Placement, Power Planning, Clock Tree Synthesis, Routing, ECO

### Tools:
 ModelSim, Xilinx Vivado, Synopsys Design Compiler, TetraMax, Hspice, iverilog, yosys, abc, ngspice, MAGIC, OpenSTA, TritonRoute

## About the design:
RTL2GDSII has been done for the design picorv32a. (picorv32a.v from [OpenLANE designs](https://github.com/The-OpenROAD-Project/OpenLane/tree/master/designs/picorv32a/src))

### About the tool chain:
OpenLANE has been usted to perform RTL2GDSII. OpenLane is an open source ASIC design flow that can be used for tapeouts. It is a compilation of Yosys, abc, openROAD, FAULT, MAGIC, OpenSTA, ngspice and many other tools.
The tool has been accessed with "interactive" tag. Interactive mode of the tool will need manual intervention in each step to launch tool at every stage of the flow. 


Tool chain can be installed from [OpenLANE installation](https://github.com/The-OpenROAD-Project/OpenLane)

FLow of the tool:
1. Synthesis
2. Floor Planning
3. Power Planning
4. Placement
5. Clock Tree Synthesis
6. Routing
7. Signoff

STA after Synthesis, Placement and Routing. ECO (Engineering Change of Order) is also supported by the tool. 
The tool supports changes to configuration on the fly. 
# RTL2GDSII of picorv32a -RISCV CPU
change directory to openlane working dir/openlane

Initiate docker using `docker`.

To invoke OpenLane tool chain in interactive mode: `./flow.tcl - interactive`

![Capture](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/d385c6f6-24a4-4f1b-b162-4e980671cf39)

Once this is done, setup package for the tool and then prepare design for the flow. 
````
package require openlane 0.9
prep -design picorv32a
````

When the setup above is succesful, design can be synthesised using Yosys and abc. ABC is used for technology mapping.
# Synthesis

Yosys and ABC were used to perform Logic optimization and Technology Mapping.

Synthesis can be done using the command `run_synthesis`

Parameters were set using config.tcl.
list of parameters that I tweaked:
  1. `$SYNTH_BUFFERING` : Enabling buffering increased area and delay but helped in keeping up the signal integrity.
  2. `$SYNTH_SIZING` : Allowed sizing of cells to accomodate effective implementation of the strategy
  3. `$SYNTH_MAX_FANOUT` : Fanout has been restricted to reduce delay caused due to capacitive load and also help in meeting slack/timing requirements.
  4. `$SYNTH_STRATEGY` : DELAY and AREA are the two strategies that are currently supported by the tool. 
  
Below is a snapshot of the terminal showing succesful completion of synthesis:

<img src ="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/14ac00df-eb27-45f3-b4d4-60c18e47fc22" width =50%>

Netlist was generated. The netlist consists of 14876 cells. Out of which, 1613 are flipflops. Flop ratio was calculated after synthesis to estimate power consumption. 
Flop ratio was calculated to estimate the power consumption because the power consumption reported by the tool does not account for clock until CTS. As clock tree is the most switching and power hungry network on chip, flop ratio was used to estimate it's power consumption. This gives the designer an idea and can help in thinking of alternatives in the very initial stages of design flow.

Flop ratio was 10.84%

Below is a snapshot of report generated after synthesis:

<img src ="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/afd5387d-4c06-40c5-afb5-807a071cb3bf" width =50% height =50%>


# Placement:

### Floor Planning:
Placement of Macros, IO pins, 

Parameters were set accordingly in tcl script.
Utilisation factor of 0.5 and Aspect ratio of 1 were the design constraints.
Paramteres that I have tweaked are:
1. `$FP_CORE_UTIL`
2. `$FP_ASPECT_RATIO`
3. `$FP_IO_HMETAL`
4. `$FP_IO_VMETAL`

Forcedirected Placement has been done. HPWL was used to estimate wirelength. MAGIC was used to view, debug the floor plan.
To generate floorplan:
```
run_floorplan
```

The snapshot below is an image of floor plan captured by KLayout:

<img src ="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/3f9ac5c3-de06-47d9-9642-27cb822c9d5d" width =50% height=50%>


The image below is from MAGIC Layout tool:

<img src ="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/d527b866-0578-4092-8666-d413f8e76654" width =50% height=50%>



The image below has been used to perform sanity check after floor planning.
A reduction of 1.2% of wirelenght was observed.

<img src ="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/a16aff16-130d-4641-812b-5cf48a23c2b4" width =50% height=50%>


<img src ="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/a26ef6e4-3fb3-41ce-b110-d46ec840e74b" width=50%>
### Detailed Placement:

```
run_placement
```

<img src ="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/06f3e37e-47bc-4216-8605-99d9c52c4330" width =50%>

Inverter Design: (cite vsdstdcelldesign git)

Mag file of inverter has been cloned to perform spice simulation. 

<img src ="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/c435c317-9194-40b9-bae5-64e38f901c28" width=50%>

ngspice has been used to simulate spice extracted from the layout. 




<img src ="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/3f882f6f-2f52-438d-92d9-7590df2fa854" width=50%>

The image below is a snapshot of spice simulation (transient analysis) of custom inverter using ngspice:

<img src ="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/7d6db82c-548b-4db2-9ccb-6b530df5adba" width=50%>

Rise time: Time taken for the output to rise from 20% to 80%.

Rise time for the CMOS inverter = 2.40 - 2.34 = 0.06 ns = 60 pico seconds

Fall time: Time taken for the output to fall from 80% to 20%.

Fall time for the CMOS inverter = 4.15 - 4.08 = 0.07 ns = 70 pico seconds


### DRC fixed for Sky130 exercises:

<img src ="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/37889393-8253-434e-abbb-a0abbab0f032" width =50%>


POLY DRC


<img src="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/52d66496-302d-43ca-b914-a29a7583ef56" width =50%>


N-Well


<img src="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/a3e9914d-624f-4c1c-b7ff-5593cf714fe5" width =50%>


<img src ="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/164b6481-2175-4720-a952-120a710c9c3a" width =50%>

<img src ="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/7804650b-7c09-4861-8f4a-212d34b3397a" width =50%>

## Static Timing Analysis

Static Timing Analysis has been done using OpenSTA. ECO had to be performed to meet timing. 
<img src ="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/8e7b8703-e628-4acb-a657-e0e443cca33a" width =50%>

<img src ="https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/df943936-88e5-4cfe-ad62-d2b316a21858" width =50%>

## Engineering Change of Order
Any deviation (typically going to a previous step or to the initial step) to meet some design specifications (generally timing) is called ECO. Any such change at a particular level will have consequences in upcoming (and/or previous) previous stages of the flow. It is important for a PD engineer to ensure that all files are updated based on ECO performed.

I have had to change few synthesis parameters and strategies in tcl script to meet timing. This caused a change in netlist and there-by, I had to re-run all the commands in the flow. 
Negative slack is never allowed. Positive slack within 10% of the clock period is the worst case allowed depending upon the design. 

## GDSII generation:
Right before GDSII extraction, SPEF (Standard Parasitic Extraction Format) has been extracted. Multi-corner STA will be take SPEF into consideration.  
I have used `run_magic` in openLANE to generate GDSII. GDSII is geometric data stream and not human readable like CIF. 

The snapshot below shows succesful execution of the command:

![gds generated](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/a9569b6e-daf7-40d8-b006-3c8001934069)

The genration of GDSII is confirmed by this snapshot below:
![gds generated 2](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/ea542e65-b9fb-4e12-ab97-1ead85d20ede)

Image below is of a GDSII file captured using KLayout:

<img src = "https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/b82601b9-ae85-45b1-9713-534d28300d09" width =50%>


## Acknowledgement

Kunal Ghosh - VLSI System Design Corp.

Nickson Jose - Instructor, VSD Corp.


