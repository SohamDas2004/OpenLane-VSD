# OpenLane-VSD : The Journey 

## Introduction
OpenLane is an ASIC infrastructure library based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, KLayout and a number of custom scripts for design exploration and optimization.

A reference flow, "Classic", performs all ASIC implementation steps from RTL all the way down to GDSII.
A Process Design Kit (PDK) is a set of files used within the semiconductor industry to model a fabrication process for the design tools used to design an integrated circuit. Here we are using:  

![image](https://github.com/user-attachments/assets/b731135d-79bc-45c8-b96c-9bc06282ff1c)

Follow the official [OpenLane Documentation](https://openlane.readthedocs.io/en/latest/) to get started. You can discuss OpenLane 2 in the [#openlane-2](https://open-source-silicon.slack.com/archives/C05M85Q5GCF) channel of the [Efabless Open Source Silicon Slack](https://invite.skywater.tools/).

###The OpenLane Infrastructure
timeline
  title The OpenLane Infrastructure
  RTL to Netlist
    : Linting / Verilator
    : Power Distribution Network Hierarchy / Yosys
    : Synthesis / Yosys
    : Synthesis / Design Compiler (with proprietary plugin)
    : Multi-corner Netlist STA / OpenSTA
  Floorplanning
    : Floorplan Initialization / OpenROAD
    : Manual Macro Placement / OpenDB
    : Tap/Endcap Insertion / OpenROAD
    : PDN Generation / OpenROAD
  Placement
    : Pin Placement (from config file) / OpenROAD, OpenDB
    : Pin Placement (Random/Matching/Annealing) / OpenROAD
    : Pin Placement (from template DEF) / OpenDB
    : Global Placement / OpenROAD
    : Resizer Design Repair (Post-GPL) / OpenROAD
    : Detailed Placement / OpenROAD
  Clock Tree Synthesis
    : Clock-Tree Synthesis / OpenROAD
    : Resizer Timing Repair (Post-CTS) / OpenROAD
  Routing
    : Global Routing / OpenROAD
    : Resizer Design Repair (Post-GRT) / OpenROAD
    : Diode Insertion on Ports / OpenDB
    : Heuristic Diode Insertion / OpenDB
    : Antenna Repair / OpenROAD
    : Resizer Timing Repair (Post-GRT) / OpenROAD
    : Detailed Routing / OpenROAD
    : Row Filling / OpenROAD
  Signoff (Timing)
    : Parasitics Extraction / OpenROAD
    : Multi-corner Static Timing Analysis / OpenSTA
    : SI-Enabled Multi-corner Static Timing Analysis / PrimeTime (with proprietary plugin)
  Signoff (Physical)
    : GDSII Stream-Out / Magic
    : GDSII Stream-Out / KLayout
    : Magic vs. KLayout Stream XOR / KLayout
    : Design Rule Checks / Magic
    : Design Rule Checks / KLayout
    : Spice Extraction / Magic
    : Layout vs. Schematic / Netgen
    : Equivalence Check (Alpha) / Yosys EQY

## Try it out!
You can try OpenLane right in your browser, free-of-charge, using Google Colaboratory by following [this link](https://colab.research.google.com/github/efabless/openlane2/blob/main/notebook.ipynb).

## OpenLane Setup 
![Screenshot 2025-05-28 112344](https://github.com/user-attachments/assets/b0d86a27-c753-4c9b-b213-74a1a9e1a05f)

![image](https://github.com/user-attachments/assets/019d217b-2fa7-4262-81ee-741a2803746a)

![image](https://github.com/user-attachments/assets/d61bc839-c071-4e93-9495-638d9bdb6004)
