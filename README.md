# DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING

## DAY 1 - Inception of open-source EDA, OpenLANE and sky130 PDK

### 1.	How to talk to computers?

#### 1.1.1 Introduction: 

This is the typical Arduino Leonardo Board. The highlighted part is the chip, and we are going to learn more about the design and contents of the chip in this course. This chip is designed from synthesis all the way down for production using the RTL to GDSll pipeline. Arduino consists of a programmable circuit board and a piece of software, or IDE that runs on a computer and is used to create and upload computer code to Arduino. 

<img src="images/Arduino.png">

The below image describes the schematic representation of a microprocessor/ S-o-C architecture, it contains processor/SoC, SDRAM, Vcc/GND, ADC, QSPL1- Flash, UART ports, SPI ports. 

<img src="images/Architecture.png" >

**Processor/SoC:** This system-on-a-chip that is the main component which runs the whole system. 
I2C1, QSPI2, UART1/2, PWM0-5, GPIO00-14: These are peripheral interfaces and though these input/output (I/O) signals are directly connected to the processor/SoC. (Which will be discussed in the later part again). 

**SDRAM:** This represents the synchronous dynamic random-access memory (SDRAM) connected to the processor/SoC.
**I2C0-EEPROM:** This is an I2C (Inter-Integrated Circuit) EEPROM (Electrically Erasable Programmable Read-Only Memory) component connected to the processor/SoC.

**QSPI1-Flash:** This is a QSPI (Quad Serial Peripheral Interface) flash memory component connected to the processor/SoC.

**JTAG-UART-FTDI:** The block represents the JTAG (Joint Test Action Group) interface, UART (Universal Asynchronous Receiver-Transmitter), and FTDI (Future Technology Devices International) components, which are used for debugging, programming, and communication purposes.

#### 1.1.2	Introduction to Risc V 

RISC-V is an open-source instruction set architecture (ISA) based on reduced instruction set computing (RISC) concepts. It has a modular design with several common extensions, enabling for customization based on unique application demands. RISC-V architectures value simplicity, efficiency, and scalability, making them suited for a wide range of applications, from microcontrollers to high-performance computing systems. RISC-V, with its expanding ecosystem and widespread industrial adoption, has the potential to define the future of computer architecture.  The RISC-V instruction set architecture (ISA) is intended to be adaptable, with a core set of 32-bit naturally aligned instructions supplemented with variable-length extensions for increased flexibility. It supports a variety of address space variants, including 32-bit, 64-bit, and a proposed 128-bit flat address space, but has yet to be finalized due to a lack of practical experience.
The below image shows how is the chip connected on the inside. 

 <img src="images/Inside chip.png" >


#### 1.1.3	From Software Applications to Hardware
This picture presents a conceptual overview of a computer system's basic architecture, emphasizing the roles of both software and hardware in enabling the device's complete functioning and capabilities.


 <img src="images/Layers.png" >


The transition from software to hardware has 3 layers. The application layer, this layer includes user-facing software like as productivity apps and other programs with which users interact directly. These apps rely on the underlying system software to deliver functionality and communicate with the hardware.
The software, it consists of the operating system (e.g., Windows 7, Linux) and the compiler, which converts high-level programming languages (C, C++, VB, and Java) into machine-readable instructions.  The assembler is responsible for converting these instructions into the final executable (.exe) file that can be run on the hardware.  The physical computing equipment consists of components such as the processor (CPU), memory, input/output (I/O) devices, and other peripherals. The hardware executes the software's machine-level instructions, doing computations and data processing.

### 1.2	ASIC Design Flow

 <img src="images/open lane asic flow.png">

This picture displays the electronic design automation (EDA) pipeline for producing an integrated circuit (IC) or chip. The procedure begins with the creation of a Register Transfer Level (RTL) description, which is subsequently converted into a gate-level network list. Static Timing Analysis (STA) and Design for Testability (DFT) approaches are used to ensure timing restrictions and testability. The following processes include floor planning, placement, clock tree synthesis, optimization, thorough routing, and different verification tests to ensure that the physical layout matches the logical design. Scripts are used to introduce phony vias and diodes in order to increase production yield. Finally, the design is converted to the GDSII format, which is the standard for semiconductor manufacturing, to complete the comprehensive EDA procedure.

#### 1.2.1	Simplified RTL2GDS flow

The first phase in the flow is synthesis, which converts the RTL code to a netlist. A netlist is a list of a circuit's components and their connections. The next phase is clock tree synthesis (CTS), which is the process of creating a circuit to distribute the clock signal throughout the chip. After the netlist and CTS are finished, the layout and power planning are completed. The floorplan is the circuit layout, and power planning is the process of constructing the circuit such that it has enough power.


 <img src="images/RTStoGDSII flow.png">
 

#### 1.2.3 Introduction to OpenLane 

OpenLane is an open-source, fully automated RTL to GDSII pipeline that employs a variety of popular open-source EDA tools at various phases of the IC design cycle. OpenLane is based on a suite of EDA tools that operate together to enable the entire chip design pipeline, from RTL to final GDSII layout. It consists of : 

| Software | Description |
|-----:|---------------|
|  Yosys	  | Verilog synthesis tool           |
|  Magic   | Layout editor                    |
|  Netgen  | Digital netlist comparison tool  |
|  Fault   | Digital fault simulator          |
|  CVC SPEF-Extractor | Extractor	Circuit verification and analysis tool  |
| CU-GR    | Global routing tool              |
| Klayout  | Mask layout editor and viewer    | 


Files inside open lane directory: 

```
Directory Structure
openlane/
├── AUTHORS.md
├── clean_runs.tcl
├── configuration
├── conf.py
├── CONTRIBUTING.md
├── default.cvcrc
├── designs
├── docker_build
├── docs
├── flow.tcl
├── LICENSE
├── Makefile
├── README.md
├── regression_results
├── report_generation_wrapper.py
├── run_designs.py
└── scripts
```

We have used picorv32a for demonstration of this experiment. The contents of picorv32a are: 
```
picorv32a

├── config.tcl
├── runs
├── sky130A_sky130_fd_sc_hd_config.tcl
├── sky130A_sky130_fd_sc_hdll_config.tcl
├── sky130A_sky130_fd_sc_hs_config.tcl
├── sky130A_sky130_fd_sc_ls_config.tcl
├── sky130A_sky130_fd_sc_ms_config.tcl
└── src
    ├── picorv32a.sdc
    └── picorv32a.v
```

