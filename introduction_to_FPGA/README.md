
# Introduction to FPGA

  
## Digital Circuit
Electronic Circuit in general can be divided into 2 main catogories: analogue circuit and digital circuit.
### Analogue circuit
An analog circuit processes and manipulates **continuous** electronic signals, uses components such as resistors, capacitors, inductors and transistors.

Example: Power modules, Amplifier, Radio Frequency Circuit

### Digital circuit
A digital circuit processes **discrete** signals represented by binary digits (0s and 1s)
It uses components such as logic gates, flip-flops, and registers, which is alos mainly contrusted by multiple transistors.

Example: Microprocessor, Digital Signal Processor, Graphical Processing Unit, Programmable Logic Device

In this course, we will focus mainly on digital circuit.

## Development of Integrated Circuit

The advancement of Integrated Circuit and Chip can be easily illustrated by the Scale of Integration. Meaning, to fit more and more transistor into a defined surface area. 

### SSI Small-Scale Integration
### MSI Medium Scale Integration
### LSI Large-Scale Integration
### VLSI Very Large-Scale Integration
### ULSI Ultra Large-Scale Integration
### GSI Giga-Scale Integration

## Digital IC

## General Purpose Processor 
CPU and GPU, MCU

## Application Specific Integrated Circuit(ASIC)
Digital Signal Processor

## Programmable Logic Device(PLD)

### Read-Only Memory(ROM)
Before logic array IC, engineer tried to use ROM to represent combinational logic circuit. Electrically Erasable Programmable ROM(EEPROM) is commonly used. However, using memory device to represent digital logic circuit is slow. Thus, array circuit built by numerous interconnected logic gates is widely used for PLD instead.

### Programmable Logic Array(PLA) and Programmable Array Logic(PAL)
Both PLA and PAL consist of an AND Array and an OR Array.

Difference is:

![classes](./PAL_circuit.png)
- In a PAL, the AND array is fixed, but the OR array is programmable.

![classes](./PLA_circuit.png)
- In a PLA, both the AND and OR arrays are programmable.


PAL or PLA can be programmed by antifuse technology(irreversible) or EEPROM.


## Field Programmable Gate Array FPGA

Based on PAL and PLA, FPGA is developed to fulfil more complex and large scale digital circuit appplication. The key feature of FPGA is "field", which means the digital circuit design can be reprogrammed outside factory, which give us greater flexible and agile in utilizing it.

1.  Logic Blocks:
    ![classes](./CLB.png)
    -   FPGAs consist of a large number of configurable logic blocks (CLBs) that can be interconnected to implement various logic functions. Each CLB typically contains Lookup Tables(LUTs), flip-flops, and multiplexers.
    
2.  Interconnect:
    
    -   The interconnect structure of an FPGA comprises a network of programmable routing resources that allow signals to be routed between the CLBs. This interconnect provides flexibility in establishing connections between different logic blocks based on the desired circuit configuration. 
    
3.  Configuration Memory:
    
    -   FPGAs use configuration memory to store the programming information that determines the functionality of the device. This memory is used to configure the interconnect resources, logic blocks, and other functional elements within the FPGA.
4.  I/O Blocks:
    
    -   Input/output blocks (IOBs) provide the interface between the internal logic of the FPGA and the external world. These blocks allow the FPGA to communicate with external devices, such as sensors, memory, or other integrated circuits.
5.  Clock Distribution:
    
    -   FPGAs have dedicated resources for distributing clock signals throughout the device. These resources help in ensuring synchronous operation of the internal logic elements and facilitate the implementation of complex synchronous designs.
6.  Embedded RAM and DSP Blocks (Optional):
    
    -   Some FPGAs include embedded memory blocks and digital signal processing (DSP) blocks to provide additional functionality for specific applications that require memory or digital signal processing capabilities. 

##  Comparison between different Digital IC

![FPGAs, Deep Learning, Software Defined Networks and the Cloud: A Love Story  Part 1 | by Jamal Robinson | Medium](https://miro.medium.com/v2/resize:fit:1400/1*ROC6Psob0nLEMZ1igILpwA.png)
All the abovementioned digital IC can be compared in 2 parameters: Flexibility and Efficency.
As mentioned before, general processor (CPU and GPU)'s archieture is designed to running large variety of general instructions to cater to the most number of scernios as possible. Such design is less efficient.

## Multiprocessor System-on-Chip(MPSoC)
Multiprocessor System-on-Chip is a technology to integrate general processor(CPU, GPU), FPGA and DSPs into one system chip. With dedicated architechure and software ecosystem, engineers and developers can allow different cores to cooperate with each other. General processors can give those repetitive tasks to dedicated core, such as FPGA and GPU, such that they can accelerate the task. Thus, the whole system can work in faster speed and greater efficiency.
### Key Application of MPSoC:
 AI and Machine Learning, Automatives, Telecommunication
 
## Avnet Ultra96