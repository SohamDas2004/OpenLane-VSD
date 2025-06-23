![image](https://github.com/user-attachments/assets/80bdf86c-a6c9-4ac0-b673-3627d5f73d66)# OpenLane-VSD : The Journey 

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

## Motive
We will use the <b>PicoRV32</b> — a small and stable RISC-V core — as our RTL design for this backend flow.
<br>
Additionally, we will create a standard cell library that will be used to generate the netlist from the RTL code. We will analyze critical aspects of the design, such as hold time, setup time, and more.
<br> <br>
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

## Looking into Routing and Placement
<br>
The first step is to bind the netlist with physical cells i.e. cells with real dimension. These blocks are sourced from a "shelf", known as a library.The library has cells with various shapes, dimensions and also contains information about the delay information. The library contains various sizes of cells with the same functionality too - since bigger cells have lesser resistance.

The second step is PLACEMENT, which is done based on connectivity. As can be seen, flip flop 1 is close to the Din1 pin and flip flop 2 is close to Dout1 pin. Combinational cells are placed in close proximity to FF1 and FF2 as to reduce delay.

<img src="https://github.com/user-attachments/assets/b9c891b1-b1cd-4a83-98a5-3ac6d73c5865" alt="image" width="500" height="500">

<img src="https://github.com/user-attachments/assets/996359d4-387c-4742-9ed6-e4243a691403" alt="image" width="500" height="500">

## Viewing the floorplan and placement 
<br>

In the terminal, the below command is run to see the floorplan in Magic vlsi. Note that the placement is yet to be done.
<pre>magic -T /Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &</pre>


Below is the png image of the core after floorplan is done.

<img src="https://github.com/user-attachments/assets/b9542a1e-9815-4084-adfe-c332c954e853" alt="image" width="600" height="600">
<br>

***

Below is the post-placement png image:

<img src="https://github.com/user-attachments/assets/362d3511-fb0d-4bbd-b21d-3b9ec26e01d7" alt="image" width="600" height="600">


Below is the image of the same in Magic VLSI.

![image](https://github.com/user-attachments/assets/a455e846-ede5-420e-8c1f-db454d673b8f)


![image](https://github.com/user-attachments/assets/019d217b-2fa7-4262-81ee-741a2803746a)

![image](https://github.com/user-attachments/assets/d61bc839-c071-4e93-9495-638d9bdb6004)

<br>


## Typical Characterization Flow

The standard cell characterization process involves the following steps:

1. Reading SPICE model files containing device-level parameters.
2. Importing the netlist extracted from SPICE simulations.
3. Identifying buffer behavior to understand drive strength and signal propagation.
4. Parsing subcircuits defined in the SPICE hierarchy.
5. Connecting required power supplies (VDD and GND) to the circuit.
6. Applying input stimulus to simulate switching activity.
7. Adding appropriate output load capacitance to mimic real usage conditions.
8. Defining simulation control commands (e.g., transient or DC analysis).

The flow is typically driven by a configuration file fed into a characterization tool such as **GUNA**. The tool performs simulations and generates timing, power, and noise models, outputting them in `.lib` (Liberty) format.
<br>

## Installing Inverter layout
Here we are majorly focused on the inverter simulations. We alredy have the inverter ready. 
The repo is cloned into our system through this [link](https://github.com/nickson-jose/vsdstdcelldesign.git) in the below mentioned directory

<pre>/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane</pre>

Commands given: 
<pre>git clone https://github.com/nickson-jose/vsdstdcelldesign.git</pre>

Here we have the mag file of the inverter ready.

![image](https://github.com/user-attachments/assets/e445e921-4f03-4bed-991f-9e634625910a) <br>


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

Upon running command in ngspice:
<pre>plot y vs time a</pre>
The below graph is obtained and is the required <b>transient response of an inverter</b>.
<br>

![image](https://github.com/user-attachments/assets/8f30b181-29ed-4f83-aac0-a1bd4cfa727d)

<br>

## Looking into parameters
<br>

Here we will consider the below circuit of an inverter: <br>


<img src="https://github.com/user-attachments/assets/9460543d-310e-432f-b6e2-b3cd43360b2e" alt="image" width="500" height="300">

<br>

Rise transition: time taken by output wave to transit from a value of 20% of its max value to 80% of max value.
<br>

Fall transition: time taken by output wave to transit from a value of 80% of its max value to 20% of max value.
<br>

Fall Cell Delay: difference between time period when input has fallen to its 50% and output has risen to its 50%.
<br>

Rise Cell Delay: difference between time period when output has fallen to its 50% and input has risen to its 50%.
<br>


<img src="https://github.com/user-attachments/assets/8c2f8730-cf44-4239-af08-81b422215b12" alt="image" width="500" height="300">


Rise time obtained by the difference:
<br> 

![image](https://github.com/user-attachments/assets/d9ba8485-47c1-4e12-ba8b-55ba438562d9)
<br>
<br>

## The 16 Mask Process

| #  | Mask Name                         | Purpose                                                                 |
|----|----------------------------------|-------------------------------------------------------------------------|
| 1  | Active (Active Area or Diffusion) | Defines where diffusion (source/drain) can occur.                      |
| 2  | N-Well                            | Defines n-well areas for PMOS transistors.                             |
| 3  | P-Well                            | Defines p-well areas for NMOS transistors (in twin-tub process).       |
| 4  | Field Oxide (LOCOS/Isolation)     | Isolation regions via field oxide or STI.                              |
| 5  | Threshold Adjustment (VT)         | For selective threshold voltage implantations.                         |
| 6  | Channel Stop                      | Prevents leakage by doping field regions.                              |
| 7  | Poly (Gate)                       | Defines polysilicon gates of transistors.                              |
| 8  | Lightly Doped Drain (LDD) N+      | Forms lightly doped n-type extension for NMOS.                         |
| 9  | Lightly Doped Drain (LDD) P+      | Forms lightly doped p-type extension for PMOS.                         |
| 10 | Spacer Etch (Sidewall Spacers)    | Defines oxide spacers for self-alignment.                              |
| 11 | Source/Drain N+                   | Heavily doped n-type source/drain (NMOS).                              |
| 12 | Source/Drain P+                   | Heavily doped p-type source/drain (PMOS).                              |
| 13 | Contact                           | Defines contact holes between metal and active regions.                |
| 14 | Metal1                            | First layer of metal interconnect.                                     |
| 15 | Via1                              | Connects Metal1 to Metal2.                                             |
| 16 | Metal2                            | Second layer of metal interconnect.                                    |
<br>

<img src="https://github.com/user-attachments/assets/7f31a0f6-cd1b-4acc-80d4-3b2eb4ca559a" alt="image" width="700" height="500">

<br>

## Lab Introduction to Magic Tool Options and DRC Rules by Tim Edward

Link to Google_Skywaters Design Rules: - https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html

For reference , we can use the github repo of Google-Skywater: - https://github.com/google/skywater-pdk

One can read here also : - https://opencircuitdesign.com/magic Specefically section 2,6 of magic tutrial was mentioned.

for the lab we need to download the lab files, which can be done through this command -: wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz 

<br>


## Steps to configure OpenSTA for Post-synthesis Timing Analysis

We will give the following commmands in the terminal in openlane directory

<pre>prep -design picorv32a -tag 01-04_12-54 -overwrite

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

echo $::env(SYNTH_STRATEGY)

set ::env(SYNTH_STRATEGY) "DELAY 3"

echo $::env(SYNTH_BUFFERING)

echo $::env(SYNTH_SIZING)

set ::env(SYNTH_SIZING) 1

echo $::env(SYNTH_DRIVING_CELL)

run_synthesis  </pre>

<pre>prep -design picorv32a -tag 01-04_12-54 -overwrite </pre> is used to overwrite the existing files with previous values of simulations. After synthesis, we have observed that the slack is <b>negative</b>.

Now run_synthesis we will see chip area has incresed and the value of slack has reduced. Since synthesis of the picorv32a is successful, so we will run the floorplan using command run_floorplan
<br>

![image](https://github.com/user-attachments/assets/ea33c8ca-eac0-43b4-bbca-f8d2e8187223)

<br>
Since, we are getting the error so first again we have to do the synthesis using the commands mentioned earlier and then we will use following commands to do the floorplan,

<pre>init_floorplan

place_io

tap_decap_or </pre>

now we are good to run_placement. <br>

![image](https://github.com/user-attachments/assets/30f00330-228c-468d-bc39-c711c9acb489)

<br>  <b>Here placement is succesfull now without any error. </b>

![image](https://github.com/user-attachments/assets/b19a1a56-9d0a-4253-9310-e14f29ccf535)

<br>

## Lab steps to Optimize synthesis to reduce setup violations

<br>

Now we will change the FANOUT parameter and again do the synthesis,

<pre> prep -design picorv32a -tag 02-04_05-27 -overwrite

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

set ::env(SYNTH_SIZING) 1

set ::env(SYNTH_MAX_FANOUT) 4

echo $::env(SYNTH_DRIVING_CELL)

run_synthesis </pre>

<br>

![image](https://github.com/user-attachments/assets/16297016-7c07-46cb-a4d2-b92f13cb45b8)

<br>

## Clock Tree Routing and Buffering Using H-Tree Algorithm

<b>Clock tree synthesis:-</b> Let's connect clk1 to FF1 & FF2 of stage 1 and FF1 of stage 3 and FF2 of stage 4 with physical wire. <br>

![image](https://github.com/user-attachments/assets/124c5ee6-d3ff-498c-b6a8-c17a679cd95b)


Now let's see what is the problem with this? Let's consider some physical distance from clk to FF1 and FF2 , so due to this t2>t1.

Skew= t2-t1, and skew should be 0ps

![image](https://github.com/user-attachments/assets/cbb81d81-3f6f-4263-bade-4741ba1c0291)


Previously we have build bad tree now we will try to modify that in a smarter way. Hrre clk will come in somewhere mid points with this clk will reach to every flip flop at almost same time. In the same way we will connect the clk2 with flip flops like midpoint manner. <br>

![image](https://github.com/user-attachments/assets/9dbe14af-311a-4c90-9bad-92d3dbc265ff)


Now will se clock tree synthesis(Buffering), Let's we have some clock route through which it has to reach to particular locations and clock end points and in the path many capacitance, resitors are there. <br>

![image](https://github.com/user-attachments/assets/434ece13-961b-47ea-b876-959cdc42e21b)

<br>

Because of the wire length we did not get the same wave form at ouput as input and bcz of RC networks , so to resolve this problem we use repeaters. The only difference between the repeaters we use for clock or for data path is that clock repeaters repeaters will have equal rise and fall time. <br>

![image](https://github.com/user-attachments/assets/4ce8236a-21d6-4b71-a9b6-8489c98bd101)

<br>

<b>Clock Net Shielding:-</b> Till now we have built the clk tree in such a fashion that the skew between the launch flop and capture flop is 0. Skew means the latency difference between clk ports of the flop pins. Clk net shielding is the critical net scene in the design. We take the particular clk net and shield it means we protect the clk from the outside world, it's like house for tha clk. <br>

![image](https://github.com/user-attachments/assets/7354ef03-c191-4b4b-9c87-58f966413098)

<br>

***

## Routing and design rule check (DRC):  Intro to Maze routing using Lee's Algorithm

<b>Maze-Routing(Lee's Algorithm):-</b> Therse should not be zig-zag lines of connections most of the connections should be in L shape or in Z shape. So according to algorithm first it create some grids and grids are routing at the backend. It's called as routing grid. There are some numbers of grids on this routig having some dimensions. SO here we are having two points one is 'Source' and the other is 'Target'. With the help of this routing grid algorithm has to find out the best possible way between them.

First step is algorithm tries to lable all of the grids surrounded. Only the adjacent horizontal and vertical grids are labeled not the digonal one as shown in the image below. <br>

![image](https://github.com/user-attachments/assets/f820252b-42df-40d0-9c13-daef3dfe827b)

<br>

SO now there are so many ways to reach to target from source but we have to choose the best shortest possible way to reach the target.And we need to avoid the zig-zag way better to cghoose 'L' shape routing'. 

<br>

![image](https://github.com/user-attachments/assets/1837e8fd-7fc5-4ee8-85e0-8e0e65ad4b0d)

<br>

![image](https://github.com/user-attachments/assets/6c34030c-6a00-4812-807f-1fb5330540fa)

<br>

## Design Rule Check

Rule 1) Wire width:- Width of the wire should be minimum that derived from the optical wavelenth of lithography technique applied.

![image](https://github.com/user-attachments/assets/93d00a4f-88d2-493f-8873-aebe82bae1d5)

Rule 2) Wire Pitch:- The minimum pitch between two wire should be this much as shown in the figure below.

![image](https://github.com/user-attachments/assets/83435949-aeb8-452b-84cf-828bec404e5a)

Rule 3) Wire Spacing:- The wire spacing between two wires should be as shown in the image below.

![image](https://github.com/user-attachments/assets/efd60d6c-ce7c-4392-aa71-f5d36ee58f83)

<br>

![image](https://github.com/user-attachments/assets/46a13f38-757c-4421-826c-09256df0435d)

<br>

Rule 1) Via Width:- via width should be some minimum value. <br>
Rule 2) Via Spacing:- Via spacing should be minimum value. <br>

After routing and DRC the next step is Parasitic extraction. Resistance and capacitance present on every wire should be extracted and use for further process.







## Conclusion 

This documentation serves as a comprehensive walkthrough of the digital ASIC design backend flow using open-source tools and the Sky130 PDK. From RTL to GDSII, each stage has been detailed with supporting visuals, real tool outputs, and practical insights.
<br>

 ## Key Takeaways

- Understanding of open-source RTL and EDA tools like Yosys, OpenROAD, and Magic.
- Hands-on application of the OpenLane flow for physical design automation.
- Deep insights into timing analysis, power planning, antenna effects, and routing challenges.
- Exposure to the SkyWater 130nm process and characterization fundamentals.
<br>

##  References
Special thanks to VSD, OpenLane, SkyWater, and the entire open-source hardware community for enabling such accessible learning platforms.<br>
>https://www.vlsisystemdesign.com/digital-vlsi-soc-design-and-planning/















