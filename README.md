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

# Magic tool file of CMOS inverter

We worked on a simple CMOS inverter predesigned in magic. We worked on the .mag file for the inverter mentioned which was available in @nickson-jose's github repository titled "vsdstdcelldesign".

In the openlane_working_dir/openLANE_flow directory, clone the said repository using the command:

git clone https://github.com/nickson-jose/vsdstdcelldesign

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/cloning%20git%20repo.PNG)

These are the files inside vsdstdcelldesign folder.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/files%20inside%20vsdstdcelldesign.PNG)

We copy sky130A.tech file from pdks folder to vsdstdcelldesign folder by navigating to pdks folder (refer the following image) and typing the command : cp sky130A.tech /Desktop/work/tools/openlane_working_dir/openLANE_flow/vsdstdcelldesign

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/cmd%20to%20copy%20skytech130A%20file.PNG)

Check whether the tech file is copied to vsdstdcelldesign folder

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/sky130A%20tech%20file%20saved%20to%20vsdstdcelldesign.PNG)

# Viewing the cell in magic tool

Then we open the cell design in magic tool by the following command : magic -T sky130A.tech sky130A_inv.mag &

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/cmd%20to%20open%20cell%20in%20magic%20tool.PNG)

This is the view of our cell in magic tool.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/std%20cell%20layout%20in%20magic%20tool.PNG)

# % what command 
Take cursor on a particular part of the cell and press 's' to select that part. Then type '% what' and press enter in tkcon terminal of magic tool to obtain more information about that part.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/what%20cmd%20in%20tkcon.PNG)

# Extracting parasatic spice file
In the tkcon terminal, enter the following 3 commands in order given below :

% extract all
% ext2spice cthresh 0 rthresh 0
% ext2spice 

To extract the parasitic spice file for the associated layout one needs to create an extraction file. After generating the extracted file we need to output the .ext file to a spice file.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/ext2spice.PNG)

In the directory vsdstdcelldesign, a '.spice' file will be created.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/sky130_inv.spice.PNG)

# Configuring ngspice file

Once we have the .spice file available in the directory, we can perform the necessary configurations and changes using an editor.

There are some changes we need to make in the .spice file (sky130A_inv.spice) to prepare it for further stages of the OpenLANE flow. They are:

include the pshort and nshort libraries - .include ./libs/phsort.lib & .include./libs/nshort.lib
pshort & nshort are replaced with pshort_model.0 and nshort_model.0 respectively.
We need to define some ports which we make use of in the design - VDD, VSS, Va.
Va A GND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns) - This implies a source Va, between A and GND, whose waveform is defined as a PULSE function with min value = 0V and max value = 3.3V
.tran 1n 20n - transient sweep from 1ns to 20 ns
.option scale=0.01u
These changes together should reflect in the spice file as follows:-

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/sky130%20inv%20spice%20file%20edit.PNG)

# Command to plot waveform in ngspice

Enter this command to view output vs time plot : % plot y vs time a

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/plot%20cmd%20in%20ngspice.PNG)

The following plot is shown:

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/spice%20plot%20y%20vs%20a.PNG)

# Characterization of the cell

To characterize the cell, we have determined values of the following parameters:

1. Rise Cell Delay - It is the propagation delay of signal when it propagates from input to output. It is calculated by taking difference of x - co-ordinates at (50% of peak magnitude of rising output waveform) - (50% of peak magnitude of falling input waveform).

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/rise%20delay.PNG)

2. Fall Cell Delay - It is the propagation delay of signal when it propagates from input to output. It is calculated by taking difference of x - co-ordinates at (50% of peak magnitude of falling output waveform) - (50% of peak magnitude of rising input waveform).

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/fall%20delay.PNG)

3. Rise Transition Delay - It is the time taken by the output signal to go from 20% to 80% of peak magnitude of rising waveform.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/rise%20transition%20delay.PNG)

4. Fall Transition Delay - It is the time taken by the output signal to go from 80% to 20% of peak magnitude of falling waveform.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/fall%20transition%20delay.PNG)



# Day 4: Pre-layout Timing Analysis and Clock Tree Synthesis

Place and routing (PnR) is performed using an abstract view of the GDS files generated by Magic. The abstract information will include metal and pin information. The PnR tool will use the abstract view information, formally defined as LEF information, to perform interconnect routing in conjunction to routing guides generated from the PnR flow.

An Introduction to LEF Files

Technology LEF - Contains layer information, via information, and restricted DRC rules
Cell LEF - Abstract information of standard cells

Some of the guidelines are as mentioned. It is said that the input and output Ports, should be designed in such a way that they occupy odd multiples of the set track values, presented in the LEF files. As shown in the image below, this tabular representation indicates the x and y offsets and pitch defined for each layer of the design. The Offset is exactly half the pitch, which indicates the tracks are centred about the origin.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%204/tracks.info.PNG)

# Standard Cell Pin Placement

On-track standard cell pin placement is essential for DRC free PnR flow. Pins must align with the li1 and met1 preferred routing directions. During standard cell creation this concept must be accounted for. To ensure a cell is aligned with routing grids in Magic we can display a grid on top of the gds file.

To display the grid in magic type the following command: % grid 0.46um 0.34um 0.23um 0.17um

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%204/grid%20cmd%20in%20magic%20tool.PNG)

Up next, we demonstrate how to define a PORT in magic. These ports can be Input/Output/VDD/GND depending upon the nature of the component and design. We use the GUI offered by Magic to do this. Upon clicking Edit->Text, One can find an interface open up which allows for enabling of a certain part of the design to become a port, whose number is customized by the user themselves.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%204/port%20confg%20in%20magic%20tool.PNG)

The ports are defined on the terminal as follows :-

% port class inout 
% port use power // for VPWR

% port class input
% port use signal // for A

% port class output
% port use signal // for Y

% port class inout
% port use ground // for VGND

After naming all the ports, cell view in magic tool:-

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%204/layout%20after%20port%20naming.PNG)

# LEF Generation in Magic

Magic allows users to generate cell LEF information directly from the Magic terminal. To generate the cell LEF file from Magic perform: % lef write

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%204/property%20lef%20file.PNG)


# Day 5: Final steps for RTL2GDS 
