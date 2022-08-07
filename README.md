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
<br />**Fig13: Contents of floorplan.tcl **

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
<br />**Fig17: Metal Layer of I/O Pins**












