# Von Neumann Model

## Stored-Program Machine Architecture

Hungarian Mathematician John Von Neumann, after reading Mauchly and Eckert's proposal for the EDVAC, which included an easier way to change the behavior of the machine using memory devices to provide a way to store program instructions, published and puiblicized the idea.

All stored-program computers have come to be known as Von Neumann systems using the Von Neumann architecture.

![von neumann architecture](https://media.geeksforgeeks.org/wp-content/uploads/basic_structure.png)

![von neumann architecture](https://i.stack.imgur.com/GNw7I.jpg)

### Characteristics

Today's version of the stored-program machine architecture satisfies at least the following:

1. Consists of three hardware systems:
  * CPU: Central Processing Unit
    * Control unit: handles all processor control signals, directs input and output flow, and fetches code for instructions.
    * ALU: Arithmetic logic unit, handles calculation CPU may need, logical operations, bit shifting, etc.
    * Registers (small storage areas)
      * Accumulator: stores calculation made by ALU.
      * Program Counter: keep track of the memory location of the next instructions that need to be fetched from memory or stored into memory.
      * MAR: Memory Address Register
      * MDR: Memory Data Register
      * C|IR: Current| Instruction Register: holds information about instruction currently being executed or decoded.
      * Others
  * Main-Memory System, which holds programs that control the computer's operation.

  * I/O System
2. Capacity to carry out sequential instruction processing.
3. Contains a single path (_Von Neumann Bottleneck_), either pysically or logically, between the main memory system and the control unit of the CPU, forcing alternation of instruction and execution cycles. Instructions can only be done one at a time and can only be carried out sequentially.

### Von Neumann Execution Cycle

1. Control Unit fetches next program instruction from memory, using program counter to determine where the instruction is located.
2. Instruction is decoded into a languae that ALU can understand.
3. Data operands required to execute the instruction are fetched from memory and placed into registers within the CPU.
4. ALU executes the instruction and places results in registers or memory.

### Communication Between Memory and CPU

#### Read

1. Address of the location is put in MAR.
2. Memory is enabled for read.
3. Value is put in MDR by the memory.

#### Write

1. Adress of location is put in MAR.
2. Data is put in MDR.
3. Memory is enabled for write.
4. Value in MDR is written to the location specified.

### System Bus Model

![system bus](https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Computer_system_bus.svg/1200px-Computer_system_bus.svg.png)

Ideas present in the Von Neumann Architecture, have been extended so that programs and data stored in a slow-toaccess torage medium, such as a hard disk, can be copied to a fast-access, volatile storage medium such as RAM prior to execution.

This architecture has also been streamlined into what is currently called the __System Bus Model__, which moves data from main memory to the CPU registers (and vice versa). The _Address bus_ holds the address of the data that the _Data bus_ is currently accessing, and the _Control bus_ carries the necessary control signal.

### Other enhancements

* Index Registers for addressing
* Floating point data
* Interrupts and Asynchronous I/O
* Virtual memory
* General Registers

## Non-Von Neumann Models

The Von Neumann Bottleneck continues to baffle engineers looking for ways to build fast systems that are inexpensible and compatible with the vast body of commercially available software. Engineers who are not constrained by the need to maintain compatibility are free to use many different models of computing:

### Neural Networks

Using ideas from models of the brain as a computing paradigm.

### Genetic Algorithms

Exploiting ideas from biology and DNA evolution.

### Quantum Computation & Parallel Computers

The underlying premise is that every algorithm has a sequential part that ultimately limits the speedup that can be achieved by multiprocessor implementation.
