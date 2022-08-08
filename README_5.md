# **ADVANCED PHYSICAL DESIGN USING OPENLANE/PDK130**
This repository contains all the information needed to build and run openLANE flow, which has the capability to perform full **ASIC** implementation steps from **RTL** to **GDSII**.

---
## **Contents**
- [**ADVANCED PHYSICAL DESIGN USING OPENLANE/PDK130**](#--advanced-physical-design-using-openlane-pdk130--)

**DAY-1**
  * [**SoC design and OpenLANE**](#--soc-design-and-openlane--)
  * [**Get familiar to open-source EDA tools**](#--get-familiar-to-open-source-eda-tools--)

  **DAY-2**

  * [**Steps to run Floorplanning using OpenLane**](#--steps-to-run-floorplanning-using-openlane--)
  * [**Steps to run  Congestion aware Placement using OpenLane & RePlace**](#--steps-to-run--congestion-aware-placement-using-openlane---replace--)

  **DAY-3**

  * [**Steps to Make changes in the IO Placer**](#--steps-to-make-changes-in-the-io-placer--)
  * [**Steps to git-clone vsdstdcelldesign and Analyse it using ngSpice**](#--steps-to-git-clone-vsdstdcelldesign-and-analyse-it-using-ngspice--)

  **DAY-4**

  * [**Converting Grid Info to Track Info**](#--converting-grid-info-to-track-info--)
  * [**Converting Magic Layout to Std. Cell Lef**](#--converting-magic-layout-to-std-cell-lef--)
  * [**Configure Synthesis Settings to fix Slack and include vsdinv**](#--configure-synthesis-settings-to-fix-slack-and-include-vsdinv--)
  * [**Steps to run CTS using TritonCTS**](#--steps-to-run-cts-using-tritoncts--)
  * [**Timings with OpenSTA**](#--timings-with-opensta--)
  * [**Deleting a clock buffer**](#--deleting-a-clock-buffer--)

  **DAY-5**

  * [**Steps to Build Power Distribution Network(PDN)**](#--steps-to-build-power-distribution-network-pdn---)
  * [**Steps to Run Routing**](#--steps-to-run-routing--)

 [**Acknowledgements**](#--acknowledgements--)

---
## **SoC design and OpenLANE**
The broad view flow from an idea to the hardware is as shown-

![FLOW](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-1/FLOW.png)

Our aim in this workshop is to go in depth from RTL to Device and create a chip of our own using OpenSource tools and also understand what all steps are involved in it.
This 
An Open Source Digital ASIC Design require three components-
- RTL(Register Transfer Language) Designs
- EDA(Electronic Design Automation) Tools
- PDK(Process Design Kit) Data

**OpenLANE** is an Open-Source PDK Flow which is a collection of files used to model a fabrication process for the EDA tools used to design a complete IC.Its main goal is to produce a clean GDSII with no human intervention.It is tuned for **SkyWater 130nm Open PDK**. It has two modes of operation-Autonomous and Interactive i.e. one can either look into each of the Design Flow one by one or can let it run automatically.Given below is the Openlane Design flow which also shows all of the open source EDA tools that were utilised in the making of OpenLane Flow for performing all the steps in the ASIC Design Flow.(Image Courtesy: efabless/openlane)

![OpenLANE Design Flow](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-1/openlane.png)

---
## **Get familiar to open-source EDA tools**

In order to begin the Openlane flow the following commands are entered one after the other in the terminal window-

```
Desktop/work/tools/openlane_working_dir/openlane
docker
flow.tcl -interactive
package require openlane 0.9
```
We are now ready to run the commands in Openlane.

![d1_1](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-1/d1_1.png)

The first step of synthesis of the design - **picorv32a(RISC-V based CPU Core)** is performed using the command in Openlane flow terminal window by-
```
run_synthesis
```
PicoRV32 is free and open hardware licensed under the ISC license.

The flow, since is interactive in nature requires that all the commands be given in a sequential order without skipping any instruction.

Once the synthesis is successfully done, the synthesis report should be read and understood thorrowly for analysis purposes. Some important parameters seen were chip area, no. of cells, no of d-ff's, etc.

In order to calculate the **flop ratio** of the design which is- **(the no. of d-ff's/the total no. of cells)**, we check the values from the report and calculate.

![d1_2](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-1/d1_2.png)

![d1_3](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-1/d1_3.png)

The flop ratio comes out to be 0.108.

The synthesized netlist(.v) is visible in the synthesis folder in the results directory now as shown below.

![d1_4](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-1/d1_4.png)

The synthesis statistical report can also be accessed from the synthesis folder in reports for the current run similar to the one we saw during synthesis in Openlane terminal using-
```
less 1-yosys_4.stat.rpt
```
The reports and results folder can be studied extensively for more in depth understanding of the synthesis process done.

---
## **Steps to run Floorplanning using OpenLane**
The step of floorplanning can be done using the command-
```
run_floorplan
```
With this the floorplanning has been done.
The folder of floorplanning in the results(for the current run) now has the def file made for the floorplan as shown. 

![d2_1](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-2/d2_1.png)

This consists of all the details related to the floorplan that has been made for the current design such as pin placement, macro placement, etc. as shown.

![d2_2](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-2/d2_2.png)

All other files should be equally explored to see how the floor -planning has been done for all the constituents of the layout.One such file is the **ioplacer.log** that contains the information about the pins placed during floorplanning as shown-

![d2_8](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-2/d2_8.png)

![d2_9](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-2/d2_9.png)

Now, in order to open the layout to see the floor-planning done in **Magic**(Open Source VLSI Layout Tool) the following command is followed-
```
magic -T <.tech file> lef read <.lef file> def read <.def file> &
```
![d2_3](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-2/d2_3.png)

This opens the layout made as follows-

![d2_4](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-2/d2_4.png)
 
Upon zooming, the pin placement can be seen as follows-

![d2_5](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-2/d2_5.png)
 
The macros(lut's, etc.) can be seen as follows-

![d2_6](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-2/d2_6.png)

As can be seen the std. cells are now placed in the floorplan.
The tkcon window along with the layout window is used to put in commands and know more about the layout as shown below.

![d2_7](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-2/d2_7.png)

---
## **Steps to run  Congestion aware Placement using OpenLane & RePlace**
The congestion aware placement is done(no involvement of timing constraints) which invloves placement in two stages global and detailed placement with Legalisations).

The main objective of global placement is to reduce wirelength(reduction of Half-Parameter wirelength in Openlane-HPWL).
 
The placement is performed by-
```
run_placement
```
The iterations being performed during the running process should have reduced overflow values until the last iteration which indicated that the process of placement is being done correctly.
Also check for the legality parameters being passed after the run is complete.

Now, to see the placement we perform the command for the magic layout window to open as follows-

![d2_10](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-2/d2_10.png)

The image for the layout after placement is shown as follows-

![d2_12](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-2/d2_12.png)

It can be seen that the power-ground distribution network has been formed at this step for power and ground signals to be provided to the different cells.
This is done after floorplanning but in Openlane it is done after the CTS step.

The detailed placement of cells is done as shown in the figure below-

![d2_11](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-2/d2_11.png)

---
## **Steps to Make changes in the IO Placer**

Openlane allows changes to be made on the fly in an interactive session.The same example is illustrated as given.

We open the layout folder using the following commands-

![d3_1](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_1.png)

 The opened layout shows the placement of the io pins equidistant.

![d3_2](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_2.png)

If me make changes in the floorplan using the below commands to the variable-**FP_IO_MODE** as 2 which was earlier set to 1(equidistant mode) in the floorplan.tcl folder in configurations,
```
set $::env(FP_IO_MODE) 2
```
we see that the pins are no more equidistant but are stacked one on top of the other.

![d3_3](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_3.png)

---
## **Steps to git-clone vsdstdcelldesign and Analyse it using ngSpice**

The cell design that we want to use in our layout design is cloned from git instead of making as it takes a lot of time. We use the command-
```
git clone <file link from github>
```
eg.-

![d3_4](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_4.png)

It appears in Openlane as follows-

![d3_5](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_5.png)

Its contents are as follows-

![d3_6](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_6.png)

Now we copy **Sky130A.tech file** from the magic folder and paste it in the vsdstdcelldesign folder as shown.

![d3_7](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_7.png)

The result can then be seen in the folder.

![d3_8](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_8.png)

Now using the following magic command we see the layout-

![d3_9](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_9.png)

There is no need to mention the entire path as the same folder is already open.

The layout of the cell is as shown below-

![d3_10](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_10.png)

Upon selecting a part we type **what** in the tkcon window and obtain the name.

In order to know the functionality of the cell we need to extract the SPICE and then we need to simulate it in **ngSpice**.

Extraction is done as follows-

![d3_11](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/D3_11.png)

This creates a new .ext file as shown-

![d3_12](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_12.png)

Then we give the command as follows to create s SPICE file to be used with the ngSpice tool.

![d3_13](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_13.png)

A SPICE file is created as follows-

![d3_14](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_14.png)

The constituents of the SPICE file upon opening are-

![d3_15](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_15.png)

Changes are made in the file in order to make it compatible to our layout by following the command-

![d3_16](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_16.png)

The updated file looks as follows-

![d3_18](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_18.png)

The ngspice is activated in the terminal as follows for simulation-

![d3_17](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_17.png)

The output waveform is obtained by writing the command in ngSpice as-
```
plot y vs time a
```
The output waveform obtained upon simulation is as follows-

![d3_19](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_19.png)

Jitters can be seen in the waveform which can be removed as follows-

![d3_20](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_20.png)

Now , on observing the waveform and plotting some points we can analyse the delays of the cell as follows-

![d3_21](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-3/d3_21.png)

**Rise Time**: Time taken by output to rise from 20% of the peak value to 80% of the peak value.

The value calculated using first two x-co-ordinate sets is:2.24567-2.181937=**0.063733**.

**Cell Fall Delay**: Difference between 50% of two consecutive fall values.

The value calculated using last two x_cordinate sets is:2.21-2.15=**0.06**.

---
## **Converting Grid Info to Track Info**
 
The first step here would be to extract a LEF file out of the layout.

**Cell LEF** - It's an abstract view of the cell and only gives information about PR boundary, pin position and metal layer information of the cell.

The **tracks.info file** gives information about where you want your routs to go and this file is used in the routing stage.

The command to open it is as follows-

![d4_1](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_1.png)

The file looks like-

![d4_2](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_2.png)

In order to make this design compatible for the flow, we set the grid as follows-

![d4_3](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_3.png)

Some other parameters that are set are as follows-

![d4_4](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_4.png)

![d4_5](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_5.png)

![d4_6](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_6.png)

![d4_7](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_7.png)

Save the file using the command in the tkcon window-
```
save sky130_vsdinv.mag
```
This gets saved in vsdstdcelldesign folder as shown.

![d4_8](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_8.png)
 
Now using the command as shown-
```
lef write
```
This updates the LEF file.

![d4_9](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_9.png)

The updates made can be seen upon opening the lef file as follows-

![d4_10](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_10.png)

---
## **Converting Magic Layout to Std. Cell Lef**

The first step is to copy the .lef file into the src folder.

![d4_11](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_11.png)

![d4_12](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_12.png)

Now the entire cell file can be read from the .lib from the vsdstdcelldesign directory as follows-

![d4_13](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_13.png)

We copy these .lib files to the src folder using the cp command as follows-

![d4_14](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_14.png)

![d4_15](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_15.png)

The next step is to modify the config.tcl file.

![d4_16](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_16.png)

We then overwrite the runs folder during prep in Openlane.

![d4_17](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_17.png)

The **ERRORS** are catered to by making necessary changes.

The following values are set before synthesis.

![d4_18](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_18.png)

The synthesis is then done.

---
### **Configure Synthesis Settings to fix Slack and include vsdinv**

The modifications done to reduce slack violations are as follows-
To do this you don't need to exit the flow and change it in config.tcl. It can be done on the fly.

echo command checks the values and set command sets it.

![d4_19](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_19.png)

The synthesis is run again now.

This process should reduce the slack values but it remains the same.

This is an **ERROR** faced and it got resolved by following the below commands-

![d4_20](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_20.png)

The chip area increased as the slack reduced. Hence, it is a tradeoff.

![d4_21](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_21.png)

![d4_22](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_22.png)

The commands that follow are-
```
init_floorplan
place_io
global_placement_or
detailed_placement
tap_decap_or
```
The layout file is now opened using the magic command and the new layout with the vsdstdcelldesign is seen.

![d4_23](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_23.png)

The layout observed is as follows-

![d4_24](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_24.png)

![d4_25](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_25.png)

![d4_26](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_26.png)

The detailed timimg analysis is done outside Openlane in **OpenSTA**.

---
## **Steps to run CTS using TritonCTS**

The next step is to run a cts after putting in the std. cell from vsdstdcelldesign using-
```
run_cts
```
Openlane using **TritonCTS** EDA is used to create CTS with one corner only.

---
## **Timings with OpenSTA**

The first step is to initiate OpenroadWwhich has OpenSTA integrated in it).

![d4_27](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_27.png)

Read the LEF() and DEF()-formed at each step, files as follows-

![d4_28](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_28.png)

A db file is now written in Openroad folder.
```
write_db pico_cts.db
```
The following commands follow-

![d4_29](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_29.png)

![d4_30](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_30.png)

Upon seeing the final reports, the hold slack if it occurs can't be catered to as it does not depend on the clock period, but the setup time does and can be reduced.

The timing analysis made here is not true as the TritonCTS is right now made for **CTS** using typical corner but we have included fast and slow libraries as well for STA here. So, it needs to be rectified following the steps-

![d4_31](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_31.png)

With this the slacks have been met and not violated for both setup and hold times.

---
## **Deleting a clock buffer**

The next step would be to delete a clock buffer(smaller one out of the list) and see what difference does it make to the STA.

These steps are followed and the CTS run again.

![d4_32](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_32.png)

This gives an error as shown-

![d4_33](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_33.png)

This is because the def file being used over here is post cts one, and we have to actually use a post placement def file as shown-

![d4_34](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_34.png)

Run the CTS again.

The **STA** is done again in **Openroad**.

![d4_35](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_35.png)

![d4_36](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_36.png)

The new timing values are as follows-

![d4_37](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_37.png)

It is seen that the result values for slack have improved and that it has although taken a toll on the area.

Put the buffer back into the list.

![d4_38](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-4/d4_38.png)

---
## **Steps to Build Power Distribution Network(PDN)**
The Power Distribution Network is created by the command-
```
gen_pdn
```
This creates a pdn.def file(as shown below) and the layout is now ready for routing.

![d5_1](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-5/d5_1.png)

---
## **Steps to Run Routing**

To perform routing we give the command-
```
run_routing
```
The README file in configurations directory gives the information of all the parameters(variables) that are to be set for the particular step in the function. Reading it gives a lot of insight into the entire flow. The README.md file is shown in this repository and can be accessed as follows-

![d5_2](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-5/d5_2.png)

The detailed routing done in Openlane is using Triton Route and the global route is done by FastRoute. Different Triton strategies are used which cater to different runs such as fast run with low detailing and vice-versa.

The routing files give the details of the entire process. The list is as shown below.

![d5_3](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-5/d5_3.png)

The **fastroute.guide** file is used to to refer to the routing done by FastRoute(Global Routing).It consists of the initial Routing Guides as shown below-

![d5_4](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-5/d5_4.png)

The post-route STA analysis is done using SPEF-EXTRACTOR outside Openlane.

Now let us look at the total no.of def files created in the entire process.

As shown below, 
The first .v file was created after synthesis.
Followed by thus, the second file got created after CTS(Clock Tree Routing) step because of the insertion of clock buffers apart from the buffers that were inserted during synthesis.
 
The other two are created before routing as the antenna diodes are inserted at this stage.

![d5_5](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-5/d5_5.png)

The routed layout is as follows-

![d5_6](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-5/d5_6.png)

![d5_7](https://github.com/Amreen-Kaur/VSD-IAT-5-Day-Cloud-Based-Workshop/blob/main/DAY-5/d5_7.png)

---
## **Acknowledgements**
I would like to thank **Kunal Ghosh, Co-founder (VSD Corp. Pvt. Ltd)** and **Nickson Jose, VLSI Engineer** for organizing this wonderful workshop and helping us throughout the process of learning.

