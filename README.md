# VSD-Sky130-OpenLANE-Workshop
Attended a 5-Day workshop on [Advanced Physical Design using OpenLANE/Sky130](https://www.vlsisystemdesign.com/advanced-physical-design-using-openlane-sky130/) conducted by [VLSI System Design](https://www.vlsisystemdesign.com/). The workshop introduced us to opensource EDA tools and PDKs and how are they used in the semiconductor industry. We performed the synthesis, floorplan, placement, clock tree synthesis and routing in openlane tool. We also designed and characterized a library cell in ngspice.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/APD%20Openlane%20workshop.PNG)

# Prerequisites
Installed and ran OpenLANE on Ubuntu OS. It is recommended to allocate 4GB RAM and 40GB Disk Space to OS for smooth functioning. Windows user can use Oracle VM VirtualBox to run the OS. For further information about installation of OpenLANE and open-source EDA tools, refer to the following two videos on Udemy by VSD.

[A complete to install OpenLANE and Sky130nm PDK](https://www.udemy.com/share/103wqAAEESeVZUR3QF/)

[A complete to install open-source EDA tools](https://www.udemy.com/share/101skKAEESeVZUR3QF/)




# RTL to GDSII flow

ASIC (Application Specific Integrated Circuit) Design Flow is an iterative and dynamic process which follows various steps.. The flow has 11 different stages as shown below :-

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/asic%20design%20flow.PNG)

Chip Specification - The VLSI engineers are provided with the specifications according to which they need to design circuits taht meets these constraints for their system.

Design Entry/Functional Verification - In this stage the RTL design and Behavioral Modeling are performed using the Hardware Description Language (HDL).

RTL synthesis - In this stage the system taht was designed is implemented and represented and the netlists are tech-mapped to the specific logic gates.

Partitioning of Chip - In this step demarcation of certain sections of the chip and designing each sub-section happens.

DFT Insertion - DFT (Design for test) Circuit is inserted.

Floorplanning - Placing the core and die happens in this stage. Also Power Distribution Network is generated in this stage.

Placement Stage - In the placement stage the positions of standard cells get fixed. Placement in OpneLANE happens in two stages: 1.Global Placement - Optimization is the main criteria here. Optimize by reducing wire length by reducing Half perimeter wire length(HPWL). It is not legal placement i.e standard cells in rows might overlap 2.Detailed Placement - Legalization is the main criteria here.

Clock Tree Synthesis - This stage works towards building a clock distribution network that whose role is to deliver the clock to all associated sequential elements prresent in the design.

Routing - This stage implements the interconnect system between standard cells in the design and minimizes DRC errors.

Final Verification - A final verification before sending the chip for fab is done here.

GDSII - Graphic Design System(GDS) is a document that contains the layout design of the chip. It comes in the binary format, which is readable by specific EDA tools.This file is sent to fabs to get fabricated.

# The OpenLANE Flow

The OpenLANE Flow is as follows:

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/openlane%20flow.PNG)

The various tools used by OpenLANE in ASIC Flo are :

Synthesis
yosys - Performs RTL synthesis
abc - Performs technology mapping
OpenSTA - Pefroms static timing analysis on the resulting netlist to generate timing reports
Floorplan and PDN
init_fp - Defines the core area for the macro as well as the rows (used for placement) and the tracks (used for routing)
ioplacer - Places the macro input and output ports
pdn - Generates the power distribution network
tapcell - Inserts welltap and decap cells in the floorplan
Placement
RePLace - Performs global placement
Resizer - Performs optional optimizations on the design
OpenPhySyn - Performs timing optimizations on the design
OpenDP - Perfroms detailed placement to legalize the globally placed components
CTS
TritonCTS - Synthesizes the clock distribution network (the clock tree)
Routing *
FastRoute - Performs global routing to generate a guide file for the detailed router
TritonRoute - Performs detailed routing
SPEF-Extractor - Performs SPEF extraction
GDSII Generation
Magic - Streams out the final GDSII layout file from the routed def
Checks
Magic - Performs DRC Checks & Antenna Checks
Netgen - Performs LVS Checks
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

yosys_2.stat.rpt folder, present in the designs/picorv32a/runs/<date_of_run>/results/synthesis, is the one we use for calculating buffer ratio, flop ratio etc.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%201/VSD%20LAB-1%20Flop%20Ratio.PNG)

Buffer Ratio = (sky130_fd_sc_hd__buf_1 + sky130_fd_sc_hd__buf_2)/(Number of Cells)= (2247+44)/17323 = 0.1322

Flop Ratio =  (sky130_fd_sc_hd__dfxtp_4)/(Number of Cells) = 1634/17323 = 0.0943



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

1. Rise Cell Delay - It is the propagation delay of signal when it propagates from input to output. It is calculated by taking difference of x - co-ordinates at (50% of peak magnitude of rising output waveform) - (50% of peak magnitude of falling input waveform). Rise cell delay was found to be 0.3019 ns.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/rise%20delay.PNG)

2. Fall Cell Delay - It is the propagation delay of signal when it propagates from input to output. It is calculated by taking difference of x - co-ordinates at (50% of peak magnitude of falling output waveform) - (50% of peak magnitude of rising input waveform). Fall cell delay was found to be 0.1489 ns.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/fall%20delay.PNG)

3. Rise Transition Delay - It is the time taken by the output signal to go from 20% to 80% of peak magnitude of rising waveform. In this case, the delay was 0.1282 ns.

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%203/rise%20transition%20delay.PNG)

4. Fall Transition Delay - It is the time taken by the output signal to go from 80% to 20% of peak magnitude of falling waveform. In this case, the delay was 0.0591 ns.

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

