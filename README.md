# OpenLane-VSD : The Journey 

## Introduction
OpenLane is an ASIC infrastructure library based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, KLayout and a number of custom scripts for design exploration and optimization.

A reference flow, "Classic", performs all ASIC implementation steps from RTL all the way down to GDSII.
A Process Design Kit (PDK) is a set of files used within the semiconductor industry to model a fabrication process for the design tools used to design an integrated circuit. Here we are using:  

![image](https://github.com/user-attachments/assets/b731135d-79bc-45c8-b96c-9bc06282ff1c)

Follow the official [OpenLane Documentation](https://openlane.readthedocs.io/en/latest/) to get started. You can discuss OpenLane 2 in the [#openlane-2](https://open-source-silicon.slack.com/archives/C05M85Q5GCF) channel of the [Efabless Open Source Silicon Slack](https://invite.skywater.tools/).


### The ASIC Design Flow

![Screenshot 2025-05-28 114823](https://github.com/user-attachments/assets/8a89d2f4-3fa4-471d-ad96-6e2b28a51b17)


## Try it out!
You can try OpenLane right in your browser, free-of-charge, using Google Colaboratory by following [this link](https://colab.research.google.com/github/efabless/openlane2/blob/main/notebook.ipynb).

***

## OpenLane Setup 
![Screenshot 2025-05-28 112344](https://github.com/user-attachments/assets/b0d86a27-c753-4c9b-b213-74a1a9e1a05f)

<pre>magic -T /Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &</pre>

![image](https://github.com/user-attachments/assets/019d217b-2fa7-4262-81ee-741a2803746a)

![image](https://github.com/user-attachments/assets/d61bc839-c071-4e93-9495-638d9bdb6004)
