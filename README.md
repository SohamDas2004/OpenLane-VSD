# OpenLane-VSD
OpenLane is an ASIC infrastructure library based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, KLayout and a number of custom scripts for design exploration and optimization.

A reference flow, "Classic", performs all ASIC implementation steps from RTL all the way down to GDSII.

You can find the documentation here to get started. You can discuss OpenLane 2 in the #openlane-2 channel of the Efabless Open Source Silicon Slack.

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


## OpenLane Setup
![image](https://github.com/user-attachments/assets/9efda392-6d6b-4ebd-9862-26f2e8627c21)
