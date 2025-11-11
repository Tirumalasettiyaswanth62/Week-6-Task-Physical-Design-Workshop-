# Day 4 :
## Pre-layout timing analysis and importance of good clock tree 

### Theory

### Implementation

* Section 4 tasks:-
1. Fix up small DRC errors and verify the design is ready to be inserted into our flow.
2. Save the finalized layout with custom name and open it.
3. Generate lef from the layout.
4. Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory.
5. Edit 'config.tcl' to change lib file and add the new extra lef into the openlane flow.
6. Run openlane flow synthesis with newly inserted custom inverter cell.
7. Remove/reduce the newly introduced violations with the introduction of custom inverter cell by modifying design parameters.
8. Once synthesis has accepted our custom inverter we can now run floorplan and placement and verify the cell is accepted in PnR flow.
9. Do Post-Synthesis timing analysis with OpenSTA tool.
10. Make timing ECO fixes to remove all violations.
11. Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts.
12. Post-CTS OpenROAD timing analysis.
13. Explore post-CTS OpenROAD timing analysis by removing 'sky130_fd_sc_hd__clkbuf_1' cell from clock buffer list variable 'CTS_CLK_BUFFER_LIST'.

Commands to open the custom inverter layout

```bash
# Change directory to vsdstdcelldesign
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

Screenshot of tracks.info of sky130_fd_sc_hd

<img width="1916" height="922" alt="tracksinfo" src="https://github.com/user-attachments/assets/82f5b25e-70de-46bf-89e3-6796fd6f6342" />

Commands for tkcon window to set grid as tracks of locali layer

```tcl
# Get syntax for grid command
help grid

# Set grid values accordingly
grid 0.46um 0.34um 0.23um 0.17um
```

Screenshot of commands run


<img width="1919" height="967" alt="gridofsky130mag" src="https://github.com/user-attachments/assets/f0ac996d-2318-41b2-a130-d2033e97e414" />

#### 2. Save the finalized layout with custom name and open it.

Command for tkcon window to save the layout with custom name

```tcl
# Command to save as
save sky130_vsdinv.mag
```

Command to open the newly saved layout

```bash
# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_vsdinv.mag &
```

Screenshot of newly saved layout
<img width="1919" height="967" alt="gridofsky130mag" src="https://github.com/user-attachments/assets/2eb008c7-2b8e-463f-9f62-a424c0219f89" />

#### 4. Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory.

Commands to copy necessary files to 'picorv32a' design 'src' directory

```bash
# Copy lef file
cp sky130_vsdinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# Copy lib files
cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```

Screenshot of commands run


<img width="1919" height="956" alt="copy and create new file as old " src="https://github.com/user-attachments/assets/c6fe10aa-6d61-4c7e-8b61-0ec164011bea" />

#### 5. Edit 'config.tcl' to change lib file and add the new extra lef into the openlane flow.

Commands to be added to config.tcl to include our custom cell in the openlane flow

```tcl
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```

Edited config.tcl to include the added lef and change library to ones we added in src directory


<img width="1919" height="960" alt="cofigtclcode" src="https://github.com/user-attachments/assets/b94342ff-957c-4cfe-a67a-94b377bdde1a" />

#### 6. Run openlane flow synthesis with newly inserted custom inverter cell.

Commands to invoke the OpenLANE flow include new lef and perform synthesis 

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Screenshots of commands run

<img width="1919" height="974" alt="pre_sta2" src="https://github.com/user-attachments/assets/94232669-f361-41fe-bc9b-9dc1995b8cd8" />
<img width="1919" height="965" alt="pre_sta1" src="https://github.com/user-attachments/assets/e977fca3-3e18-48af-b62b-a7b55dee9e9f" />
<img width="1919" height="963" alt="pre_sta5" src="https://github.com/user-attachments/assets/ea7e3abf-6c6c-4d98-8f36-018b018167bf" />
<img width="1919" height="962" alt="pre_sta4" src="https://github.com/user-attachments/assets/0a734b9d-9956-4827-b799-b1578fcb592b" />
<img width="1919" height="973" alt="pre_sta3" src="https://github.com/user-attachments/assets/dccd5a70-1c8e-4cb3-99e5-ad3863241a46" />

Commands to view and change parameters to improve timing and run synthesis

```tcl
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 24-03_10-03 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to display current value of variable SYNTH_STRATEGY
echo $::env(SYNTH_STRATEGY)

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to display current value of variable SYNTH_BUFFERING to check whether it's enabled
echo $::env(SYNTH_BUFFERING)

