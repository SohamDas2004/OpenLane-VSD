# OpenLane-VSD : The Journey 

## Introduction
OpenLane is an ASIC infrastructure library based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, KLayout and a number of custom scripts for design exploration and optimization.

A reference flow, "Classic", performs all ASIC implementation steps from RTL all the way down to GDSII.
A Process Design Kit (PDK) is a set of files used within the semiconductor industry to model a fabrication process for the design tools used to design an integrated circuit. Here we are using:  

![image](https://github.com/user-attachments/assets/b731135d-79bc-45c8-b96c-9bc06282ff1c)

The [Google SkyWater 130 PDK](https://github.com/google/skywater-pdk.git) is an open-source toolkit for designing computer chips at the 130nm scale. Developed by Google and SkyWater Technology, it makes chip design accessible to everyone, from students to researchers, and supports innovation by providing essential design resources and compatibility with various design tools.

Follow the official [OpenLane Documentation](https://openlane.readthedocs.io/en/latest/) to get started. You can discuss OpenLane 2 in the [#openlane-2](https://open-source-silicon.slack.com/archives/C05M85Q5GCF) channel of the [Efabless Open Source Silicon Slack](https://invite.skywater.tools/).

<br>

## Openlane flow

![image](https://github.com/user-attachments/assets/87382829-4e6c-4cd7-b2b2-b607e69be934)
 <br>

## Overview of Physical Design flow

Place and Route (PnR) is the core of any ASIC implementation and Openlane flow integrates into it several key open source tools which perform each of the respective stages of PnR. Below are the stages and the respective tools (in ( )) that are called by openlane for the functionalities as described:

- **Synthesis**
  - Generating gate-level netlist ([yosys](https://github.com/YosysHQ/yosys)).
  - Performing cell mapping ([abc](https://github.com/berkeley-abc/abc)).
  - Performing pre-layout STA ([OpenSTA](https://github.com/The-OpenROAD-Project/OpenSTA)).

- **Floorplanning**
  - Defining the core area for the macro as well as the cell sites and the tracks ([init_fp](https://github.com/The-OpenROAD-Project/OpenLane/blob/master/script/init_fp.tcl)).
  - Placing the macro input and output ports ([ioPlacer](https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/ioPlacer)).
  - Generating the power distribution network ([pdn](https://github.com/The-OpenROAD-Project/OpenLane/blob/master/script/gen_pdn.tcl)).

- **Placement**
  - Performing global placement ([RePlAce](https://github.com/The-OpenROAD-Project/RePlAce)).
  - Performing detailed placement to legalize the globally placed components ([OpenDP](https://github.com/The-OpenROAD-Project/OpenDP)).

- **Clock Tree Synthesis (CTS)**
  - Synthesizing the clock tree ([TritonCTS](https://github.com/The-OpenROAD-Project/TritonCTS)).

- **Routing**
  - Performing global routing to generate a guide file for the detailed router ([FastRoute](https://github.com/The-OpenROAD-Project/FastRoute)).
  - Performing detailed routing ([TritonRoute](https://github.com/The-OpenROAD-Project/TritonRoute)).

- **GDSII Generation**
  - Streaming out the final GDSII layout file from the routed DEF ([Magic](http://opencircuitdesign.com/magic/)).

<br>

## Try it out!
You can try OpenLane right in your browser, free-of-charge, using Google Colaboratory by following [this link](https://colab.research.google.com/github/efabless/openlane2/blob/main/notebook.ipynb).

***

<br>

## OpenLane Setup 

![Screenshot 2025-05-28 112344](https://github.com/user-attachments/assets/b0d86a27-c753-4c9b-b213-74a1a9e1a05f)

<br>

OpenLane is <b>successfully</b> run.

![Screenshot 2025-05-28 112820](https://github.com/user-attachments/assets/467fc065-e851-4514-91c8-5dc5a65ac4de)

<br>

## Tech File Exploration

Now we will delve into some of the deafult configurations which skywater130nm pdk provides.
For example, below are the default settings found in the openlane configurations.

![Screenshot 2025-05-28 121521](https://github.com/user-attachments/assets/3939eb04-88b9-476d-88e5-1224e49990ec)


Here are the defaults for floorplan found in floorplan.tcl file

![image](https://github.com/user-attachments/assets/972067fa-c4a6-4f3f-bbd1-ea0cca75dbfb)
<br>

## Finding Flop ratio
The general rule of thumb is: dff_count / total_number_of_cells <br>
We have done it for our case here:<br>
![image](https://github.com/user-attachments/assets/93904a21-dc3a-4c8e-88a3-78bfbfa3a12f)

<br>
At the very end the chip area is also observed:<br>

![image](https://github.com/user-attachments/assets/846f3d05-e843-443b-bb81-07e383703d01)



## Viewing the floorplan and placement 
<br>

In the terminal, the below command is run to see the floorplan in Magic vlsi. Note that the placement is yet to be done.
<pre>magic -T /Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &</pre>


Below is the png image of the core after floorplan is done.

![image](https://github.com/user-attachments/assets/b9542a1e-9815-4084-adfe-c332c954e853)


***

Below is the post-placement png image:

![image](https://github.com/user-attachments/assets/362d3511-fb0d-4bbd-b21d-3b9ec26e01d7)


Below is the image of the same in Magic VLSI.

![image](https://github.com/user-attachments/assets/a455e846-ede5-420e-8c1f-db454d673b8f)


![image](https://github.com/user-attachments/assets/019d217b-2fa7-4262-81ee-741a2803746a)

![image](https://github.com/user-attachments/assets/d61bc839-c071-4e93-9495-638d9bdb6004)


***
<br>

## Installing Inverter layout
Here we are majorly focused on the inverter simulations. We alredy have the inverter ready. 
The repo is cloned into our system through this [link](https://github.com/nickson-jose/vsdstdcelldesign.git) in the below mentioned directory

<pre>/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane</pre>

Commands given: 
<pre>git clone https://github.com/nickson-jose/vsdstdcelldesign.git</pre>

Here we have the mag file of the inverter ready.

![image](https://github.com/user-attachments/assets/e445e921-4f03-4bed-991f-9e634625910a)


Now the final inverter layout is observed. 

![image](https://github.com/user-attachments/assets/85706177-50d9-4114-9879-73b2e93eed77)
<br>
![image](https://github.com/user-attachments/assets/21380a57-8f11-43f7-ad9d-796a4962d67b)
<br> 

## Ngspice Simulations

It is to be noted that ngspice was needed to be installed in the specified directory. 
<pre>~/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign</pre>
<br>
Command in terminal:
<pre>sudo apt install ngspice</pre>
<br>
sky130_inv.spice file was modified as below:
<br>

![image](https://github.com/user-attachments/assets/dd2da34d-f9d1-4574-ba66-28692f27ffd3)
<br>

Simulation setup is done and well running.
<br>
![image](https://github.com/user-attachments/assets/f924d91b-d621-4b80-ba11-4aa0ef89cad3)
<br>
<br>
Now we can plot various graphs.
<br>