### 1.3 Working of Openlane 


We have to start openlane with the following command: 

``` docker ./flow.tcl – interactive ```  

The interactive term is used to run every stage of the process by us, if we don’t run with interactive then the RTS to GDS II flow will be completed directly. 

Once we run the command, Openlane runs, after we need to run next command in openlane, and it is package require openlane 0.9 and then we have to execute ``` prep -design picorv32a ``` . This will help in preparation of the design. 

It will look like in the below picture once it is successfully executed. 

  <img src="images/preparation stage of openlane.jpeg">

Once the preparation is completed, we will run a command run synthesis

  <img src="images/synthesis run command.jpeg">


The run synthesis command will do the synthesis process and produces the results. Once it is completed, we will be able to see synthesis was successful and it will look like below, 

  <img src="images/Synthesis.jpeg">


After the synthesis is successful, to see the results of synthesis process we can go into following directories, 

```Cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/14-04_16-30/reports/synthesis```

This command would take us to the directory where all the results are stored and it looks as below. 

  <img src="images/report directories.jpeg">

We can use ```less``` command to see what is in the output reports. 

The timing report would look like this; 

  <img src="images/timing report.jpeg" >

The detailed analysis can be performed using the printing statistics that we get from the report. 

 <img src="images/Detailed reports.jpeg">

We can see that dxftp value is 1613 and the total number of wires are 14876 so, the flop ratio is 

``` (value of dxftp/total number of cells) * 100 = (1613/14876) ``` which is approximately 10.84%. 

After the synthesis process, we can also observe the ABC mappings, 

  <img src="images/mapping after synthesis.jpeg">


  ## DAY 2 - Good Floorplan vs Bad Floorplan and Introduction to library cells
  ### 2.1 Chip FloorPlan Considerations
  #### 2.1.1 Utilization factor and Aspect Ratio

This section is basically for the height & width of the core and die. Te basic Idea is to start with a Netlist. A netlist decribes the connectivity of the electronic design. To define the dimensions of the chip, the dimensions of the netlist are required i.e; the logic gates and flip flops used in the chip. 

Considering the standard cell dimensions 1 unit and 1 unit and asuuming the same area for the flipflop too 
This means area = 1unit * 1unit = 1 Sq. units. 

<img src="images/Area.png">

Combining all the flipflops together the length and width would be 2 units and 2 units. Therefore the toatl area would be 4 sq units. 


Core is the fundamental part where the logic is put and the die is the outer layer of the core. Die is a semiconductor material specimen on which the fundamental circuit is fabricated. If the logic completely occupies the core area then the utilization factor would be 100% i.e; 1. 

Utilization Factor can be well described as, 

UF = Area occupied by Netlist/Total area of the core 

Aspect Ratio = Height/Width. 

If the aspect ratio is 1, it means that the chip is square and when AR is any other value then it means the chip is rectangle. 

#### 2.1.2 Concept of Preplaced Cells 

Let's consider an example of a big combinational logic with N logic gates. Breaking down this combinational logic into granular parts i.e; breaking the circuit into 2 blocks and executing them seperately. The I/O pins of them are extended and connected. 

<img src="images/Cut circuit.png">

Each box is backboxed i.e; copied and made invisible to the top netlist. The major advantage of this process is that the black box can be used multiple times on the netlist. The 2 boxes can be given to two seperate users and they can be connected accordingly. 

<img src="images/Black box.png" >

The main concept of the preplaced cells is that the cells are executed only once and they can be reused in the netlist whenever there is a similar kind of requirement.  

This arrangement of the IP's in the chip is called as Floorplanning. SInce they are placed before the placement and Routing, they are called Preplaced-cells. 

#### 2.1.3 Decoupling Capacitors. 

Considering a design background, to define the location of the cells, left side has all input pins and right side has all output pins. From the design summary we can get to know about the I/O pins, the blocks which are communicating with the Input Pins. 

All the blocks A,B,C are placed close to the input side, these cells are placed depending on the design scenario, the location isn't touched. Once they are placed the locations cannot be changed. They should be well designed and they should be observed. 

Considering a circuit. In this case the circuit gets the power supply from the main supply itself, since the main supply is far from the circuit, there would be a voltage drop and losses. So, if we consider 1 volt is taken from Vdd, by the time it reaches the circuit, Vdd' will be 0.7 or 0.8 and the rest of it is drained in the voltage loss. 

<img src="images/Decouple circuit.png">

If the voltage is in undefined region then it might go towards logic 1 or logic 0. 

<img src="images/Noise Margin.png">

Therefore to reduce this, Decoupled capacitors are placed. In this case, the decoupled capacitors act like a charge buffer, they are placed close to the circuit and they provide continous supply to the circuit. When the logic has to be 1 the capacitior discharges and provides the required voltage. When it has to charge, it takes the voltage from main power supply. 

<img src="images/Decoupling Capacitors.png">

This eliminates the voltage drops and continously provides voltage to the circuit making sure it is not in undefined region. The chip looks like this after placing the decoupling capacitors. 

<img src="images/Chip.png">

#### 2.1.4 Power Planning

Considering a complete circuit with 4 Macros and logic. Connecting all the Macros to the power supply, the ground lines are tapped to the ground. Assume the 'red' path is 16 bit bus, and the supply power is source. 
When logic is 1 it is charged to voltage V 

<img src="images/Macros.png">

the inverter changes from logic 0 to logic 1 or vice versa, that means all the capacitord which are charged will be discharged and all the discharged ones will be charged. Since all the charged capacitors are discharged to ground, there is a bounce, and it is called as Ground bounce. If the bounce moves to the undefined region, it might be a problem.

<img src="images/Voltage Bounce.png">

When all the capacitors charge, then there is demand in power supply and then voltage droop occurs. As long as the droop is in Noise Margin, there shouldn't be a problem. This is called mainly because of one power supply for the whole region. Instead of single power supply, we would have multiple to facilitate the power for all the macros. 

<img src="images/Voltage Droop.png" >