# Command to display current value of variable SYNTH_SIZING
echo $::env(SYNTH_SIZING)

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

#### 8. Once synthesis has accepted our custom inverter we can now run floorplan and placement and verify the cell is accepted in PnR flow.

Now that our custom inverter is properly accepted in synthesis we can now run floorplan using following command
<img width="1920" height="1080" alt="316330928-55de3fc6-498d-4456-8e79-ae6e175d2ca6" src="https://github.com/user-attachments/assets/179395ce-acd6-4009-a186-cbcf25efed26" />

```tcl
# Now we can run floorplan
run_floorplan
```

#### 9. Do Post-Synthesis timing analysis with OpenSTA tool.

Since we are having 0 wns after improved timing run we are going to do timing analysis on initial run of synthesis which has lots of violations and no parameters were added to improve timing

Commands to invoke the OpenLANE flow include new lef and perform synthesis 

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Commands run final screenshot





<img width="1919" height="968" alt="slack0" src="https://github.com/user-attachments/assets/0a41b662-9e90-4aca-8089-cdd6b072e735" />


<img width="1915" height="963" alt="report of or" src="https://github.com/user-attachments/assets/2f07fb84-5fdc-4fb9-be2b-d50bb7dc6144" />



<img width="1919" height="965" alt="run_synthesis" src="https://github.com/user-attachments/assets/bff74845-2f8a-4d4a-9532-567edab7a832" />


<img width="949" height="981" alt="run cts" src="https://github.com/user-attachments/assets/e508b2f3-b1e0-4b5f-a055-6bcff8465f84" />

#### 12. Post-CTS OpenROAD timing analysis.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

```tcl
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/24-03_10-03/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Check syntax of 'report_checks' command
help report_checks

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```

Screenshots of commands run and timing report generated

<img width="949" height="981" alt="run cts" src="https://github.com/user-attachments/assets/e3540010-fd5b-4b8b-a15e-c975979586d2" />


<img width="1919" height="969" alt="sky130lefcode" src="https://github.com/user-attachments/assets/99d0938d-bfd0-4010-8b90-d90eeacf1c38" />
<img width="1919" height="981" alt="synthesis_run2" src="https://github.com/user-attachments/assets/decb798c-e917-4870-a2bd-685be1e19ab5" />
<img width="1919" height="959" alt="sta pre_sta conf" src="https://github.com/user-attachments/assets/88321e7f-aa77-4681-9fcb-942176dacd7e" />

<img width="1919" height="989" alt="skyinvexpand" src="https://github.com/user-attachments/assets/497f2720-1774-4e98-bcb3-dbdd229fe077" />

<img width="1919" height="961" alt="magiclayout" src="https://github.com/user-attachments/assets/ff6b50b0-ae3b-45c4-83cb-2af190f9f984" />
<img width="1919" height="944" alt="my_base sdc1" src="https://github.com/user-attachments/assets/79c2fbe4-6664-422b-b415-7c63fdfde87e" />
<img width="1919" height="968" alt="slack0" src="https://github.com/user-attachments/assets/e4e2cf10-c963-475c-bdb5-87f1b0139c57" />
<img width="1919" height="940" alt="my_base sdc" src="https://github.com/user-attachments/assets/f2cc8a27-379c-42eb-981a-8f19dae98082" />
