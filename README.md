# Advanced-Physical-Design-using-SKY-130-PDK
Open Source Physical Design from RTL to GDS-II
## About The Project
OpenLane is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, CU-GR, Klayout and a number of custom scripts for design exploration and optimization. The flow performs full ASIC implementation steps from RTL all the way down to GDSII. We will be performing complete OPEN ASIC Flow on Reference picorv32-a RISC-V core.
## Openlane WorkFlow:
![image](https://user-images.githubusercontent.com/107258443/175244818-882577f4-60ac-4698-bddd-8aa0074ea9cf.png)
<br />**Fig01:  Openlane Workflow**

Openlane workflow consists of following stages-
### 1. RTL Synthesis
- Yosys: Used for RTL synthesis
- ABC: ABC tool for technology mapping of yosys's internal gate library to a target architecture.
- OpenSTA: For Static Timing Analysis after the Synthesis

### 2. Floorplan,Power-distribution Network Formation,Placement,Clock Tree Synthesis
- OpenROAD is a package which is used for all the above mentioned process. 
Placement is of two types, one is global placement in which tool roughly places the design across the chip and other one is detailed placement in which standard cells are placed in the appropriate rows.
TritonCTS - forms the clock distribution network
  
### 4. Routing
- FastRoute - Performs global routing to generate a guide file for the detailed router
- TritonRoute - Performs detailed routing from global routing guides
- SPEF-Extractor - Performs SPEF extraction that include parasitic information
  
### 5. GDSII Generation
- Magic - Streams out the final GDSII layout file from the routed def
=======================================================================================

## Day 1
### Getting into the Skywater PDKs:
SKY130A is the opensource PDK which is used in this project. open_pdks is the modified PDKs so that they can be compatible with the open source EDA tools.

![image](https://user-images.githubusercontent.com/110470328/183156964-b596f370-3e85-406d-9489-32ea2d767873.png)
<br />**Fig02:  Skywater PDK Directory**

### Preconfigured Designs in Openlane
There are many preconfigured designs in the openlane/designs folder and we will using picorv323a design which is a opensource RISC-V based CPU.
![3](https://user-images.githubusercontent.com/110470328/183157557-c5951a31-49b8-4a79-960c-31b7c4a3a45f.JPG)
<br />**Fig03: picorv32a Design for the flow**

![5](https://user-images.githubusercontent.com/110470328/183157822-58a56077-ca78-4492-87d9-066fe937e9c7.JPG)
<br />**Fig04: Contents of picorv32a Design Folder**

In the above snippet we can see that there are two config.tcl files which contains the constraints that will be used in the Openlane Flow.
sky130_sky130a_fd_sc_hd_config.tcl has high precedence compared to config.tcl.
runs folder will contains the results and reports after execution of each stage of flow.

### Invoking Openlane
1. Openlane can be invoked by either using **"docker"** or **"make mount"** commmands.
2. Run **"./flow.tcl -interactive"** to run the OpenLANE flow in interactive mode i.e we will have to run each step manually
3. Import openlane package using **"package require openlane 0.9"**

![1](https://user-images.githubusercontent.com/110470328/183159288-e7a5b457-d40e-48de-9629-0d3d0e28c81d.JPG)
<br />**Fig05: Invoking Openlane**

### Design Stage Preparation
  **_Command: "prep -design picorv32a"_**
<br />This command will create directory structure required for openlane flow. We need to modify .tcl file to change the constraints as per our need like changing core utilization ratio.
![4](https://user-images.githubusercontent.com/110470328/183159993-1a928aae-0448-4f66-aaec-bbd13a2f24df.JPG)
<br />**Fig06: Design Preparation**

![6](https://user-images.githubusercontent.com/110470328/183160100-be66463a-db0e-4e21-b244-fe0853efbcf5.JPG)
<br />**Fig07a: Design Folder contents after Design Preparation**

![7](https://user-images.githubusercontent.com/110470328/183160568-fdeb9a93-c955-46b2-a39d-864621d0e600.JPG)
<br />**Fig07b: Design Folder contents after Design Preparation**

![8](https://user-images.githubusercontent.com/110470328/183162209-20d6ca68-384c-41ae-801f-3aca9c41fbf6.JPG)
<br />**Fig07c: Design Folder contents after Design Preparation**

### Run Synthesis
Sythesis maps the RTL design into interconnection of logical gate which will perform the same functions as described using HDL(Hardware Description Language).
**_command: run_synthesis_** performs synthesis using YOSYS open-source synthesis tool.
![9](https://user-images.githubusercontent.com/110470328/183175439-81504ca7-1918-457c-ab8f-7d862c0438e1.JPG)
<br />**Fig08: run_synthesis completion**

#### Reports: 
1. 1-yosys_4.stat.rpt shows the information about cells usage, wires and chip area of the module.
![10](https://user-images.githubusercontent.com/110470328/183177835-7ba0cba4-3ec9-4d14-8e4e-703fe13ac6e5.JPG)
<br />**Fig09: Yosys Report of cells and wires used **

2. 2-opensta.timing.rpt has data like fanout, slew, delay, slack(WNS and TNS).
![14](https://user-images.githubusercontent.com/110470328/183186108-8a3df816-5c03-47b2-af9b-dd2a8f20659d.JPG)
<br />**Fig10: OpenSTA Report of Timing **


## Day2
### Floorplanning
We can set the following varialbles in **config.tcl**:
1. Core Utilization (FP_CORE_UTIL)
2. Die/Core Area
3. Aspect Ratio
4. Macros Placement
5. Configuration of I/O pins (random equidistant or on one side)

More floorplanning environment variables can be set and variables that can be set can be viewed in the /configuration/README.md file.
Below is the snippet of README.md
![15](https://user-images.githubusercontent.com/110470328/183286301-238086e6-371d-42ee-b5f2-3147d866c7f9.JPG)
<br />**Fig11: Directory Structure of Openlane Configuration **

Readme File contents is shown below-
![16](https://user-images.githubusercontent.com/110470328/183286444-dd078f57-47be-4e08-acd0-9198f0c6fcbf.JPG)
<br />**Fig12: Contents of README.md **

The defaults are already set in the **floorplan.tcl** file-
![17](https://user-images.githubusercontent.com/110470328/183286507-de6780be-e1e8-4ba7-ab98-578ce3164cf0.JPG)
<br />**Fig13: Contents of floorplan.tcl**

Floorplan is run using command **run_floorplan** . Alternatively we can also use the following commands in the updated version of OPENLANE.
init_floorplan 
place_io
global_placement_or
detailed_placement 
tap_decap_or
detailed_placement
gen_pdn
run_routing

![19](https://user-images.githubusercontent.com/110470328/183286743-5db0cb9f-e6ff-4255-91a2-feab730c63fc.JPG)
<br />**Fig14: "run_floorplan" **

After floorplan is completed ***floorplan.def*** is generated. DEF file conveys logical design data and physical design data.
![21 floorplan def file](https://user-images.githubusercontent.com/110470328/183286949-ffc045d6-90c8-40ba-afcf-61732fc7bd71.JPG)
<br />**Fig15a: Generated Floorplan DEF File  **

![22 floorplan def](https://user-images.githubusercontent.com/110470328/183287015-2176c7a0-b309-48c5-b0ee-e83beb7375e4.JPG)
<br />**Fig15b: Generated Floorplan DEF File  **


#### Viewing Floorplan in Magic Layout Tool
To view our floorplan in Magic we need to provide three files as input:
1. Technology file (sky130A.tech)
2. Def file (generated from floorplan step)
3. Merged LEF file (generated during design prep stage)
<br />  **_Command: magic -T \<tech file> lef read \<lef file> def read \<def file>_**
![23 cmd to open magic floorplan](https://user-images.githubusercontent.com/110470328/183287036-812c805e-1c9e-4a15-b2ab-7841b06daa17.JPG)
<br />**Fig16: Starting Magic Layout Tool **

![24 IO pins in magic](https://user-images.githubusercontent.com/110470328/183287068-55591de1-819d-4ff4-bd84-e1ad2228b277.JPG)
<br />**Fig17: Floorplan in Magic Layout Tool**

![25 layer of io pins magic](https://user-images.githubusercontent.com/110470328/183287117-2d7b522b-db92-4420-b5da-32d6e6749ea2.JPG)
<br />**Fig18: Metal Layer of I/O Pins**

In the updated version of Openlane Power Distribution Network is also laid out in Floorplan stage earlier it used to be before routing.
![18](https://user-images.githubusercontent.com/110470328/183287192-b1b414a4-bf88-490a-ad74-b99a502f8b73.JPG)
<br />**Fig19: Successful generation of PDN**

### Placement
After Floorplanning is done then Standard cells are laid out in the placement stage. The synthesized netlist has been mapped to standard cells and floorplanning phase has determined the standard cells rows, enabling placement. 
The placement can be run using **run_placement** command.
![27  run_placement](https://user-images.githubusercontent.com/110470328/183287522-9f2439c5-5bab-4cd1-8bd6-15b804b88d28.JPG)
<br />**Fig20: Placement successfully completed**

![28 Magic placement](https://user-images.githubusercontent.com/110470328/183288221-d558bdd4-3586-457a-8b67-f8e3f9707752.JPG)
<br />**Fig21: Placement in Magic**

![30 Placement zoomed in](https://user-images.githubusercontent.com/110470328/183288257-30deffbd-6834-46cb-ba8e-cc7ed04be320.JPG)
<br />**Fig22: Placement zoomed-in view**

![29 Placement directory](https://user-images.githubusercontent.com/110470328/183288244-d2e2f7df-1263-4d69-be05-50803499128f.JPG)
<br />**Fig23: Placement Results stored in placement directory**

### Standard Cell Characterization
1. Inputs - PDKs, DRC & LVS rules, SPICE models, library & user-defined specs.
2. Design Steps - Software GUNA (Characterization tool)
3. Outputs - Outputs are CDL, GDSII, LEF, extracted Spice netlist (.cir), & provide Timing (delays, slews), Power, Noise, function informataion in **.lib** format
<br />**==> .lib information is important for PD analysis.**

## Day 3
### Opening the custom inverter layout file in Magic
![31 opening std cell in magic](https://user-images.githubusercontent.com/110470328/183290513-48e2a34d-1279-49ae-807f-3abeed28e832.JPG)
<br />**Fig24: Opening inverter.mag file in Magic**

![32  inv layout in magic](https://user-images.githubusercontent.com/110470328/183288743-801d8c45-b013-4871-a2bc-e1789db777f6.JPG)
<br />**Fig25: Inverter Layout in Magic**

![33 inv nmos pmos in magic](https://user-images.githubusercontent.com/110470328/183288772-80eb3786-99ce-45db-a363-a9c34958ac70.JPG)
<br />**Fig26: NMOS & PMOS Layers in Magic**

Now we will need the spice file .cir from this layout for simulation and characterization in NGspice.
This can be done using following commands -
**_extract all
ext2spice cthresh 0 rthresh 0
ext2spice_**

![35 spice file ext2spice](https://user-images.githubusercontent.com/110470328/183289035-09e2c2fe-183d-4fcc-be69-9470e03fc58a.JPG)
<br />**Fig27: Extracting spice file from Magic**

The extracted spice netlist can be opened in a text editor and is shown below -
![36 Extracted spice netlist](https://user-images.githubusercontent.com/110470328/183290601-1a387f0f-e2ab-4077-97ac-df8ec123217a.JPG)
<br />**Fig28: Spice Netlist of Inverter**

To plot the output waveform of the spice deck we will use NGspice. The steps to run the simulation on ngpice are as follows:
1. Source the **.cir** spice deck file
2. Run the spice file by: run
3. Run: *setplot* → allows you to view any plots possible from the simulations specified in the spice deck
4. Select the simulation desired by entering the simulation name in the terminal
5. Run: display to see nodes available for plotting
6. Run: *plot  vs*  to obtain output waveform  

Trip Point or Switching Threshold is calculated where Vin=Vout.

### CMOS Inverter fabrication Process:
Inverter fabrication includes 16 MASK CMOS Process and steps are mentioned below-
16-Mask CMOS Process-
1. Selecting a substrate
2. Creating active regions for transistors
3. N-well and p-well formation
4. Formation of Gate
5. Lightly doped drain (LDD) formation
6. Source and Drain Formation
7. Steps to form connects and Interconnects
8. Higher level Metal Formation


Now coming to the simulation in ngspice.
**Note:** In spice, X subscript is used when we are invoking existing subckt (i.e., if we have **_.subckt nshort_model.0_** ).
Since here we are invoking nmos and pmos models directly, we need to change X0 to M0 and X1 to M1.
![40 Modified Spice Netlist](https://user-images.githubusercontent.com/110470328/183290908-5ad67cfd-8e1e-4c41-aca8-2a00b3647ad2.JPG)
<br />**Fig29: Spice Netlist of Inverter**

![37 Inverter cs](https://user-images.githubusercontent.com/110470328/183291126-90311615-4ebe-4552-a269-f177d5f1d568.JPG)
<br />**Fig30: Simulated Invertor Waveform**

Now we will find Input Rise and Output rise time which is time from 20% of max. value to 80% of max. value.
![38  rise time input](https://user-images.githubusercontent.com/110470328/183291173-7180b519-afe2-451c-9521-86a00cae6067.JPG)
<br />**Fig31: Input Rise Time**

![39 op rise time](https://user-images.githubusercontent.com/110470328/183291243-ed27a591-1d6b-4472-bbfe-a10403ea4166.JPG)
<br />**Fig32 Output Rise Time**

Input rise time = 59ps and Output rise time = 60.1ps


## Day 4
Magic can extract layout info in LEF format for PNR Tools. Given below are some rules which needs to be followed while doing layout of std.cell.
1. Width of Standard cell should be odd multiples of track horizontal pitch and Height should be odd multiples of track Vertical Pitch
2.Input and Output Ports are at the intersection of vertical and horizontal tracks.

**track.info** has following format:<br />
  **_\<metal layer> \<direction> \<offset> \<pitch>_** <br />
 Below is a snapshot of **tracks.info** file
![41 Tracks,info](https://user-images.githubusercontent.com/110470328/183291585-e24a36e4-bd4c-45ab-b502-7e42f0ee46f2.JPG)
<br />**Fig33 Contents of tracks.info**


