#### 2.1.5 Pin Placement and Logical cell placement blockage. 
Circuit 1 is powered by clk1, circuit 2 by clk2, and they have separate inputs (Din1 and Din2) and outputs (Dout1 and Dout2). The preplaced cells and BlockA receive input from Din1, followed by input from Din2. Another set of prepared cells, BlockB, receives input from clk1 and clk2 and outputs clk. As a result, we now have four input ports: Din1, Din2, Clk1, and Clk2, as well as three output ports: Dout1, ClkOut, and Dout2. 

<img src="images/Flip flops.png" alt="Alt Text">

Adding up similar design to the existing circuit, it will have 6 input ports and 5 output ports. This whole timing information and the connectivity are coded in Verilog/VHDL language and it is called a Netlist.

<img src="images/Complete flip flops.png" alt="Alt Text">

Keeping the netlist in the core and filling the space between the core and the die with pin information. The input ports are located on the left side, while the output pins are on the right. The order of placing is "random".

<img src="images/power planning.png" alt="Alt Text">

#### 2.1.6 Steps to run floorplan using OpenLANE

Check the README.md. In the readme file we can see about the Aspect and utilization ratios. We will be able to see the Power Distribution network preset values as well. All the details of floorplan would be given in the readme file. 

#### 2.1.7 Review floorplan files and steps to view floorplan

Next we have to check the floorplan details, 

 <img src="images/florplan default details.PNG" alt="Alt Text">

 Then, we need to run floorplan by using ```run_floorplan``` command

 <img src="images/succesful floorplan.PNG" alt="Alt Text">

 Floorplan def file

 <img src="images/floorplan def.PNG" alt="Alt Text">

 Floorplan image 

 Check the README.md 

 <img src="images/magic.PNG" alt="Alt Text">

#### 2.1.8 Review floorplan layout in Magic

We review floorplan here. The input/output pins are equidistant, and we can hover on the picture and select S, this will select the file and then we can zoom by using ```Z``` and then to zoom out, we should be using ``` Shift + Z ```.

 <img src="images/magnified.PNG" alt="Alt Text">

 <img src="images/what tkcon.PNG" alt="Alt Text">

 <img src="images/what for vertical.PNG" alt="Alt Text">

We can see the buffer from the below image. 


 <img src="images/buffer.PNG" alt="Alt Text">


### 2.2 Library Binding and Placement 
#### 2.2.1 Netlist Binding and Initial Place Design

We have a netlist of gates, and the shapes of the gates represent their functionality. So, if we consider the NOT gate to be a tringular shape, it is actually a box with physical dimensions of width and height. Similarly, the AND gate and flipfops have shapes but they are square boxes. Every logic is essentially assumed to be a square box. All of the gates and flipflops' physical dimensions have now been set. We will provide a certain form with specific dimensions for each component of the netlist since shapes such as AND and OR gates do not exist in the actual world, so we make them square boxes. All blocks will also have a width and height, as well as the right shape.

<img src="images/Placement Big image.png" alt="Alt Text">

A library has all the information related to teh netlist. It will also have the information of the height and width.  Once all the shapes and sizes of the gate are described then all of them are placed on the floorplan. Since there are already preplaced cells, the placement makes sure that they are not disturbed and it will also take care, that are no other cells which are placed in that area. 

<img src="images/Flip flops and Placement.png" alt="Alt Text">

#### 2.2.2 Optimize placement using estimated wire-length and capacitance

In this stage we are estimating the wire length and capacitance, based on these the repeaters are inserted. 

- Repeaters : These are buffers which recondition the signal integrity. One of the con of adding adding repeaters, is that, there is area loss. Using slew we can estimate if signal integrity is maintained or not.

Based on signal transition (slew) we can say that it goes beyond a certain limit. 

In 1st step, There is no need of any repeaters because the are close to each other and the signal intergity is maintained. 
In second step, We would need repeaters/buffers because the distance is more. 

<img src="images/1 and 2.png" alt="Alt Text">

#### 2.2.3 Final placement optimization

In 3rd step too, we would need buffers because 2nd logic gatre and the flipflop 2 is very high and since we have to maintain signal intergity, a buffer has been added. 

In 4th step too, we need buffers becuase the distance is huge between Inputs, gates, and flipflops. 

<img src="images/3 and 4.png" alt="Alt Text">

#### 2.2.4 Need for libraries and characterization

Converting the RTL to hardware is called Logic Synthesis. 
The steps of Logic Synthesis: 

- RTL-Hardware
- Floorplan
- Placement
- Clock Tree Synthesis
- Routing

#### 2.2.5 Congestion aware placement using RePlAce

Here we run the placement. The placement is done in 2 types, global placement and detailed placement, So when we are running placement it means that we are actually running global placement and the main purpose of this placement is to reduce the wire length. 

 <img src="images/placement done.PNG" alt="Alt Text">

 <img src="images/standard cells placement.PNG" alt="Alt Text">

 The zoomed imaged of the Standard cells 

 <img src="images/standard cells zoomed.PNG" alt="Alt Text">

### 2.3 Cell design and characterization flows
#### 2.3.1 Inputs for cell design flow

Gates, flipflops, and buffers are calles as Standard cells in the Cell Design Flow. 'Library' is the section where these standard cells are located. Additionally, there are numerous other cells in the entire set that are varied in size but have the same behavior.

The Cell Design Flow has 3 main parts 
- Inputs
- Design Steps
- Outputs

Inputs required for cell design is PDKs, DRC, LVS rules, SPICE models, libraries, and user defined specs. Inputs that we get from the foundry are called as SPICE model parameters. 

#### 2.3.2 Circuit design step

The circuit design steps has 3 substeps again;
- Circuit design
- Layout Design
- Characterization

Circuit Design must be implemented first, and then the PMOS and NMOS transistors must be modeled in a way that satisfies the library requirements.

<img src="images/Cell design flow.png" alt="Alt Text"> 

### 2.3.2 Layout design step

First, we implement the given function using PMOS and NMOS transistor connections. Then, we derive the PMOS and NMOS network graphs from this implementation, followed by finding the Euler's path for both networks. 

 <img src="images/.png" alt="Alt Text"> 

Using this path, we create a stick diagram, representing inputs and gate connections. This diagram is then converted into a layout while adhering to design rules and user-defined specifications, utilizing Magic. 

 <img src="images/Stick Diagram.png" alt="Alt Text"> 

