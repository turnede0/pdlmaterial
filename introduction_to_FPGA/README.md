
  
  

# Introduction to FPGA

  

## Introduction to Electronic Circuit

Electronic circuits in general can be divided into 2 categories: analog circuits and digital circuits.

  

![School electronic: Difference between Analog and Digital circuits](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhvywgcV4LYdufLiolPm5zDeKMtEH7wLQMDD2LGe_KXqMvzY7PTt-pWmW9zlVjX_OQiINitBKjcBGtqU3tvJbqRbdVWN0dMfBoUTA8yjYstEWoHSOo_T8cfg5J_sT3zXL9IDGonfnRYuOQ/s1600/s.jpg)

  

### Analogue circuit

An analog circuit processes and manipulates **continuous** electronic signals using components such as resistors, capacitors, inductors, and transistors.

  

Example: Power modules, Amplifier, Radio Frequency Application Circuit

  

### Digital circuit

A digital circuit processes **discrete** signals represented by binary digits (0s and 1s)

It uses components such as logic gates, flip-flops, and registers, which are also mainly constructed by transistors.

  

Example: Microprocessor, Digital Signal Processor, Graphical Processing Unit(GPU), Programmable Logic Device

  

In this course, we will discuss mainly digital circuits.

  



## Common Circuit vs Integrated Circuit

