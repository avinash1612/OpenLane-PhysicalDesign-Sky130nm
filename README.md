# OpenLane-PhysicalDesign-Sky130nm
This repository is developed as part of Advanced Physical Design Project with help of VSD.
# To the reviewer: I am still updating this. I have not finished doing it. Yet to update the repo

## Project details: 
Author: [Avinash Hegde](https://www.linkedin.com/in/avinashhegdek/)

Graduate Student, MS in Computer Engineering,
University of Cincinnati, Ohio.
email: kotaae@mail.uc.edu 

### Skills:
Digital ASIC Design, RTL2GDS Flow, Verilog HDL, Tcl scripting, STA, Spice deck creation and simulation, Floor planning, Placement, Power Planning, Clock Tree Synthesis, Routing, ECO

### Tools:
iverilog, yosys, abc, ngspice, MAGIC, OpenSTA, TritonCTS

## About the design:
RTL2GDSII has been done for the design picorv32a. (picorv32a.v from [OpenLANE designs](https://github.com/The-OpenROAD-Project/OpenLane/tree/master/designs/picorv32a/src))
Day-1: Inception opf open-source EDA, OpenLANE and SKY Water 130nm PDK

### About the tool chain:
OpenLANE has been usted to perform RTL2GDSII. OpenLane is an open source ASIC design flow that can be used for tapeouts. It is a compilation of Yosys, abc, openROAD, FAULT, MAGIC, OpenSTA, ngspice and many other tools.
The tool has been accessed with "interactive" tag. Interactive mode of the tool will need manual intervention in each step to launch tool at every stage of the flow. 

/PNR flow here/

Tool can be installed from [OpenLANE installation](https://github.com/The-OpenROAD-Project/OpenLane)

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

synthesis can be done using the command `run_synthesis`

Parameters were set using config.tcl.
list of parameters that I tweaked:
  1. `$SYNTH_BUFFERING` : Enabling buffering increased area and delay but helped in keeping up the signal integrity.
  2. `$SYNTH_SIZING` : Allowed sizing of cells to accomodate effective implementation of the strategy
  3. `$SYNTH_MAX_FANOUT` : Fanout has been restricted to reduce delay caused due to capacitive load and also help in meeting slack/timing requirements.
  4. `$SYNTH_STRATEGY` : DELAY and AREA are the two strategies that are currently supported by the tool. 
  
Below is a snapshot of the terminal showing succesful completion of synthesis:
![synth2](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/14ac00df-eb27-45f3-b4d4-60c18e47fc22)
                              Figure1: Synthesis


Flop ratio was calculated after synthesis to estimate power consumption. 

Flop ratio was 10.84%

![assign1](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/afd5387d-4c06-40c5-afb5-807a071cb3bf)

Day2: 

Floor planning:
Utilisation factor of 0.5 and Aspect ratio of 1 were the design constraints.
```
FP_CORE_UTIL, FP_ASPECT_RATIO
```
were set accordingly in tcl script.
Forcedirected Placement has been done. HPWL was used to estimate wirelength.
To execute floorplan:
```
run_floorplan
```
![Capture1](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/8c29b181-3365-4a70-a67a-e4edbade20c4)

A reduction of 1.2% of wirelenght was observed.

![Capture1](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/a16aff16-130d-4641-812b-5cf48a23c2b4)

MAGIC was used to view the floor plan.

![floorplan](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/d527b866-0578-4092-8666-d413f8e76654)

![floor plan close up2](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/a26ef6e4-3fb3-41ce-b110-d46ec840e74b)

Inverter Design: (cite vsdstdcelldesign git)

Mag file of inverter has been cloned to perform spice simulation. 

![vsd inv 2](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/c435c317-9194-40b9-bae5-64e38f901c28)

ngspice has been used to simulate spice extracted from the layout. 

//spice simulation result here.

TO THE VSD REVIEWER: I DID ALL THE LABS. WILL FINISH THIS WRITE UP SOON.
![vsd_inv lef](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/3f882f6f-2f52-438d-92d9-7590df2fa854)
![spice_inv_s1](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/7d6db82c-548b-4db2-9ccb-6b530df5adba)

![new_cf_delay](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/c45ee8f8-62c7-4116-b721-1759cdf94718)

![rise_transition_80](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/90dad95a-afb3-4e6a-8581-7df0b8ec9ba4)
![rise_20percent](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/13919d41-582e-4249-bbf3-4c0376492a27)
![rise_transition_80](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/2b6b7b94-ef62-4436-8d53-f605e0f87ad2)

DRC fixed:

![drc fixed m3](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/37889393-8253-434e-abbb-a0abbab0f032)


POLY DRC


![drc check poly](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/52d66496-302d-43ca-b914-a29a7583ef56)


N-Well


![drc fixed nwell](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/a3e9914d-624f-4c1c-b7ff-5593cf714fe5)


![post routing](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/164b6481-2175-4720-a952-120a710c9c3a)

![writing db and reading lib](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/7804650b-7c09-4861-8f4a-212d34b3397a)
![slack met after cts2](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/8e7b8703-e628-4acb-a657-e0e443cca33a)
![slackmet fin](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/df943936-88e5-4cfe-ad62-d2b316a21858)


GDS generated
![gds generated](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/a9569b6e-daf7-40d8-b006-3c8001934069)


![gds generated 2](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/ea542e65-b9fb-4e12-ab97-1ead85d20ede)





