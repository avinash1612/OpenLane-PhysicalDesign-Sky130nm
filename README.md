# OpenLane-PhysicalDesign-Sky130nm
This repository is developed as part of Advanced Physical Design workshop by VSD.

RTL2GDSII has been done for the design picorv32a. (picorv32a.v from [OpenLANE designs](https://github.com/The-OpenROAD-Project/OpenLane/tree/master/designs/picorv32a/src))

Day-1: Inception opf open-source EDA, OpenLANE and SKY Water 130nm PDK

OpenLANE has been usted to perrform RTL2GDSII. OpenLane is an open source ASIC design flow that can be used for tapeouts. It is a compilation of Yosys, abc, openROAD, FAULT, MAGIC, OpenSTA, ngspice and many other tools.
The tool has been accessed with "interactive" tag. Interactive mode of the tool will need manual intervention in each step to launch tool at every stage of the flow. 

/PNR flow here/

As part of the workshop, we were provided with image of virtual disk, that has all the necessary tools compiled in it. Tool can be installed from [OpenLANE installation](https://github.com/The-OpenROAD-Project/OpenLane)
To invoke OpenLane tool chain in interactive mode: 
```
*change directory to openlane working dir/openlane*
docker
./flow.tcl - interactive
```
![Capture](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/d385c6f6-24a4-4f1b-b162-4e980671cf39)

Once this is done, setup package for the tool and then prepare design for the flow. 
````

package require openlane 0.9
prep -design picorv32a
````

When the setup above is succesful, design can be synthesised using Yosys and abc. ABC is used for technology mapping.
````

run_synthesis
````

![synth2](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/14ac00df-eb27-45f3-b4d4-60c18e47fc22)


Flop ratio was calculated after synthesis to estimate power consumption. 

Flop ratio was 10.84%

![assign1](https://github.com/avinash1612/OpenLane-PhysicalDesign-Sky130nm/assets/56393465/afd5387d-4c06-40c5-afb5-807a071cb3bf)