The next crucial step involves extracting parasitics from the layout to characterize it in terms of timing. Finally, we proceed to characterize the layout, obtaining information on timing, noise, and power. 

 <img src="images/Complete cell design flow.png" alt="Alt Text">

The output typically includes the layout in GDS2 format along with an extracted spike netlist. These steps highlight the complexity involved in even seemingly simple circuit designs, demonstrating the thorough process required for effective layout design.

#### 2.3.3 Typical characterization flow

The characterization flow for the circuit involves several crucial steps to accurately analyze its behavior. First, we gather all necessary inputs, such as the layout, SPICE netlist, and subcircuit models. Then, we proceed with reading the SPICE models and the extracted netlist to understand the circuit's characteristics. Defining the behavior of the circuit is pivotal in this process. Subsequently, we integrate the subcircuit models and establish connections to the power sources to ensure proper functioning. Applying stimulus to the circuit enables us to observe its response under different conditions. Additionally, specifying the output capacitance is essential for accurate characterization. Finally, we execute the simulation by providing the necessary commands, such as transient simulation, to analyze the circuit's performance comprehensively. 

<img src="images/Circuit.png" alt="Alt Text">

### 2.4 General timing characterization parameters
#### 2.4.1 Timing threshold definitions

Timing threshold definitions in a SPICE or characterisation setup are discussed here. The slew low rise and slew high rise thresholds, 
are important to determine the slope of the waveforms. The Slew_low_rise_thr is typically 20% to 30% from the bottom of the line.  These cutoff points play a crucial role in determining slew rates and offer valuable information about waveforms. 

We looked at the importance of the in-rise and out-rise threshold are about 50% mark on the input and output waveforms. Fall waveforms characteristics are also calculated in the similar fashion.  

<img src="images/In and Out rise.png" alt="Alt Text">

<img src="images/Slew.png" alt="Alt Text">

#### 2.4.2 Propagation Delay and Transition Time 

Propagation delay =  output - input threshold (determines how fast a signal propagates through a circuit). 
                  = (out_rise_thr) - (in_rise_thr)

<img src="images/Propagation Delay.png" alt="Alt Text">

Transition time = Low Threshold - High Threshold ( the time it takes for a signal to transition between thresholds). 
                = (slew_high_rise_thr) - (slew_low_rise_thr)

Whenever we see a negative delay, it means that the choice of the points is not right and getting negative delays is not accepted. 

<img src="images/Prop Delay 1.png" alt="Alt Text">

## Day 3 - Design library cell using Magic Layout and ngspice characterization
### 3.1 Labs for CMOS inverter ngspice simulations
#### 3.1.1 IO_Placer Revision

In the floor planning that we obtained, the pins are spaced equally and we can change the distance between the pins using "set" command.
To do that, we first need to look at the switches, and then we must use  "env(FP_IO_MODE) 1" and end up at "env(FP_IO_MODE) 2" and then we can proceed with the floorplanning again.

```magic -T``` can be used to verify the changes. 

#### 3.1.2 SPICE deck creation for CMOS inverter

First we need to create a SPICE netlist. A netlist is a file that contains information about the circuit, including the components and their connections. Once we create a netlist, you can then create a SPICE tag to simulate the circuit.

Initially Substrate Pins are Defined. 

<img src="images/Spice connections.png" alt="Alt Text">

Next Values for PMOS and NMOS are given. 

In the Next step the Nodes are identified. 

<img src="images/Name Nodes.png" alt="Alt Text">

Nodes are properly named, Vin, Vss, Vdd, out.

<img src="images/Nodes.png" alt="Alt Text">

Lastly a Spice Deck is created using the following format, 

Drain - Gate - Source - Substrate 

<img src="images/Spice Deck.png" alt="Alt Text">

#### 3.1.3 SPICE simulation lab for CMOS inverter

Here the Load capacitor and source are described. The supply Voltage Vdd is connected in between Vdd and 0 and It's Value is 2.5V 
Input Voltage is connected between Vin and 0 and has a value of 2.5V 

The Model File has the Complete Description about PMOS and NMOS. 

<img src="images/Spice Deck.png" alt="Alt Text">

After we do Spice Simulation, we get graphs. 

<img src="images/Spice Graph 1 .png" alt="Alt Text">

<img src="images/Spice Graph 2 .png" alt="Alt Text">

#### 3.1.4 Switching Threshold Vm 

We have taken 2 CMOS converters into consideration. For one PMOS width (WP) is 2.5 times greater than the NMOS width (WN) and for another both widths are equal. 

The switching threshold is defined as VIN = VOUT. 

During the switching transition, both the PMOS and NMOS transistors operate in the saturation region and at the switching threshold, the gate-source voltage (VGS) and the drain-source voltage (VDS) are equal. Despite opposite current flow directions in PMOS and NMOS, their magnitudes remain equal, resulting in a net current of zero, like IDSP equals -IDSN.

<img src="images/Spice Waveform.png" alt="Alt Text">

<img src="images/CMOS Roboust.png" alt="Alt Text">

#### 3.1.5 Static and dynamic simulation of CMOS inverter

In our simulation approach, we're using transient analyses with pulse input waveforms to understand how CMOS inverters behave dynamically. By calculating rise and fall delays and determining switching thresholds, we're measuring how fast and reliable these inverters are across different configurations. We're tweaking the sizes of both PMOS and NMOS transistors to see how their proportions affect signal propagation and switching behavior. Through this process, we can understand how transistor sizing impacts the overall performance of CMOS inverters. 

 Time vs Voltage is plotted and we can calculate the fall and rise delay. 

 <img src="images/Spice Waveform 1.png" alt="Alt Text">

 #### 3.1.6 Lab steps to git clone vsdstdcelldesign 

 Here we would be just cloning git, using the below command, 

 ```git clone https://github.com/nickson-jose/vsdstdcelldesign```

 <img src="images/cloning git.PNG" alt="Alt Text">

 This will create a folder vsdstdcelldesign in Openlane directory and copy the file ``` sky130A.tech ``` in this vsdstdcelldesign folder. 

  <img src="images/copying the sky file.PNG" alt="Alt Text">

 The contents of the folder are : 
 
