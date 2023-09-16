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

</details>

<details>
<summary>DAY 2 : Good Floorplan vs Bad Floorplan and Introduction to library cells</summary>
<br>

## Chip Floorplanning Considerations

### 1. Define Width and height of core and die

- ```Die``` : Structure that consists of core which is a small semiconductor material on which the fundamental circuit is fabricated.
- ```core``` : Structire that contains primary logic and functional components.
UTILISATION FACTOR = Area Occupied by the Netlist / Area of the core (usually 50%-70%)
ASPECT RATIO = Height / Width (1 = square, others = rectangle)

### 2. Define Location of Pre-Placed cells

```pre-placed cells``` : memories, clock gating cells, comparator, mux etc

- The arrangement of these IPs on chip is called FLOORPLANNING
- These IPs have user defined locations and hence are placed in chip before automated placement and routing. Therefore called pre-placed cells.
- Automated PnR tool places the remaining logical cells in design onto chip.

  ### 3. De-coupling capacitors

We know that all the combinational blocks are connected to Vdd and Vss for their operation. But when there is a large circuit with many resistors, then The capacitors in the logic might not get fully charged as there occurs voltage deop due to wire metal and the resistors present along the path. So after voltage drop, if the voltage obtained by the logic is within noise margin, then it works well but what if it doesn't? 

We use De-Coupling capacitors (A huge capacitance with voltage equal to that of supply voltage) that is placed close to the combinational logic. When the switching activity takes place, it detatches the circuit from main supply and this capacitor acts as power supply.

### 4. Power Planning

- Power planning during the Floorplanning phase is essential to lower noise in digital circuits attributed to voltage droop and ground bounce. Coupling capacitance is formed between interconnect wires and the substrate which needs to be charged or discharged to represent either logic 1 or logic 0.
- When a transition occurs on a net, charge associated with coupling capacitors may be dumped to ground. If there are not enough ground taps charge will accumulate at the tap and the ground line will act like a large resistor, raising the ground voltage and lowering our noise margin. To bypass this problem a robust PDN with many power strap taps are needed to lower the resistance associated with the PDN.

### 5. Pin Placement

- ```Pin placement``` is an essential part of floorplanning to minimize buffering and improve power consumption and timing delays.
- We usually place input pins on the left and output pins on the right
- for primary inputs and outputs, pin size may be small and for clock, the pin size would be large because clock should drive many cells so we need to make sure that the resistance is less.
- larger the area, lesser the resistance.
- ```Placement blockage``` is done inorder to makesure that no logic is placed along the area where the pin placement is carried out.

## Floorplan
```run_floorplan```

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/d044dd49-28d1-4992-bc0d-e473c3b7a06f)

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/ff0a7882-1f2a-413c-a3cf-4352dbc787ff)

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/29f59e29-47c9-4fb2-a9b2-e365285d07a5)

Changes made in the config.tcl for floorplan purpose:

![image](https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/77f891bd-89f3-40f1-a64b-91ffdb993378)

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/7daa74fc-223b-42ca-8ab7-1f2c88dde73a)

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/045e37b6-263e-4ba4-b54f-4f55929cbd91)

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/d520141c-e2d2-4b56-86a5-085a75380544)

## Library Binding and Placement

### 1. Bind the netlist with physical cells

- ```Library``` consists of cells, sizes of cells, various flavours and shapes of the cells, Timing, Power and delay information.
- Now, we have the floorplan, netlist and representation of components of netlist in library
- place all the components such that the timing is not disturbed and distribute them properly. 


### 2. Optimize Placement

- Some components may be located very far to their inputs which can disturb signal integrity (as wire length increases, RC value increases). Therefore we use repeaters(may be series of buffers) inorder to avoid signal loss but area loss comes into picture.
- Assuming that all the clock signals are working at ideal rate, we do the timing analysis if the current placement works good.

### 3. Placement

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/56ee6522-5591-4d69-a6dc-2209a2e79c7c)

## Cell Design Flow

Cell design is done in 3 parts:

1. **Inputs** - PDKs (Process design kits), DRC & LVS rules, SPICE models, library & user-defined specs.
2. **Design Steps** - Design steps of cell design involves Circuit Design, Layout Design, Characterization. The software GUNA used for characterization. The characterization can be classified as Timing characterization, Power characterization and Noise characterization.
3. **Outputs** - Outputs of the Design are CDL (Circuit Description Language), GDSII, LEF, extracted Spice netlist (.cir), timing, noise, power.libs, function.

### General Timing characterization parameters

#### Timing threshold definitions

