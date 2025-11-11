# Day 3 :
## Design library cell using Magic Layout and ngspice characterization

### Theory

### Implementation

* Section 3 tasks:-
1. Clone custom inverter standard cell design from github repository: [Standard cell design and characterization using OpenLANE flow](https://github.com/nickson-jose/vsdstdcelldesign).
2. Load the custom inverter layout in magic and explore.
3. Spice extraction of inverter in magic.
4. Editing the spice model file for analysis through simulation.
5. Post-layout ngspice simulations.
6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

#### 1. Clone custom inverter standard cell design from github repository

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Check contents whether everything is present
ls

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

Screenshot of commands run

#### 2. Load the custom inverter layout in magic and explore.

Screenshot of custom inverter layout in magic

<img width="1914" height="964" alt="Screenshot 2025-10-28 202453" src="https://github.com/user-attachments/assets/9897d029-d349-4d68-ab64-a25296eab4df" />

NMOS and PMOS identified


<img width="1917" height="998" alt="magic pmos" src="https://github.com/user-attachments/assets/cffcb34b-3a1e-4592-9dd2-dcadb4856445" />


<img width="1918" height="989" alt="magic nmos" src="https://github.com/user-attachments/assets/81f3e372-24b2-4a7e-9927-e7c6350a5e9c" />

Deleting necessary layout part to see DRC error


<img width="1918" height="999" alt="drc error " src="https://github.com/user-attachments/assets/2bc07f46-eeba-41b7-9241-955f421efeef" />

#### 3. Spice extraction of inverter in magic.

Commands for spice extraction of the custom inverter layout to be used in tkcon window of magic

```tcl
# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice
```
Screenshot of created spice file


<img width="1919" height="994" alt="ngspice code" src="https://github.com/user-attachments/assets/a9a697ab-dd6f-42d3-b574-a206ef512f15" />

#### 4. Editing the spice model file for analysis through simulation.

Measuring unit distance in layout grid


<img width="1915" height="993" alt="minimum value of layout" src="https://github.com/user-attachments/assets/3aa3fe11-3c27-43b5-8e11-3b4dd53c35d3" />

Final edited spice file ready for ngspice simulation

<img width="1919" height="994" alt="ngspice code" src="https://github.com/user-attachments/assets/23f951ae-9811-49b5-9abb-207fe11da3af" />

#### 5. Post-layout ngspice simulations.

Commands for ngspice simulation

```bash
# Command to directly load spice file for simulation to ngspice
ngspice sky130_inv.spice

# Now that we have entered ngspice with the simulation spice file loaded we just have to load the plot
plot y vs time a
```

80%  screen shots:

<img width="1914" height="1007" alt="80% of wave" src="https://github.com/user-attachments/assets/99a9cacb-dec9-49be-8749-1d1984fda9bc" />

<img width="452" height="121" alt="80%rise time" src="https://github.com/user-attachments/assets/2185cc30-688b-4bcf-a726-d7b913a76e8e" />

50% screen shots:

<img width="1919" height="971" alt="50% wave" src="https://github.com/user-attachments/assets/11507d70-f302-431f-bf17-33f149bc8a5d" />


<img width="440" height="99" alt="cellrise delay50%" src="https://github.com/user-attachments/assets/a4dcb903-c9b7-4733-98ea-461a2af614a0" />

20% screenshots :


<img width="1918" height="959" alt="20%wave" src="https://github.com/user-attachments/assets/e093d831-a442-4a37-ba0d-b91d7bd475c5" />


<img width="452" height="120" alt="20%of wave" src="https://github.com/user-attachments/assets/4c7c790d-e84d-4a4d-8f45-79b450cd8ca1" />

#### 6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

Link to Sky130 Periphery rules: [https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html)

Commands to download and view the corrupted skywater process magic tech file and associated files to perform drc corrections

```bash
# Change to home directory
cd

# Command to download the lab files
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

# Since lab file is compressed command to extract it
tar xfz drc_tests.tgz

# Change directory into the lab folder
cd drc_tests

# List all files and directories present in the current directory
ls -al

# Command to view .magicrc file
gvim .magicrc

# Command to open magic tool in better graphics
magic -d XR &
```

Screenshots of commands run


<img width="1914" height="972" alt="magicrc" src="https://github.com/user-attachments/assets/0224c15a-b965-43ed-bcc3-e14a5c825432" />

**Incorrectly implemented poly.9 simple rule correction**

Screenshot of poly rules

<img width="1909" height="971" alt="m3 webpage" src="https://github.com/user-attachments/assets/76a4455a-d9f8-40cb-a95d-72f4c528656f" />

Incorrectly implemented poly.9 rule no drc violation even though spacing < 0.48u



<img width="1916" height="967" alt="poly" src="https://github.com/user-attachments/assets/8796348b-3770-432a-8462-6852770ce8ce" />


<img width="1919" height="959" alt="ploy mag" src="https://github.com/user-attachments/assets/3d3e4673-041d-4c35-a97e-8b3d868e3ec7" />

New commands inserted in sky130A.tech file to update drc


<img width="1919" height="933" alt="nwell drc code1" src="https://github.com/user-attachments/assets/1e6cb7f1-3d42-4ca8-bf0d-21729a54841e" />

<img width="1919" height="957" alt="nwell drc code 2" src="https://github.com/user-attachments/assets/1a6e20bb-15fd-4ea4-860e-a09c17eae420" />

Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented

<img width="1918" height="994" alt="error coorection drc poly" src="https://github.com/user-attachments/assets/19903a98-cd21-475a-a0c3-322b8b614a68" />


<img width="1918" height="966" alt="poly drc why" src="https://github.com/user-attachments/assets/305c385a-4e16-4b30-bafc-3a1fd6c71dbc" />

Incorrectly implemented difftap.2 simple rule correction


<img width="1919" height="972" alt="difftap" src="https://github.com/user-attachments/assets/e5d2fdc8-f2a7-4403-92d5-711c9f0d6410" />

New commands inserted in sky130A.tech file to update drc


<img width="1918" height="966" alt="geditpoly" src="https://github.com/user-attachments/assets/19c73da0-12c9-4d74-a3e7-deb0e22b8969" />

Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented
<img width="1919" height="945" alt="nwell drc why " src="https://github.com/user-attachments/assets/13ed6819-8077-4cc5-8acc-e6b10044fc5e" /><img width="1918" height="966" alt="geditpoly" src="https://github.com/user-attachments/assets/b660f6a4-8902-4f89-8822-7f3946d787e3" />

<img width="1919" height="<img width="1919" height="960" alt="drcwhy" src="https://github.com/user-attachments/assets/304160a4-a98e-4f03-834b-6d3ac536e180" /><img width="1918" height="999" alt="drc error " src="https://github.com/user-attachments/assets/1709e934-ee68-4983-b4a2-09dab88bb868" />mg width="1916" height="992" alt="extracted spice deck" src="https://github.com/user-attachments/assets/509e5080-2d00-4651-ad7b-b61e602ff66d" />

<img width="1916" height="992" alt="extracted spice deck" src="https://github.com/user-attachments/assets/619d9836-9488-403e-bdd8-4ce617c37a8e" />
<img width="1918" height="994" alt="error coorection drc poly" src="https://github.com/user-attachments/assets/5861aec2-3342-4f27-a838-72502f25be38" />


<img width="1916" height="939" alt="cifseeVIA2" src="https://github.com/user-attachments/assets/f9553d9b-1b7a-4e97-9471-a7e78164a24b" />


<img width="1919" height="967" alt="boxcontactcut" src="https://github.com/user-attachments/assets/249a7d61-d61e-41b2-8f4a-c1c9be4c8d78" />