```
├──  Images
├── README.md
├──  LICENSE
├──  extras
├── sky130_inv.mag
├── sky130A.tech
├──  sky130_inv.ext
├── sky130_inv.spice
├── bsim4v5.out
├── sky130_vsdinv.mag
├── sky130_vsdinv.lef
├── libs

```


 CMOS Inverter Magic File 

 
 <img src="images/cell design.PNG" alt="Alt Text">

 ### 3.2 CMOS Fabrication Process
 #### 3.2.1 Create Active regions

The choice of substrate is an important step in the production of semiconductors, and a P-type silicon substrate is frequently selected because of its characteristics, which include high resistivity and certain doping levels. Doping is the process of introducing foreign contaminants to change the properties of a substrate. Maintaining a doping level below the well doping is an important factor to take into account while constructing PMOS and NMOS components independently. To make areas on the substrate active for transistors, there are multiple processes involved. First, silicon dioxide is generated to serve as an insulator, and then silicon nitride is deposited. To stop interference, these layers offer isolation between transistor areas. Then, employing masks, photolithography techniques are used to delineate regions for transistor formation. 

Layers of photoresist shield specific regions during later processing stages. Local oxidation of silicon (LOCOS) is the process by which the silicon dioxide layer is selectively generated in exposed locations after superfluous materials have been etched away. The field oxide is formed by this development, creating electrical isolation between transistor areas. In order to achieve well-defined active and isolating regions—which are crucial for transistor operation and the prevention of undesired electrical coupling—the silicon nitride layer is finally eliminated.

 <img src="images/Psubstrate.png" alt="Alt Text">


 #### 3.2.2 Formation of N-well and P-well

P-well and N-well cannot be done at a same time. When using photoresist for a region, we have to completely close the other region and mask & UV light, patterning of P-well can be performed. 

<img src="images/P well and N well.png" alt="Alt Text">

 #### 3.2.3 Formation of Gate Terminal 

The gate terminal regulates the threshold voltage, which is required for transistor operation. The threshold voltage equation depends on characteristics such as doping concentration and oxide capacitance. The doping concentration and oxide capacitance can be adjusted to reach the desired threshold voltage for transistors. The relationship between fabrication and threshold voltage emphasizes the significance of controlled tests in semiconductor production. Moving onward, the creation of the gate requires maintaining the doping concentration. In a multi-step process, such as a 16-mask process, the fourth mask is used to shield certain parts while exposing others to UV light for chemical action and photoresist removal. These repeating photolithography operations are essential for introducing masks or layouts during production.


#### 3.2.4 Lightly doped drain (LDD) formation

The necessary doping profile, such as P+, P-, and N, must be achieved throughout the fabrication process in order for transistors to function properly. This profile includes P+ for the source and drain in PMOS transistors, P- for Lightly Doped Drain (LDD) creation, and N for the substrate. Similarly, in NMOS transistors, N+ is used for the source and drain, N- is used for LDD creation, and P represents the substrate. The purpose of this profile is to counteract two major effects: the hot electron effect and the short channel impact. When the device size is reduced, the hot electron effect develops, resulting in increased electric fields and energy in carriers, which may cause reliability difficulties.

The short channel effect occurs when drain voltage enters the gate channel, impairing the gate's capacity to control current flow. To counteract these effects, the fabrication procedure involves the creation of LDD structures. This procedure involves using masks to protect certain areas, implanting phosphorous for N-type impurities, and implanting boron for P-type impurities. Side wall spacers formed by anisotropic etching assist preserve the LDD structures during subsequent implantation procedures for source and drain development, ensuring that the proper doping profile is maintained. These specific fabrication techniques are vital in enhancing transistor performance and reliability.

<img src="images/LDD.png" alt="Alt Text">

#### 3.2.5 Source and Drain Formation

Adding a tiny coating of screen oxide prevents ions from reaching too deep during implantation by reversing their path.  We add arsenic to make certain sections of the chip more conductive, and boron to make others less conductive. Then we heat everything up to set the adjustments in place. This ensures that the chip's components perform as planned. 

Finally the CMOS is put into the furnace atleast of 1000 Degrees 

<img src="images/Source and Drain.png" alt="Alt Text">

#### 3.2.6 Local interconnect formation

Here, we begin by removing the thin screen oxide layer that had been used to prevent channeling effects. Next, we use a procedure known as sputtering to deposit titanium metal onto the substrate. This involves hitting the metal with argon gas. We then heat the titanium in a nitrogen environment, forming low-resistance contacts of titanium silicide (TiSi2) and titanium nitride (TiN). These contacts are essential for local connections on the semiconductor. This technique, known as RCA cleaning, ensures that only the desired contacts remain. These contacts can now be connected internally or raised to higher metal levels for further connection.

#### 3.2.7 Higher level metal formation

Higher-level metal interconnects are formed by depositing a thick layer of phosphorus- or boron-doped SiO2. Then, chemical mechanical polishing (CMP) is done to planarize this layer. After that,  SiO2 layer is etched off and contact holes are made using photolithography.  thin coating of titanium nitride (TiN) is deposited first, then thick layer of tungsten is applied, and then CMP is used to polish the layers together. Tungsten is put into the contact holes, and then another layer of metal placed. The necessary contact regions are exposed by designing this metal layer using mask, and any extra metal is removed by etching. Finally, a  layer of Si3N4 is deposited, and mask 16 is used to make the contact holes for bringing the contacts outside the chip. This completes the fabrication processand CMOS is created using multiple layers. 

<img src="images/Final Fabrication.png" alt="Alt Text">

#### 3.2.8 Lab introduction to Sky130 basic layers layout and LEF using inverter

In Sky130, every color represents a separate layer. Here, the first layer is for local connection, which is displayed in blue_purple, the second layer is metal 1, which is shown in light purple, and metal 2 is shown in pink. Solid line indicates N-well. Green represents the N-diffusion area. Red color is for polysilicon gate. Similarly, the brown represents P-diffusion. In the Tckon window, we can see PMOS and NMOS layers after selection. Using these techniques we can tell the CMOS inverter is working well. 

<img src="images/cell design.PNG" alt="Alt Text">

<img src="images/pmos and nmos.PNG" alt="Alt Text">