- ```slew_low_rise_thr``` - 20% from bottom power supply when the signal is rising
- ```slew_high_rise_thr``` - 20% from top power supply when the signal is rising
- ```slew_low_fall_thr``` - 20% from bottom power supply when the signal is falling
- ```slew_high_fall_thr``` - 20% from top power supply when the signal is falling
- ```in_rise_thr``` - 50% point on the rising edge of input
- ```in_fall_thr``` - 50% point on the falling edge of input
- ```out_rise_thr``` - 50% point on the rising edge of ouput
- ```out_fall_thr``` - 50% point on the falling edge of ouput

</details>

<details>
<summary>DAY 3 :  Design library cell using magic layout and ngspice characterisation </summary>
<br>

## SPICE Deck creation for CMOS Inverter

SPICE deck contains the information of netlist such as:
- Connectivity Information
- Component values
- 'Nodes' identified
- 'Node' names

![image](https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/fde8c66e-6547-49a2-bdad-478c812d5419)

### [CMOS_INVERTER.cir]()

```
*** MODEL DESCRIPTIONS ***
*** NETLIST DESCRIPTION ***
M1 out in vdd vdd pmos W=0.375u L=0.25u
M2 out in 0 0 nmos W=0.375u L=0.25u

cload out 0 10f

Vdd vdd 0 2.5
Vin in 0 2.5
*** SIMULATION Commands ***

.op
.dc Vin 0 2.5 0.05
*** include tsmc_025um_model.mod ***
.LIB "tsmc_025um_models.mod" CMOS_MODELS
.end
```

SPICE Simulation steps
```
cd <folder where the .cir file is present>
source CMOS_INVERTER.cir
run
setplot
dc1
display
plot out vs in
```

Observe the output. It should be symmetric ie., the threshold voltage should be at vdd/2 if it isnt, try to increase the PMOS width and run the simulation again. One of the important parameters tthat defines the **ROBUSTNESS** of the CMOS is ```Switching Threshold (Vm)``` @Vm : Vin = Vout

## Fabrication Process for a CMOS Inverter

Fabrication of CMOS Inverter is a 16-Mask process

### 1. Selecting the substrate 

- P-Type substrate with resistivity around (5-50 ohm) doping level (10^15 cm^-3) and orientation (100).
- Note that substrate doping should be less than well doping (used to fabricate NMOS and PMOS)

### 2. Create active resistance

This step creates pockets for NMOS and PMOS
1. Grow SiO2(~40nm) on Psub
2. deposit ~80nm Si3N4 on SiO2
3. deposit 1um layer of photoresist(used to define regions)
4. photolithography
5. etch out Si3N4 and SiO2 using a suitable solvent
6. Place the obtained structure in oxidation furnace due to which field oxide is grown.This process is called ```LOCOS``` that is ```Local oxidation of silicon```
7. Etch out Si3N4 using hot phosphoric acid

### 3.NWell and PWell formation
### 4. Formation of Gate
### 5. Light Doped Drain Formation(LDD Formation)
### 6. Source and Drain Formation
### 7. Steps to form contacts and interconnects
### 8. Higher level metal formation
### 9. Final STructure

![image](https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/0e355a75-55ff-4723-96ae-4abd5845697c)

## Inverter Layout using Magic

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/d0cffd03-5ee1-4432-a99a-b377159dcf43)

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/d2dd204d-d8ec-46c4-a00c-5edfc15a30f9)

- select a region from the layout, go to the console and type ```what``` to display the information of selected area
- To select a region, place ```cursor``` on that point and  press```s```. More the number of times you press ```s```, higher the abstraction selected.

### DRC Check

To check for DRC Errors, select a region (left click for starting point, right click at end point) and see the DRC column at the top that shows how many DRC errors are present.The Details of DRC Errors will be printed on the console.

![image](https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/eebc0109-4408-40fa-a18e-ead67419cfa7)

## Extracting PEX to SPICE with MAGIC

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/bd359b3a-ed51-4879-9c5a-c810d4db8be0)

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/0c035a8e-513c-46fa-8cb4-90ef0daa0cc2)

The above file has details of inverter netlist but the sources and their values are not specified. So we have to modify the file.

- Grid size from the layout is 0.01u
- specify the library for MOS
- create VDD, VSS, Input pulse Va
- specify the type of analysis to be done

### Grid Size

![image](https://github.com/yagnavivek/PES_OpenLane_PD/assets/93475824/1fd94afe-3a40-4269-92ed-d7c46f248417)

## Modified Spice netlist

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/590671e9-5cde-4b52-82ee-fcf544c5834e)

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/9e19ba43-8143-4d95-b215-56ae1fdbd45f)

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/8bfac9c0-622c-4102-8fc5-30c90c750e46)

![image](https://github.com/kushal2710/pes_openlane_pd/assets/115935208/6a7badb7-5bef-47cd-b3c6-3f76292cc1fd)

The results obtained from the graph are :
- Rise Transition : 0.0395ns
- Fall transition : 0.0282ns
- Cell Rise delay : 0.03598ns
- Cell fall delay : 0.0483ns























