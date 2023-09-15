# Advanced Physical Design using OpenLANE/Sky130 

## IC Package 

<img width="600" height="400" alt="image" src="https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/96055996-9ea4-4f61-b9fb-295efe5b1bbb">

The most critical component of the IC is the ```chip``` because it contains the actual functionality of the device and IO pads helps in exchange of signals with the chip.

## CHIP

![image](https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/2d811fbe-7f4d-4dae-8bd5-e1d26c7df813)

## Application Specific Integrated Circuit (ASIC)

To develop an ASIC, We mainly require 3 components
- RTL IPs 
- PDK data 
- EDA Tools 

1. **```RTL```** : It is a level of abstraction in digital design and electronics where a system's behavior is described in terms of registers, logic gates, and the flow of data between them.
2. **```PDK```** : Process Design Kit is a collection of files, libraries, and documentation provided by semiconductor foundries to assist designers in creating integrated circuits (ICs) that are compatible with a specific manufacturing process. PDKs include information on design rules, device models, and technology files required for designing and simulating ICs.
3. **```EDA```** : Electronic Design Automation tools are software tools used for designing and testing electronic systems, including ICs and PCBs.

## RTL2GDSII FLow (simplified)

- ```synthesis```
- ```Floorplanning```
- ```Powerplanning```
- ```Placement```
- ```Clock Tree Synthesis```
- ```Routing```
- ```Signoff```

## OpenLane

![image](https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/68749aa4-dd1a-401e-b7a3-0c74df581e8e)

# COURSE

<details>
<summary>DAY 1 : Inception of opensource-EDA, Opennlane and Skywater130</summary>
<br>

## Skywater-130 PDK

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/d4fbdf50-15ec-4089-b194-c179a9775755)

The Skywater PDK files we are working with are described under $PDK_ROOT
1. Skywater-pdk – Contains all the foundry provided PDK related files
2. Open_pdks – Contains scripts that are used to bridge the gap between closed-source and open-source PDK to EDA tool compatibility
3. Sky130A – The open-source compatible PDK files

## Invoking OpenLane

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/a33747e1-b6bc-4dc6-86dd-08403d32c735)

## Importing package

Different software dependencies are needed to run OpenLANE. To import these into the OpenLANE tool we need to run: ```package require openlane 0.9```

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/7eb49a11-f9bd-4d37-b257-1077d4597ab2)

## Designs present in openlane and Hierarchy in a Design

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/cf547543-427f-4134-a03e-fb8d8212d462)

- ```Config.tcl files``` - Design specific configuration switches used by OpenLANE

## Config file example content

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/264ba01e-7a14-4af7-b65e-43994439c187)

## Prepare the design for the flow 

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/d2f3d841-c133-4e78-8c9f-c333425c27eb)

Once the design prep stage is done, it creates a runs directory where all the results will be stored

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/1f5cce5a-7719-4e0e-bdee-a057f0ed16fd)

## Synthesis

```run_synthesis```

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/2dad83b8-73c8-4d8b-9ca3-164180b334b8)

****The main task to do at the beginning stage is to find the flop ration ie., (No. of D flip flops / Total number of cells)****

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/5a4308a6-d271-4cb1-97df-06296752f908)






