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

### Run Synthesis
Sythesis maps the RTL design into interconnection of logical gate which will perform the same functions as described using HDL(Hardware Description Language)