Viewing the lef file:

![alt text](https://github.com/Dole97/VSD-Sky130-OpenLANE-Workshop/blob/main/VSD%20Workshop/Day%204/view%20lef%20file.PNG)

We see that setting a layer as port , it creates a PIN.

Copy lef file to picorv32a/src: 

Copy the libraries to picorv32a/src:





Including Custom Cells in OpenLANE
Modify the picorv32a/src/config.tcl file as :



prep design and this time if you are already using the older tag then use prep -degign <design_name> -tag <tag_name> -overwrite. -overwrite is significant as it overwrites the new changes made in the configuration files.

After add these commands to include sky130_vsdinv.lef in ~/tmp/meged.lef in openlane flow:

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs


Run synthesis:



We see slack violations after synthesis:



Viewing the Custom Inverter cell in Magic
Run floorplan and placement. Invoke magic: 

Our Standard Inverter cell is now integrated to the picorv22a layout:



Type the command expand in tkcon window to view layout:



Copy my_base.sdc to picorc32a/src:



my_base.sdc file is as shown:



Crate pre_sta.conf file and run sta pre_sta.conf in terminal.

pre_sta.conf 

running sta pre_sta.conf: 



Fixing Slack Violations
Here we change the synthesis strategy from 2 to 1. 

Now to reduce the slack violation we will be upsizing the buffere with large delays. Note that we are doing this in terminal (with sta_ pre_conf.sta command) and not inside openlane.

We will upsize cell 41881 which belongs to net 11341.



Type command report_net -connection _11341_ to get details about the net.



To upsize buffere 41881 from 1 to 4 use the command : replace_cell _41881_ sky130_fd_Sc_hd__buf4

To check reports type the command : report_checks -fields {net cap slew input_pins} -digits 4



Go on doing doing this with other bufferes to reduce the slack viloations in range (-1-0)(i.e expample 0.7).

Improvements in slack violations: 

Now use the command write_verilog <.v file_location> to update the verilog file after pre_sta reduction in slack violations.



Note not to run synthesis again in openLANE flow and directly run floorplan after this.

Clock Tree Synthesis
To run Clock tree synthesis in OpenLANE flow us e the command -> % run_cts

OpenLANE will add the necessary buffers required to make sure the timing rules are adhered and will modify our pre-existing netlist. A file named "picorv32a.synthesis_cts.v" will be created in the /designs/picorv32a/runs/<tag_name>/results/synthesis directory.

OpenROAD
Timing analysis is done in OpenLANE by creating a .db database file. This database file is created from the post-cts LEF and DEF files. To invoke OpenROAD use command -> % openroad.

Then type these command:

% write_db pico_cts.db

% read_db pico_cts.db

% read_lef <Location_of_LEF_file> //Location of LEF file - /designs/picorv32a/runs/<tag_name>/tmp/merged.lef

% read_def <Location_of_DEF_file> //Location of DEF file - /designs/picorv32a/runs/<tag_name>/results/cts/picorv23a.cts.def

% read_verilog <Location_of_verilog_file> //Verilog file - /designs/picorv32a/runs/<tag_name>/results/synthesis/picorv32a.synthesis_cts.v

% read_liberty $::env(LIB_SYNTH_COMPLETE)

% link_design <design_name> //design name = picorv32a

% read_sdc <Location_of_sdc_file> //sdc file - /designs/picorv32a/runs/<tag_name>/src/my_base.sdc

% set_propagated_clock [all_clocks]

% report_checks -path_delay min_max -fields {slew trans net cap inpput_pin} -format full_clock_expanded -digits 4 

# Day 5: Final steps for RTL2GDS

Checking which part of flow we are in
TO check which part of flow we currently are in use the command: % echo $::env(CURRENT_DEF)

After the CTS when we use the command we see that CURRENT_DEF holds the def file that we get after performing the cts stage. 

Power Distribution Network
TO generate PDN us the command: gen_pdn



This creates power ring for the entire core, power halo for any preplaced cells, power straps to bring power into the center of the chip and power rails for the standard cells.

The distribution of power in the cell is as shown:



After the PDN stage run % echo $::env(CURRENT_DEF) command again, and you will see that the current file is pdn.def



Routing
There are two types of routing : Global and Detailed routing. Global routing is done by FastRoute and Detailed routing is done by TritonRoute. TritonRoute offers us various routing strategis.ROUTING_STRATEGY from 0 to 3 uses Triton-13 engine which has faster runtime and ROUTING_STRATEGY 14 uses Triton-14 engine which has better DRCs. For checking which strategy we are using use the command :

% echo $::env(ROUTING_STRATEGY)

After this run routing by using the command : run_routing



Routing completed:



SPEF Extraction
Once the routing is completed, the interconnect parasitics are extracted to perform sign off Post-STA analysis. These parasitics are extracted into a SPEF file.

The SPEF EXTRACTOR is yet to be integrated to OpenLANE, we have to run it separately. Its available in /work/tools directory. We have to run the python file named main.py in SPEF_EXTRACTOR directory. The command to do this is as folows:

$ python3 main.py /designs/picorv32a/runs/<tag_name>/tmp/merged.lef /designs/picorv32a/runs/<tag_name>/results/routing/picorv32a.def



After the extraction is complete we open the /designs/picorv32a/runs/<tag_name>/results/routing and we will finf .spef file created there:

# Acknowledgements

Nickson Jose - VSD VLSI Engineer (https://github.com/nickson-jose/)

Kunal Ghosh - Co-founder (VSD Corp. Pvt. Ltd) (https://github.com/kunalg123)

Praharsha Mahurkar - Teaching Assistant at VSD (https://github.com/praharshapm)



