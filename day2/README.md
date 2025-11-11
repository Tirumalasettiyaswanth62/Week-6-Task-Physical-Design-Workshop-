# Day2:
## Good floorplan vs bad floorplan and introduction to library cells
### Implementation

Section 2 tasks:- 
1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.
2. Calculate the die area in microns from the values in floorplan def.
3. Load generated floorplan def in magic tool and explore the floorplan.
4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.
5. Load generated placement def in magic tool and explore the placement.

* All section 2 logs, reports and results can be found in following run folder:
Commands to invoke the OpenLANE flow and perform floorplan

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

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Now we can run floorplan
run_floorplan<img width="1904" height="993" alt="Screenshot 2025-10-28 130147" src="https://github.com/user-attachments/assets/b5f38bd6-d38c-4dbd-afb2-9ff6c0b5880d" />

```


Screenshot of floorplan run
<img width="1905" height="977" alt="Screenshot 2025-10-28 125457" src="https://github.com/user-attachments/assets/be9c5b54-4e55-4171-90c9-a90ff2d428c1" />




<img width="1904" height="993" alt="Screenshot 2025-10-28 130147" src="https://github.com/user-attachments/assets/e41b4b98-e6b4-4199-937d-bcfcd89360c5" />




<img width="1913" height="996" alt="Screenshot 2025-10-28 124127" src="https://github.com/user-attachments/assets/4ce8e4c1-5124-4eb1-8ea4-bc123cb63321" />



<img width="1910" height="975" alt="Screenshot 2025-10-28 130658" src="https://github.com/user-attachments/assets/42cfbd91-27b8-4984-9a3b-9424cccb308e" />



<img width="1907" height="970" alt="Screenshot 2025-10-28 130822" src="https://github.com/user-attachments/assets/44981c6a-c519-4792-92bb-d4ee2b641f56" />
<img width="1911" height="941" alt="Screenshot 2025-10-28 135038" src="https://github.com/user-attachments/assets/e0bb4406-da4a-4e6b-95a0-72c1e9149869" />
<img width="1910" height="938" alt="Screenshot 2025-10-28 135111" src="https://github.com/user-attachments/assets/6dac7a0d-1a22-4ebc-a3d9-e230e9c2a6f4" />




