# Day-1 :
### 1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform synthesis

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

# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```
#### Screenshots of synthesis statistics report file with required values highlighted
<img width="940" height="877" alt="Screenshot 2025-10-28 122357" src="https://github.com/user-attachments/assets/390652fe-a789-43ad-858b-931f3340784d" />

<img width="935" height="787" alt="Screenshot 2025-10-28 122452" src="https://github.com/user-attachments/assets/ce29361a-0e4b-4d68-ab78-3dd744d1c0ab" />

<img width="1592" height="947" alt="Screenshot 2025-10-28 123834" src="https://github.com/user-attachments/assets/b8e82dae-897c-4d3d-848b-36e07c0e7b78" />