#### 3.2.9 Lab steps to create std cell layout and extract spice netlist

Here we are using extract all in tckon window. 

<img src="images/extract all.PNG" alt="Alt Text">

After this, the .ext file is placed in ``` vsdstdcelldesign folder ``` , and in tckon window we have to run ``` ext2spice cthresh 0 rthresh 0 ``` and then type ``` ext2spice ```. 

<img src="images/spice file.PNG" alt="Alt Text">

### 3.3 SKY Tech Labs
#### 3.3.1 Lab steps to create final SPICE deck using Sky130 tech 

We have to edit the spice values, the below image shows the values of the spice files. 

<img src="images/spice file with values.PNG" alt="Alt Text">

Then we have to run ngspice using command ``` ngspice sky130_inv.spice ```. 

<img src="images/spice values.PNG" alt="Alt Text">

then we have to ``` plot y vs time a ``` in ngspice terminal 

<img src="images/plot.PNG" alt="Alt Text">

<img src="images/plot y vs time.PNG" alt="Alt Text">

#### 3.3.2 Lab steps to characterize inverter using sky130 model files

Here we can find out all the rise, fall, propagation delays from this. 

<img src="images/Rise and Fall Delay.PNG" alt="Alt Text">

#### 3.3.3 Lab introduction to Magic tool options and DRC rules

To download the labs, we have to go to the home directory and run ``` wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz ```  and we have to extract the file using ``` tar xfz drc_tests.tgz ``` and then we have to drc_tests folder and see if it is properly extracted or not. 

<img src="images/downloading drc.PNG" alt="Alt Text"> 

Tor un Magic we should use, ``` magic -d XR  & ``` command. 

#### 3.3.4 Lab introduction to Magic and steps to load Sky130 tech-rules

To run magic we should the command ``` magic -d XR ``` 

And then we have to add up met3.mag file. 

<img src="images/magic met3.PNG" alt="Alt Text">

Next we will select an area in the file and check ``` drc why ``` in tckon file 

<img src="images/cif see.PNG" alt="Alt Text">

Next, select a blank area and place the cursor over selected place icon. Then execute the command ``` cif see VIA2 ``` in the tkcon tab. Then all the black squares will appear in the selected area. 

<img src="images/black squares.PNG" alt="Alt Text">

#### 3.3.5 Lab exercise to implement poly resistor spacing to diff and tap

We have to edit sky130A.tech file and execute the tkon file and load poly 9 file in that. 

<img src="images/dimensions of emty poly.PNG" alt="Alt Text">

#### 3.3.6 Lab challenge exercise to describe DRC error as geometrical construct 

we have to make changes here in sky130A.tech 

<img src="images/modfing nwell rules.PNG" alt="Alt Text">

<img src="images/nwell.PNG" alt="Alt Text">

#### 3.3.7 Lab challenge to find missing or incorrect rules and fix them

We have to open magic tool and execute the commands drc style drc(full) and drc check and add up tye nsubstrate in the n well. 

<img src="images/nsubstratecontact.PNG" alt="Alt Text">


## Day 4 - Pre-layout timing analysis and importance of good clock tree
### 4.1 Timing modelling using delay tables

#### 4.1.1 Lab steps to convert grid info to track info

Now, we must extract the '.lef' file from the '.mag' file and transfer it into the picorv32a flow. There are certain rules that must be followed while creating standard cells: The input and output ports must be located at the intersection of the vertical and horizontal tracks. The standard cell's width and height should be odd multiples of the track pitch and vertical pitch, respectively.

The file is located in ``` pdk/sky130/libs.tech /openlane/sky130_fd_sc_hd/track.info ``` to get grid coordinates, 

<img src="images/grid coordinates.PNG" alt="Alt Text">

<img src="images/intersection.PNG" alt="Alt Text">



#### 4.1.2 Lab steps to convert magic layout to std cell LEF

The port name along with its values will now need to be defined. Different ports can have their values changed, and we will need to modify the 'attach to layer' as Metal1 for the power and ground ports.

<img src="images/enabling text.PNG" alt="Alt Text">

<img src="images/vsdinv.PNG" alt="Alt Text">



#### 4.1.3 Introduction to timing libs and steps to include new cell in synthesis

We have to run the synthesis using the below commands here, 

```
docker ./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]      
add_lefs -src $lefs
run_synthesis

```

#### 4.1.4 Introduction to delay tables 

Delay tables represent the input transitions and output loads, delay characteristics of every buffer. analysis of these delay tables, will help us to understand the delays caused by various clock tree buffers, considering their fluctuating input transitions and output loads. It's important to understand the timing properties of the buffers involved when applying clock gating in a clock tree. 

<img src="images/Delay table part 1.png" alt="Alt Text">

#### 4.1.5 Delay table usage Part 2

Using logical gates, such as AND or OR gates, to modify the clock signal's flow is known as clock gating. Therefore, it is possible to optimize power consumption by enabling or restricting the clock signal's propagation depending on specific conditions. Delay tables, such as two-dimensional tables (LFOs), are used to record each buffer's delay characteristics by changing the input transitions and output loads.

- Each node should drive the same load to maintain consistency.
- Buffers at the same level should be identical to avoid skew issues.

<img src="images/Delay table CBUF 2.png" alt="Alt Text">

#### 4.1.6 Lab steps to configure synthesis settings to fix slack and include vsdinv

We have to overwrite the picorv32a file and synthesis, 

```

prep -design picorv32a -tag 01-04_12-54 -overwrite

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

echo $::env(SYNTH_STRATEGY)

echo $::env(SYNTH_BUFFERING)

echo $::env(SYNTH_SIZING)

set ::env(SYNTH_SIZING) 1

echo $::env(SYNTH_DRIVING_CELL)

```

<img src="images/overwrite.PNG" alt="Alt Text">

then we to run synthesis using ``` run_synthesis ``` command. 

<img src="images/prep and overwrite.PNG" alt="Alt Text">

We have to modify the config file, 

<img src="images/modified config file.PNG" alt="Alt Text">

<img src="images/reduced slack.PNG" alt="Alt Text">

After this we have to run floorplan and run placement using ``` run_floorplan ``` and  ``` run_placement ``` 

<img src="images/commands used to run floorplan successfully.PNG" alt="Alt Text">

