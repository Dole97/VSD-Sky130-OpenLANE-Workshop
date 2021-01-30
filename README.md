# VSD-Sky130-OpenLANE-Workshop
Attended a 5-Day workshop on [Advanced Physical Design using OpenLANE/Sky130](https://www.vlsisystemdesign.com/advanced-physical-design-using-openlane-sky130/) conducted by [VLSI System Design](https://www.vlsisystemdesign.com/). The workshop introduced us to opensource EDA tools and PDKs and how are they used in the semiconductor industry. We performed the synthesis, floorplan, placement, clock tree synthesis and routing in openlane tool. We also designed and characterized a library cell in ngspice.

# Prerequisites
Installed and ran OpenLANE on Ubuntu OS. It is recommended to allocate 4GB RAM and 40GB Disk Space to OS for smooth functioning. Windows user can use Oracle VM VirtualBox to run the OS. For further information about installation of OpenLANE and open-source EDA tools, refer to the following two videos on Udemy by VSD.

[A complete to install OpenLANE and Sky130nm PDK](https://www.udemy.com/share/103wqAAEESeVZUR3QF/)

[A complete to install open-source EDA tools](https://www.udemy.com/share/101skKAEESeVZUR3QF/)


# Day 1: Introduction to open-source EDA, OpenLANE and Sky130 PDK

# Skywater PDK Files

The Skywater PDK Files we are working on are described under $PDK_ROOT. There are 3 subdirectories required for workshop.

1. Skywater-pdk – Contains all the foundry provided PDK related files
2. Open_pdks – Contains scripts that are used to bridge the gap between closed-source and open-source PDK to EDA tool compatibility
3. Sky130A – The open-source compatible PDK files

# Launching OpenLANE

Navigate to the folder where flow.tcl file is located by typing 'cd <path_to_flow.tcl>' in terminal.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%201/VSD%20LAB-launching%20openlane.PNG)

Then type in the command './flow.tcl -interactive' to run openLANE flow interactively. Once the openLANE starts, type '% package require openlane 0.9' to import the necessary software dependencies required.

# Preparing the design

The command 'prep -design <design_name>' prepares the file structure for our design. In our case, the design_name=picorv32a.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%201/prep%20-design.PNG)

The 'prep -design <design_name> -tag <tag_name>' command enables us to create a custom folder of given tag_name.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%202/prep%20design%20tag.PNG)

'prep -design <design_name> -tag <tag_name> -overwrite' command can overwrite a parameter in design file.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%202/overwrite%20cmd.PNG)

echo $env(CLOCK_PERIOD) command shows us the existing clock period of design. set $env(CLOCK_PERIOD) enables us to edit and change the existing value of clock period. You can overwrite value of clock period after running above command 'prep -design <design_name> -tag <tag_name> -overwrite'.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%202/change%20clock%20period.PNG)


# Running Synthesis

The '% run_synthesis' command runs the synthesis of design in openlane.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%201/run_synthesis.PNG)

The console notifies us after the synthesis is completed successfully.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%201/synthesis%20successful.PNG)

The synthesis file picorv32a.synthesis.v (present in the designs/picorv32a/runs/<date_of_run>/results/synthesis folder) should look like this.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%201/picorv32a%20synthesis%20.v%20file.PNG)




# Day 2: Floorplan considerations and introduction to library cell

# Run Floorplan

After synthesis, the next step is to run the floorplan by entering the command '% run_floorplan'. 

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%202/run%20floorplan.PNG)

This is the lef file of our design generated.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%202/picorv32a%20lef%20file.PNG)

Syntax for viewing floorplan in magic : magic -T <magic tech file> lef read <lef file> def read <def file>
  
![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%202/cmd%20to%20open%20floorplan%20in%20magic.PNG)  

We need Magic technology file i.e sky130A.tech, merged lef file and def file of floorplan to view floorplan in magic tool. 

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%202/floorplan%20view%20in%20magic.PNG)

Press s followed by v to select and view the whole design.

# Run Placement

After floorplan, the next stage is placement. Run the placement by entering command '% run_placement'. Running placement is completed successfully.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%202/run%20placement%20complete.PNG)

Syntax to open placement in magic tool : magic -T <magic tech file> lef read <lef file> def read <def file>
  
![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%202/cmd%20to%20open%20placement%20in%20magic%20tool.PNG)

This is the view of placement in magic tool.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%202/placement%20view%20in%20magic.PNG)



# Day 3: Design and characterization of one library cell using Magic Layout tool and ngspice
# Day 4: Pre-layout Timing Analysis and Clock Tree Synthesis
# Day 5: Final steps for RTL2GDS 