![Difference Between Discrete Circuits And Integrated Circuits](https://www.elprocus.com/wp-content/uploads/2017/02/featuerd-image.png)

  

**Common circuit** is built using **discrete electronic components** like resistors, capacitors, transistors, and other components. These components are individually assembled and interconnected on a PCB (Printed Circuit Board) or breadboard.

  

**Integrated circuit** or **Chip** is a **single semiconductor device** that integrates multiple electronic components, such as transistors, resistors, capacitors, and interconnections circuits. ICs are fabricated using various semiconductor manufacturing processes, such as photolithography and chemical vapor deposition.

  

## Foundation of Integrated Circuit - Transistor

Transistors are semiconductor devices used to amplify or switch electronic signals and electrical power. There are two commonly used transistors: **Bipolar Junction Transistor(BJT)** and **Metal-Oxide-Semiconductor Field-Effect Transistor(MOSFET)**.

  Example: MOSFET and BJT IC
![Understanding the Difference Between BJT and MOSFET and How to Select the  Right One for Your Designs](https://circuitdigest.com/sites/default/files/field/image/Difference-Between-MOSFET-and-BJT.jpg)


Imagine you are going to build a simple switch circuit. Surely you can use a mechanical switch. However, physical presses are required for activating or deactivating the circuit. What if pressing is not allowed or compatible in your application scenario (e.g. the switch circuit implemented underground)?
![schoolphysics ::Welcome::](https://www.schoolphysics.co.uk/age11-14/Electricity%20and%20magnetism/Current%20electricity/text/Switches_/images/2.png)
By using a transistor, we can create an electrical signal-controlled "switch" circuit. The following example is done by an NPN BJT.

  

Example: An opened "switch" implemented by NPN BJT

  

![Transistor as a Switch - Using Transistor Switching](https://www.electronics-tutorials.ws/wp-content/uploads/2013/09/tran46.gif)

  

Example: A closed "switch" implemented by NPN BJT

  

![transistor switch in saturation](https://www.electronics-tutorials.ws/wp-content/uploads/2018/05/transistor-tran47.gif)

  

By using a transistor, we can build an amplifier circuit, which is the basis of **Anlogue Integrated Circuit** , or a switch circuit, which is the basis of **Digital Integrated Circuit**.

  

We use multiple transistors to form different kinds of digital logic gates, which are the basic building blocks from all digital circuits and microprocessor-based systems.

  
## Introduction to Digital Circuit

  

### Logic Gate

Digital Circuit manipulate electronic input and output signal using logic gates. Engineers will utilize different combinations of logic gates can fullfil certain tasks.

The basic structure of a logic gate are **2 input signals**, **1 output signals** and the logic gate itself.

![Ch 3. Logic gates and logic circuits – IGCSE Computer Science](https://michellescomputerscience.wordpress.com/wp-content/uploads/2016/08/logic-gates.jpg)
  
 
Example: An AND gate circuit implemented by 2 NPN BJT

  

![Logic AND Gate Tutorial with Logic AND Gate Truth Table](https://www.electronics-tutorials.ws/wp-content/uploads/2018/05/logic-log43.gif)


### Multiplexer (MUX)

 Multiplexer allow engineers to use **input control signals** switch the **output signal** between different **input signals**, acting as an mechanical dip switch.
![](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b2/Multiplexer2.png/300px-Multiplexer2.png)
Example: A MUX IC

![74HC157 Multiplexer IC Pinout, Features, Equivalent & Datasheet](https://components101.com/sites/default/files/components/SN74HC157N-Multiplexer.jpg)

### Lookup Table (LUT)
Lookup Table map all the possible combination of input signal to a programmed-set of output signals. Lookup Table is commonly built by **Read-Only Memory(ROM) device**, where a logic gates combination circuit will be mapped to the lookup table in the ROM. 

![](https://image.slidesharecdn.com/introductionadigitalcircuitdesignisjustanideaperhapsdrawnonpaperweeventuallyneedtoimplementthecircui-230303102953-cb44f67a/85/Introduction-A-digital-circuit-design-9-320.jpg)

### Flip-Flop
While Logic Gates, MUXs, Lookup Tables can handle most of the decision-making tasks, they lack the ability to **store the input signal as data ** in the form of ** memory**.

Flip-Flop is a basic building block used to construct more complex digital systems, such as counters, registers, and block memory.
 
 Example: A commonly used Data Flip-Flop
![](https://qph.cf2.quoracdn.net/main-qimg-0a8ada5b5090c21af7b9618c08edda81-lq)

Example: A Data Flip-Flop IC
![Introduction to 74LS74 Dual D Flip-Flop Pinout, Features and working — The  Engineering Knowledge | by Engineering Knowledge | Medium](https://miro.medium.com/v2/resize:fit:902/0*9Rce6AMJYgqM0HB-.jpg)
  

## Development of Integrated Circuit

  ![Integrated Circuits - SparkFun Learn](https://cdn.sparkfun.com/assets/7/a/6/9/c/51c0d009ce395feb33000000.jpg)

The development of integrated circuits and chips can be easily illustrated by the **Scale of Integration**, which distinguishes how many transistors can be fitted into a defined surface area on a semiconductor material.

  

| Scale of Integration |Name | Year | Transistor count | Logic gates number | Example |

|---|---|---|---|---|---|

| SSI| _small-scale integration_ | 1964 | 1 to 10 | 1 to 12 | Flip-Flops |

| MSI | _medium-scale integration_ | 1968 | 10 to 500 | 13 to 99 |Counters |

| LSI |_large-scale integration_ | 1971 | 500 to 20 000 | 100 to 9999 | Intel 4004(First mircroprocessor) |

|VLSI|_very large-scale integration_ | 1980 | 20 000 to 1 000 000 | 10 000 to 99 999 | System on Chip(SoC) |

|ULSI|_ultra-large-scale integration_| 1984 | 1 000 000 and more | 100 000 and more | AMD Ryzen 9 |

  
## Digital IC in nutshell: Parallel vs Sequential Processing
A video to emulated how CPU and GPU work in a fun way! 
[Adam and Jamie Paint the Mona Lisa in 80 Milliseconds! (HD) - YouTube](https://www.youtube.com/watch?v=WmW6SD-EHVY&t=8s)
  

## Programmable Logic Device(PLD)

Programmable Logic Devices are digital ICs with numerous digital logic gates built in. Engineers and developers can design and program the digital logic inside it using **Hardware Description Language(HDL)**.

### Read-Only Memory(ROM)

![Look-up Tables | Principles Of Digital Computing | Electronics Textbook](https://www.allaboutcircuits.com/uploads/articles/256-addressable-memory-locations-in-ROM-chip.jpg)

Before the digital logic array IC, engineers tried to use ROM to represent combinational logic circuits as **Lookup Table**. Electrically Erasable Programmable ROM(EEPROM) was commonly used. However, using a memory device to represent digital logic circuits is slow and cost-ineffective. Thus, an array circuit built by numerous interconnected logic gates is widely used for PLD instead.

 Example: A EEPROM IC
![EEP ROM IC, For Electronics at Rs 50/piece in Thane | ID: 21188274848](data:image/jpeg;base64,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)

### Programmable Logic Array(PLA) and Programmable Array Logic(PAL)

Both PLA and PAL consist of an AND Array and an OR Array.

  

Difference is:

  

![classes](./PAL_circuit.png)

- In a PAL, the AND array is fixed, but the OR array is programmable.

  

![classes](./PLA_circuit.png)

- In a PLA, both the AND and OR arrays are programmable.

  
  

PAL or PLA can be programmed by **anti-fuse technology(irreversible)** or EEPROM.

Example: A PLA IC
![C64 PLA implemented in VHDL](https://frank-buss.de/c64/pla/c64-pla-chip.jpg)
  

## Field Programmable Gate Array (FPGA)

![FPGA Design, Architecture and Applications (Updated) [2024]](https://www.logic-fruit.com/wp-content/uploads/2021/12/FPGA-Graphics-1.jpg.webp)

  

Based on PAL and PLA, FPGA is developed to fulfill more complex and large-scale digital circuit applications. The key feature of FPGA is "field", which means the digital circuit design can be reprogrammed outside the factory, which gives us greater flexibility and agility in utilizing it.

Example: Xilinx Artix-7 FPGA development board
  ![Digilent Basys3 Xilinx Artix-7 FPGA Board](https://m.media-amazon.com/images/I/71mTcGhDgHL.jpg)

### Structure of a FPGA

1. Logic Blocks:

![classes](./CLB.png)

- FPGAs consist of a large number of configurable logic blocks (CLBs) that can be interconnected to implement various logic functions. Each CLB typically contains Lookup Tables(LUTs), flip-flops, and multiplexers.

2. Interconnect:

- The interconnect structure of an FPGA comprises a network of programmable routing resources that allow signals to be routed between the CLBs. This interconnect provides flexibility in establishing connections between different logic blocks based on the desired circuit configuration.

3. Configuration Memory:

- FPGAs use configuration memory to store the programming information that determines the functionality of the device. This memory is used to configure the interconnect resources, logic blocks, and other functional elements within the FPGA.

4. I/O Blocks:

- Input/output blocks (IOBs) provide the interface between the internal logic of the FPGA and the external world. These blocks allow the FPGA to communicate with external devices, such as sensors, memory, or other integrated circuits.

5. Clock Distribution:

- FPGAs have dedicated resources for distributing clock signals throughout the device. These resources help in ensuring synchronous operation of the internal logic elements and facilitate the implementation of complex synchronous designs.

6. Embedded RAM and DSP Blocks:

- Some FPGAs include embedded memory blocks and digital signal processing (DSP) blocks to provide additional functionality for specific applications that require memory or digital signal processing capabilities.

## Application Specific Integrated Circuit(ASIC)

Unlike PLA and FPGA that can be programmed to meet a variety of use case requirements after manufacturing, ASIC designs are tailored early in the design process to address specific needs. ASIC is expensive, and consumes much time to develop and manufacture. Thus, it are only used in very specific and dedicated task in an electronic system.

  

It is very common for chip engineers to use FPGA to test and evaluate an ASIC design before manufacturing the acutal ASIC.

  

Example: Targeted AMD ASIC dedicated for Interactive Streaming

![AMD Rolls Out 5 nm ASIC-based Accelerator for the Interactive Streaming Era - News](https://www.allaboutcircuits.com/uploads/articles/AMD_5nm_ASIC_based_NAB_chip_BIG.jpeg)

  

## General Purpose Processor

Central Processing Unit (CPU), Microcontroller Unit (MCU), and Microprocessor are the common general-purpose processors,

 Example: AMD Ryzen 9 CPU
![7 Reasons AMD Processors are better than Intel | by Digitplus CRFT | Medium](https://miro.medium.com/v2/resize:fit:1200/1*xzKZKOQO2bi7ii5z_2Zkdg.jpeg)

Unlike PLDs, a General purpose processor is designed to run a large variety of general instructions to cater to the most number of scenarios as possible. Thus, many logic gates are compiled together to form defined function blocks, such as **Control Unit**(CU), **Arithmetic Logic Unit**(ALU), **Registers**.

  

### Working Process of General Purpose Processor



#### 1. **Fetch**

  

- The Control Unit (CU) retrieves ("fetches") the next instruction to be executed from the main memory (RAM) and places it into the Instruction Register (IR).

- The address of the instruction is stored in the Program Counter (PC), which is then incremented to point to the next instruction.

  

#### 2. **Decode**

  

- The fetched instruction is sent to the Instruction Decoder within the Control Unit.

- The Control Unit interprets the instruction to understand what operation is to be performed and which resources are required.

  

#### 3. **Execute**

  

- The decoded instruction is sent to the appropriate part of the CPU. For instance, if it’s an arithmetic operation, it will be sent to the ALU.

- The required data is fetched from the registers or memory.

- The ALU performs the required operation (addition, subtraction, logical operations, overflow detection, etc...).

- The result of the operation is stored back in a register or memory.

  

#### 4. **Write Back**

  

- The result of the execution stage is written back to the appropriate place, such as a register or a memory location.

- This step updates the status registers and flags (like zero, carry, overflow) based on the result of the operation.

  
  

## Comparison between different Digital IC

  

![FPGAs, Deep Learning, Software Defined Networks and the Cloud: A Love Story Part 1 | by Jamal Robinson | Medium](https://miro.medium.com/v2/resize:fit:1400/1*ROC6Psob0nLEMZ1igILpwA.png)

  

All the abovementioned digital IC can be compared in 2 parameters: Flexibility and Efficency.

  

As mentioned before, general purpose processor's architecture is designed to run a large variety of general instructions to cater to the most number of scenarios as possible. However, many steps are required for running a task in a general-purpose processor, resulting in an efficiency bottleneck.

  

FPGA and ASIC on the other hand, can be designed for specific repetitive applications or task in a very efficient way.

  

## Multiprocessor System-on-Chip(MPSoC)

Multiprocessor System-on-Chip is a technology to integrate general processor(CPU, GPU), FPGA, and DSPs into one system chip. With MPSoC, engineers and developers can allow different processors to cooperate with each other. General processes can assign repetitive tasks to a dedicated processor, such as FPGA and GPU, such that they can accelerate the task. Thus, the whole system can work in a faster speed and greater efficiency.

### Key Application of MPSoC:

AI and Machine Learning, Automotive, Telecommunication

## Zynq® UltraScale+™ MPSoC and Ultra96

  

![](https://www.amd.com/content/dam/amd/en/images/illustrations/2617829-zynq-eg-block.jpg)

  

Developed by AMD Xilinx, Zynq® UltraScale+™ MPSoC combines the capabilities of a traditional FPGA in the name of "Programmable Logic" with a high-performance processing system, making it a powerful and versatile platform for a wide range of applications.

  
Example: Zynq® UltraScale+™ MPSoC used in our main course hardware: Avnet Ultra96-V2 development board
![Avnet Ultra96-V2](https://china.xilinx.com/content/dam/xilinx/imgs/prime/ultra96-v2-front-view.jpg)

  



  

## AMD Xilinx Vivado

![设计输入和实现](https://china.xilinx.com/content/xilinx/zh/products/design-tools/vivado/implementation/_jcr_content/root/parsysFullWidth/xilinxflexibleslab_1/xilinxflexibleslab-parsys/xilinxtabs/childParsys-methodology/xilinxcolumns_copy_c/childParsys-1/xilinximage.img.png/1697734151140.png)

  

To design, implement, and verify digital logic circuits for the Programmable Logic in Zynq® UltraScale+™ MPSoC or Xilinx FPGA, Xilinx Vivado design suite is primarily used.

### Key Features and capabilities of the AMD Vivado

1. Integrated Design Environment (IDE):

2. High-Level Synthesis (HLS)

3. IP Integrator

4. Implementation and Optimization

5. Verification and Debugging

## PYNQ

  

PYNQ stands for Python Productivity for Zynq. It is an open-source project that enables programming MPSoCs with Python.

  

A great tool for prototyping and exploring FPGA designs using a high-level language like Python.

  
  
  

## Initializing PYNQ on Ultra96

Follow the documentation to connect Ultra96 to your computer:

  

https://ultra96-pynq.readthedocs.io/en/latest/getting_started.html

  

In your browser, connect to http://192.168.3.1/. Enter Login password: **xilinx** to start using Jupyter Notebook.

  

## Using Juypter Notebook in PYNQ

  

Juypter Notebook is used as the GUI IDE for PYNQ. Juypter Notebook support running Python notebook and Linux terminal.

![class](https://imgur.com/C0vT626.png)

  

### Open and Run a Displayport example notebook

1. Connect MiniDisplayport to DP-to-HDMI converter, HDMI cable, and HDMI Video Capture card

2. Connect HDMI Video Capture card to **your computer's** USB port

3. Connect the Webcam to **Ultra96's** USB port

4. Open "common" folder

5. In the "common" folder, open "display_port_introduction.ipynb" notebook

  

![Imgur](https://imgur.com/FZDMTYC.png)

  

6. Make sure you connect everything properly and open the "Camera" application in your computer.

  

![Imgur](https://imgur.com/4kXYJsH.png)

  

7. Run the whole notebook to display camera output to your computer

  

![Imgur](https://imgur.com/wwttGSc.png)

  

### Using Linux Terminal in Juypter Notebook in PYNQ

  

Go back to the Homepage. Initialize a new terminal by pressing New > Terminal.

  

![Imgur](https://imgur.com/NbkQppz.png)

  

![Imgur](https://imgur.com/vy7DCOa.png)

  

### Common Linux Command

1.  `ls`: Lists the contents of a directory.

2.  `cd`: Changes the current working directory.

3.  `mkdir`: Creates a new directory.

4.  `rm`: Removes (deletes) files or directories.

5.  `cp`: Copies files or directories.

6.  `mv`: Moves or renames files or directories.

7.  `cat`: Displays the contents of a file.

8.  `nano`: A text editor for editing files.

9.  `ssh`: Connects to a remote machine via Secure Shell (SSH).

10.  `chmod`: Changes the permissions of a file or directory.

11.  `sudo`: Executes a command with superuser (administrative) privileges.

12.  `grep`: Searches for a pattern in a file or output.

13.  `top`: Displays the running processes and their system resource usage.

14.  `apt-get`: The package manager for Debian-based Linux distributions (e.g., Ubuntu).

15.  `systemctl`: Manages system services (start, stop, enable, disable).