We can see the placement in the below image, 

<img src="images/modified placement.PNG" alt="Alt Text">

zommed image, 

<img src="images/zoomed floorplan.PNG" alt="Alt Text">

the ``` sky_vsdinv ``` 

<img src="images/sky_vsdinv.PNG" alt="Alt Text">

the tkcon image 

<img src="images/expand.PNG" alt="Alt Text">

The final placement looks like this, 

<img src="images/placement sky130A.PNG" alt="Alt Text">

### 4.2 Timing analysis with ideal clocks using openSTA
#### 4.2.1 Setup timing analysis and introduction to flip-flop setup time

Before going on to real clocks in timing analysis using ideal clocks, we first need to understand its basic components and characteristics. Initially, we have a clock frequency of 1 GHz and clock period of 1 ns. Considering an example with an ideal clock network, a launch flop, a capture flop, and combinational logic between them. In order to ensure correct functioning with frequency, the combinational delay between the flops must be less than the clock period. This setup time decreases the effective time available for combinational logic operations in each clock period. As a result, the combinational delay is changed to allow setup time, making sure that the system functions consistently at the required frequency.

<img src="images/Jitter 1.png" alt="Alt Text">

#### 4.2.2  Introduction to clock jitter and uncertainty

Jitter is a temporary variations in the clock period due to inherent variations in clock source.  These variations impact the effective time available for combinational logic operation within each clock period. Along with Jitter we have, setup uncertainty (SU) , setup time (S), adjusting the combinational delay. With the clock period (T) set at 1 ns, setup time at 10 ps (0.01 ns), and uncertainty at 90 ps, the combinational delay is roughly 1.9 nanoseconds. 

<img src="images/Jitter.png" alt="Alt Text">


#### 4.2.3 Lab steps to configure OpenSTA for post-synth timing analysis

Here again we would configure the Post STA configurations, this step is important to check if picorv32a has timing violations. 

we have to create a new sta config file using ``` vi pre_sta.conf ``` and then place in src directory. 

<img src="images/sta config file.PNG" alt="Alt Text">

Then we need to create my_base.sdc file to define all the environment variables and place it in the src directory. 

<img src="images/my base file.PNG" alt="Alt Text">

Then run the pre_sta.conf file using sta ```pre_sta.conf```  command to observe the slack. 

<img src="images/sta config 1.PNG" alt="Alt Text">

<img src="images/sta config 2.PNG" alt="Alt Text">

#### 4.2.4 Lab steps to optimize synthesis to reduce setup violations

Here we have to overwrite the picorv32a file and then add FANOUT Parameters. We can use the below commands and run the synthesis again.  

```
prep -design picorv32a -tag 18-04_20-08 -overwrite

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

set ::env(SYNTH_SIZING) 1

set ::env(SYNTH_MAX_FANOUT) 4

echo $::env(SYNTH_DRIVING_CELL)

run_synthesis

```
I forgot to take the screenshots after running the synthesis part again, so I'm adding up the history of what commands I have run. 

<img src="images/max fanout.PNG" alt="Alt Text">

next, we have to run the pre_sta.conf again and check the values. 


#### 4.2.5  Lab steps to do basic timing ECO

Here we have to replace sky130_fd_sc_hd__or3_2 with sky130_fd_sc_hd__or3_2

So first we would report all the connections to net 

``` report_net -connections _11672``` 

Next we would replace sky130_fd_sc_hd__or3_2

<img src="images/replacing cell.PNG" alt="Alt Text">

To generate the timing output ``` report_checks -fields {net cap slew input_pins} -digits  ```

<img src="images/reduced slack 23.PNG" alt="Alt Text">

### 4.3 Clock tree synthesis TritonCTS and signal integrity
#### 4.3.1 Clock tree routing and buffering using H-Tree algorithm


While doing the first connections, the connections to the clock port and flipflops were made in such a way that they formed a structure but the distribution wasn't equal and resulted in skew. Therefore to reduce the skew, H tree algorithm is used, it takes the midpoint of the clock path and constructs a balanced a tree structure, making sure that the clock time are similar at all flip flops, at the same time some buffers are also added to ensure signal integrity and reduces the losses because of resistance and capacitance. 

<img src="images/CTS with all buffers.png" alt="Alt Text">

#### 4.3.2 Clock tree synthesis TritonCTS and signal integrity

In our clock set up, we verified that all of the clock's factors, such as its starting and ending points, were exactly in sync.  However, we must also be careful about issues such as crosstalk, which can interfere with our clock signals and create errors. To prevent this, we are going to put a protective shield around our clock signals, similar to building a fence to keep them safe. The fence avoids unwanted interference and ensures that our clock signals are correct. It's just like adding an extra layer of protection so that everything runs correctly.

<img src="images/Shield.png" alt="Alt Text">

#### 4.3.3 Lab steps to run CTS using TritonCTS

Now we have to run the cts. But before that we have to run the whole process from preparation to placement again and then we have to run the CTS. 

Here are overwriting the previous verilog file that was written and later to that we are adding the following commands. 

```

   prep -design picorv32a -tag 18-04_20-08 -overwrite
   set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
   add_lefs -src $lefs
   set ::env(SYNTH_STRATEGY) "DELAY 3"
   set ::env(SYNTH_SIZING) 1
 

```

<img src="images/setting up openlane.PNG" alt="Alt Text">

<img src="images/overwrite verilog.PNG" alt="Alt Text">

<img src="images/overwritten file.PNG" alt="Alt Text">

Next we running synthesis using ``` run_synthesis ``` command 

<img src="images/synthesis Improvement.PNG" alt="Alt Text">

Next we would run the floorplan uisng ``` run_floorplan ``` command.  I encountered an error while perfoming the floorplan operation, the floor plan was initially failing, so I ran the below commands to remove the error and run successful floorplan. 

```
   init_floorplan
   place_io
   tap_decap_or

```

<img src="images/floor plan error.PNG" alt="Alt Text">

<img src="images/init.PNG" alt="Alt Text">
   
Next we have to run placement using ``` run_placement ``` command. 

<img src="images/cts.PNG" alt="Alt Text">

After CTS is successfully run, it would show something like this 

<img src="images/cts run.PNG" alt="Alt Text">


### 4.3.4 Lab steps to verify CTS runs 

To verify CTS Rus, we must use ``` OpenRoad```.  

We have to read the merged.lef file 

``` read_lef /openLANE_flow/designs/picorv32a/runs/18-04_20-08/tmp/merged.lef ```

Then we should write pico_cts.db 

``` write_db pico_cts.db ``` 




### 4.4 Timing analysis with real clocks using openSTA
#### 4.4.1 Setup timing analysis using real clocks 

Using hold timing analysis we want to make sure that the data doesn't change too quickly after it's captured by the capture flop. 

1+2=∆1 and 1+3+4=∆2 and (∆1-∆2)=skew
In these cases we have to consider the propogation skew (s) and uncertainty delay (US). If (Data required time)- (Data arrival time) =  -ve then it is called Slack.  

In hold time analysis the pulse is sent to both launch and capture flip flop.

#### 4.4.2 Hold timing analysis using real clocks

Combinational Delay > hold time of capture flip flop. Right after clk reaches to launch flip flop, it takes 2buffer delay (∆1) and when it reaches to capture flip flop, it takes 3buffer delay (∆2). In this cases the uncertinity is same for both flip flops. 

#### 4.4.3 Lab steps to analyze timing with real clocks using OpenSTA

```
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/18-04_20-08/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/18-04_20-08/results/cts/picorv32a.cts.def
write_db pico_cts.db
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/18-04_20-08/results/synthesis/picorv32a.synthesis_cts.v
read_liberty /openLANE_flow/vsdstdcelldesign/libs/sky130_fd_sc_hd__typical.lib
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
report_clock_skew -hold
report_clock_skew -setup
exit
```
<img src="images/imp.PNG" alt="Alt Text">

#### 4.4.4 Lab steps to execute OpenSTA with right timing libraries and CTS assignment

We need to check the current value of the cts_clk_buffer_list 

```echo $::env(CTS_CLK_BUFFER_LIST)```

After this we should see the current def value. 

``` echo $::env(CURRENT_DEF) ``` 

Then we should run the Cts

``` run_cts ```

 Once the cts is executed successfully it will look like this. You will see " Clock Tree Synthesis was successful". 

 <img src="images/cts run.PNG" alt="Alt Text">


#### 4.4.5 Lab steps to observe impact of bigger CTS buffers on setup and hold timing 

We should follow the below commands: 

```
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/18-04_20-08/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/18-04_20-08/results/cts/picorv32a.cts.def
write_db pico_cts.db
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/18-04_20-08/results/synthesis/picorv32a.synthesis_cts.v
read_liberty /openLANE_flow/vsdstdcelldesign/libs/sky130_fd_sc_hd__typical.lib
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
report_clock_skew -hold
report_clock_skew -setup
exit
```

## Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA

### 5.1 Power Distribution Network and routing 
#### 5.1.1 Lab steps to build power distribution network 

The following commands are used for generating at ```pdn``` 

```
docker ./flow.tcl  -- run openlane

package require openlane 0.9 -- get the package

prep -design picorv32a -tag 18-04_20-08-- prepare the design

we have to always make sure to run the prep again before geerating Power Distribution Network (PDN)

echo $::env(CURRENT_DEF) -- get the current def

gen_pdn -- generate the pdn

```

<img src="images/gen pdn.PNG" alt="Alt Text" >


#### 5.1.2 Basics of global and detail routing and configure TritonRoute 

The final Step of the process is routing. It takes a very long time to run, once done, it displays the values of wns and tns. 

the command to perform routing is ```run_route```. 

Once the routing is done, it would look something like this. 

<img src="images/routing done.PNG" alt="Alt Text" >


#### 5.1.3 Routing topology algorithm and final files list post-route 

After routing we would be checking if the slack requirements are met. Then we would verify our Final picorv32a design with all the connections. 

The below commands are used for the poste route check. 

```
/opening to Openroad
openroad

/reading lef file 
read_lef /openLANE_flow/designs/picorv32a/runs/18-04_20-08/tmp/merged.lef

/reading def file 
read_def /openLANE_flow/designs/picorv32a/runs/18-04_20-08/results/cts/picorv32a.cts.def

/write db
write_db pico_cts.db

/read db
read_db pico_cts.db

/read the verilog file
read_verilog /openLANE_flow/designs/picorv32a/runs/18-04_20-08/results/synthesis/picorv32a.synthesis_cts.v

/read lib file
read_liberty /openLANE_flow/vsdstdcelldesign/libs/sky130_fd_sc_hd__typical.lib

/ link the design
link_design picorv32a

/reading sdc 
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

/read spef file 
read_spef /openLANE_flow/designs/picorv32a/runs/18-04_20-08/results/routing/picorv32a.spef

```

<img src="images/after route.PNG" alt="Alt Text" >

<img src="images/report checks and read spef.PNG" alt="Alt Text" >

<img src="images/met slack.PNG" alt="Alt Text" >

<img src="images/final route cells.PNG" alt="Alt Text">

<img src="images/pico file.PNG" alt="Alt Text" >


## References 

- Skywater PDK [https://github.com/google/skywater-pdk)] 
- Github [https://github.com]
- Cell Design [https://github.com/nickson-jose/vsdstdcelldesign]
- Opnelane [https://openlane.readthedocs.io/en/latest/flow_overview.html]
- Skywater PDK Docs [https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#poly]
- Notes References [https://github.com/kmkalpana2001/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING?tab=readme-ov-file]
- Images Source [https://vsdiat.com/dashboard]
- Content Source [https://www.vlsisystemdesign.com/wp-content/uploads/2017/07/Introduction-to-Industrial-Physical-Design-Flow.pdf]

## Acknowledgement 

I would like to extend my gradtitude to Mr.Kunal Gosh, Mr.Nickson Jose, Mr.Mohamad Shalan, and Mr.Tim Edwards for their outstanding mentorship and insightful presentation during the DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING workshop. The workshop was expertly crafted and flawlessly executed, providing me with invaluable insights and perspectives. The workshop not only imparted knowledge but also provided fresh perspectives, enriching my understanding of the subject. My heartfelt thanks go out to Mr. Kunal Ghosh and Mr. Nickson Jose for their unwavering commitment towards learning and contributing to the success of this endeavor. 


