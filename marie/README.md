# MARIE, a simple computer

* CPU
  * Data Path
  * Control Unith
* Registers
* ALU
* Bus
  * Sync/Async
  * Bus Abitration
    * Daisy Chain
    * Centralized Parallel
    * Distributed using Self-Selection
    * Distributed using Collision Detection
* Clocks
* I/O
  * Memory-Mapped
  * Instruction-Based
* Memory
  * Organization
  * Addressing
    * Byte-Addressable Architecture
    * Word-Addressable Architecture
* Interrupts
* ISA: Instruction Set Architecture
* Instruction Processing
  * Fetch-Decode-Execute Cycle
  * Interrupts and I/O

## CPU Basics and Organization

Central Processing Unit (CPU) is responsible for fetching program instructions, decoding each instruction that is fetched, and performing the indicated sequence of operaitons on the correct data.

This unit can be divided into two pieces.

### Data Path

Network of storage units (registers) and arithmetic and logic units, connected by buses where the timing is controlled by clocks.

### Control Unit

Module responsible for sequencing operations and making sure the correct data is where it needs to be at the correct time.

The Control Unit is the "_traffic manager_" of the CPU. It extracts instructions from memory, decodes these instructions, and makes sure data is in the right place at the right time, tells the ALU which registers to use, services interrupts, and turns on the correct circuitry in the ALU for the execution of the desired operation.

It uses a _Program Counter Register_ to find the next instruction for execution and a _Status Register_ to keep track of things like overflows, carries, borrows, and the like.

## Registers

Hardware device that stores binary data. Registers are located on the processor so information can be accessed very quickly.

Registers are not addressed in the same way memory is addressed. Registers are addressed and manipulated by the control unit itself.

## ALU

The _Arithmetic Logic Unit_ carries out the logic operations (such as comparisons) and arithmetic operations (such as add or multiply) required during program execution.

Generally an ALU has two data inputs and one data output.

Operations performed in the ALU often affect bits in the _Status Register_, to indicate actions such as whether an overflow has occurred.

## Bus

CPU communicates with the other components via a bus, which is a set of wires that acts as a shared but common data path to connect multiple subsystems within the system.

it consists of multiple lines, allowing the parallel movement of bits.

At any one time, only one device may use the bus. However, this sharing often results in _communications bottleneck_. The speed of the bus is affected by its length as well as by the number of devices sharing it.

A bus can be __point-to-point__, connecting two specific components, or it can be a __common pathway or multipoint__ that connects a number of devices, requiring these devices to share the bus.

Due to the different types of information buses transport and the various devices that use the buses, buses themselves have been divided into different types:

* __Processor-memory__ buses, are short, high-speed buses that are closely matched to the memory system on the machine to maximize the bandwidth and are usually very design specific.
* __I/O__ buses, are typically longer than processor-memory and allow for many types of devices with varying bandwiths. They are compatible with many different architectures.
* __Backplane__ bus, is actaully built into the chassis of the machine and connects the processor, I/O devices, and memory, so all devices share one bus.

Many computers have a hierarchy of buses, so it is not uncommon to have two buses or more in the samy systems.

Personal computers refer to _internal or system bus_ to the bus that connects CPU, memory and all other internal components. _External or expansion buses_ connect external devices, peripherals, expansion slots, and I/O ports to the rest of the computer. Most PCs also have _local buses_, data buses that connect a peripheral device directly to the CPU, whch are very high-speed buses and can be used to connect only a limited number of similar devices.

Buses are physically little more than bunches of wires, but they have specific standard for connectors, timing, signaling specifications and exact protocols for usage.

### Synchronous Buses

Clocked, things happen only at the clock ticks. Every device is synchronized by the rate at which the clock ticks, or the _Clock Rate_. For example, if the bus clock rate is 133Mhz, then the length of the bus cycle is 1/133.000.000 or 7.52ns.

Because the clock controls the transactions, any _clock skew_ (drift in the clock) has the potential to cause problems, implying that the bus must be kept as short as possible so the clock drift cannot get overly large. In addition, the bus cycle time must not be shorter than the length of time it takes information to traverse the bus.

The length of the bus, therefore, imposes restrictions on both the bus clock rate and the bus cycle time.

### Asynchronous buses

Control lines coordinate the operations and a complex _Handshaking protocol_ must be used to enforce timing.

### Bus Arbitration

To use a bus, a device must reserve it, because only one device can use the bus at a time.

Bus masters are devices that are allowed to initiate transfer of information (control bus) whereas bus slaves are modules that are activated by a master and respond to requests to read and write data (so only masters can reserve the bus).

In systems with more than one master device, _bus arbitration_ is required. Bus arbitration schemes must provide priority to certain master devices while, at the same time, making sure lower priority devices are not starved out.

#### 1. Daisy Chain Arbitration

Uses a "_grant bus_" control line that is passed down the bus from the highest priority device to the lowest priority device.

#### 2. Centralized Parallel Arbitration

Each device has a request control line to the bus, and a centralized arbiter selects who gets the bus. Bottlenecks can result using this type of arbitration.

#### 3. Distributed Arbitration Using Self-Selection

Instead of a central authority selecting who gets the bus, the devices themselves determine who has highest priority and who should get the bus.

#### 4. Distributed Arbitration Using Collision Detection

Each device is allowed to make a request for the bus, if the bus detects any collisions (multiple simultaneous requests), the device must make another request.

## Clocks

Every computer contains an internal clock that regulates how quickly instructions can be executed. The clock also synchronizes all of the components in the system. The CPU uses this clock to regulate its progress, checking the otherwise unpredictable speed of the digital logic gates.

The CPU requires a fixed number of clock ticks to execute each instruction. Therefore, instruction performance is often measured in __Clock Cycles__, the time between clock ticks, instead of seconds. The __Clock Frequency__ is measured in __MHz__, where 1MHz is equal to 1 million cycles per second, so 1Hz is 1 cycle per second. The Clock Cycle time (or Clock Period) is simple the reciprocal of the clock frequency. For example, an 800Mhz machine has a clock cycle time of 1/800.000.000 of 1.25ns. If a machine has a 2ns cycle time, then it is a 500Mhz machine.

__Most machines are synchronous__, there is a master clock signal, which ticks at regular intervals. Registers must wait for the clock to tick before new data can be loaded.

### Minimum Clock Cycle

It seems reasonable to assume that if we speed up the clock, the machine will run faster. However __there are limits on how short we can make the clock cycles__. When the clock ticks and new data is loaded into the registers, the register outputs are likely to change. These changed output values must propage through all the circuits in the machine until they react the input of the next set of registers. The clock cycle must be long enough to allow these changes to reach the next set of registers. __If the clock cycle is too short__, we could end up with some values not reaching the registers, resulting in an inconsistent state in our machine. Therefore, the __minimum clock cycle time must be at least as great as the maximum propagation delay of the circuit__, from each set of register outputs to register inputs.

### Architecture

The architecture of a maxchine has a large effect on its performance. Two machines with the same clock speed do not necessarily execute instructions in the same number of cycles.

```
CPU Time = seconds / program = instructions/program * average cycles/instructions * seconds/cycle
```

### System & Bus Clock

Generally, the term clock refers to the _System Clock_ or master clock that regulates the CPU and other components. However, certain buses also have their own clocks. _Bus Clocks_ are usually slower than CPU clocks, causing bottleneck problems.

When we connect all of the components together in a serial fashion, where one component must complete its task before another can function properly, it is important to be aware of these performance bounds so we are able to synchronize the components properly.

## I/O Subsystem

I/O devices allow us to communicate with the computer system. I/O is the transfer of data between primary memory and various I/O peripherals.

These devices are not connected directly to the CPU. Instead, there is an _interface_ that handles the data transfers. This interface converts the system bus singlas to an from a format that is acceptable to the given device. The CPU communicates to these external devices via input/output registers.

The exchange of data is performed in two ways:

### Memory-Mapped I/O

Registers in the interface appear in the computer's memory map and there is no real difference between accessing memory and accessing an I/O device.

Clearly, this is advantageous from the perspective of speed, but it uses up memoy space in the system.

### Instruction-Based I/O

CPU has specialized instructions that perform the input and output. Although this does not use memory space, it requires specific I/O instructions, which implies it can be used only be CPUs that can execute this specific instructions.

Interrupts play a very important part in I/O, because they are an efficient way to notify the CPU that input or output is available for use.

## Memory Organization and Addressing

You can envision memory as a matrix of bits. Each row, implemented by a register, has a length typically equivalent to the word size of the machine. Each register (more commanly referred to as a _memory location_) has an unique address. Memory addresses usually start at zero and progress upward.

A memory address is typically stored in a single machine word.

Memory is built from _Random Access Memory (RAM) chips_.

Memory is often referred to using the __L x W notation__ (length x width). For example, 4M x 16 means the memory is 4M long (2^2 X 2^20 = 2^22 words) and it is 16 bits wide (word size). To address this memory (assuming word addressing), we need to be able to uniquely identify 2^12 different items (different addresses). So we need to count from 0 to 2^12 - 1 in binary. In general, if a computer has 2^n addressable units of memory, it will require N bits to uniquely address each type.  

### Words

Each building (word) has multiple apartments (bytes), and each apartment has its own address. All of the apartments are numbered sequentially (addressed), from 0 to the total number of apartments in the complex. The buildings themselves serve to group the apartments.

__Words are the basic unit of size used in various instructions__. For example, you may read a word from or write a word to memory, even on a byte-addressable machine.

### Byte Adressable Architecture

An __address is almost always represented by an unsigned integer__. Normally, memory is __byte-addressable__, which means that __each individual byte has an unique address__.

Some machines may have a word size that is larger than a single byte. For example, a computer might handle 32-bit word, but still employ a byte-addressable architecture. In this situation, when a word uses multiple bytes, the byte with the lowest address determines the address of the entire word.

#### Issue of alignment

In an architecture is _byte-addressable_, and the instruction set architecture word is larger than 1 byte, the issue of _alignment_ must be addressable.

For example, if we wish to read a 32-bit word on a byte-addressable machine, we must make sure that:

1. The word was stored on a natural alignment boundary.
2. Access starts on that boundary.

This is accomplished, in the case of 32-bit words, by requiring the address to be a multiple of 4.

Some architectures allow unaligned accesses, where the desired address does not have to start on a natural boundary.

### Word Addressable Architecture

It is also possible that a computer might be __word-addressable__, which means each word has its own address, but most current machines are byte-addressable.

### Memory Interleaving

Memory interleaving splits memory across multiple memory modules (or banks). With high-order interleaving, the more intuitive organization, the high-order bits of the address are used to select the bank, and distributes the addresses so that each module contains consecutive addresses.

## Interrupts

Interrupts are vents that alter the normal flow of execution in the system, that can be triggered for a variety of reasons:

* I/O requests.
* Arithmetic errors, underflow or overflow.
* Hardware malfunction.
* User-defined break points.
* Page faults.
* Invalid instructions.
* Miscellaneous.

The actions performed for each of these types of interrupts (__interrupt handling__) are very different.

An interrupt can:

* Be initiated by the user or the system.
* Be __maskable__ (disabled or ignored).
* Be __nonmaskable__ (a high priority interrupt that must be acknowledged).
* Occur within or between instructions.
* Be synchronous (occurs at the same place every time a program is executed).
* Be asynchronous (occurs unexpectedly).
* Result in program terminating or continuing execution once the interrupt is handled.

## ISA: Instruction Set Architecture

ISA specifies the instructions that the computer can perform and the format for each instruction. It is essentially an interface between the software and the hardware.

Most ISAs consist of instructions for processing data, moving data, and controlling the execution sequence of the program.

### MARIE specifics

`Load` instruction allows us to move data from memory into the CPU (via the MBR and the AC). All data from memory must move first into the MBR and then into either the AC or the ALU, there are no  other options in this architecture.

Notice that the `Load` instruction does not have to name thew AC as the final destination, this register is implicit in the instruction.

### Assembly Language Instructions

Instructions consist of an __4 bit opcode + 12 bit address__ combination. Most people would rather use the instdruction name, or __mnemonic__, for the instruction, instead of the binary value.

Binary instructions are called __machine instructions__, whereas the corresponding mnemonic instructions are what we refer to as __assembly language instructions__.

There is a one-to-one correspondence between assembly language nad machine instructions.

When we write in an assembly language program, we need an assembler to convert it to its binary equivalent.

### Assemblers

An assembler's job is to convert assembly language (using mnemonics) into machine language (binary instructions). The assembler reads a _source file_ (assembly program) and produces an _object file_ (machine code).

Substituting simple alphanumeric names for the opcodes makes programming much easier. We can also substitute _labels_ (simple names) to identify or name particular memory addresses, making the task of writing assembly programs ever simpler. When the address field of an instruction is a label instead of an actual physical address, the assembler still must translate it into a real, physical address in main memory.

#### Labels & Symbol Table

Labels are nice for programmers. However, they make more work for the the assmebler. It must make two phases through a program to do the translation, reading the program twice, from top to bottom each time, because it does not know where the data portion of the instruction is located if it is given only a label.

On the first pass, the assembler builds a set of correspondences called a _symbol table_. It also begins to translate the instructions.

On the second pass, the assembler uses the symbol table to fill in the addresses and create the corresponding machine language instructions.

#### Assembler Directive

Instruction specifically fort the assembler that is not supposed to be translated into machine code, for example, to specifiy which base is to be used to interpret the value (binary, hex, decimal).

#### Comment Delimiter

Special characters that tell the assembler (or compiler) to ignore all text following the special character.

#### Why Assembling Language?

Compiler takes high-level language (such as C++) and converts it into assembly language. Compilers, in most cases, do a great job. Ocasionally, however, programmers must bypass some of the restrictions found in high-level languages and manipulate the assembly code themselves. By doing this, programmers can make the program more efficient in terms of time (and space).

Some situations favor programs written entirely in assemblyl anguage, for example, if the overall size of the program or response time is critical. This is because compilers tend to obscure information about the cost of various operations and programmers often find it difficult to judge exactly how their compiled programs will perform.

Assembly language puts the programmer closer to the architecture, and thus, in firmer control.

A perfect example, in terms of both response performance and space-critical design, is found in _embedded systems_.

## Instruction Processing

All computers follow a basic machine cycle:

1. Fetch
2. Decode
3. Execute

### Fetch-Decode-Execution Cycle

Represents the steps that a computer follows to run a program.

Note that computers today, even with large instructions sets, long instructions, and huge memories, can execute millions of these fetch-decode-execute cycles in the blink of an eye.

#### Overview

1. CPU fetches an instruction, transfering it from main memory to the instruction register.
2. CPU decodes instruction, determining the opcode and fetching any data necessary to carry out the instruction.
3. CPU executes instruction, perfoming the operation(s) indicated by the instruction.

Notice that a large part of this cycle is spent copying data from one location to another.

#### Details

When a program is initially loaded, the adress of the first instruction must be placed in the PC. The steps in this cycle, which take place in specific clock cycles, are listed below:

1. Copy contents of the PC to the MAR: `MAR <- PC`.
2. Go to main memory and fetch instruction found at the address in MAR, placing this instruction in the IR: `IR <- M[MAR]`.
3. Increment PC by 1, which now points to the next instruction in the program: `PC <- PC + 1`.
  * This is because MARIE is word-addressable. If MARIE were byte-addressable, the PC would need to be incremented by 2 to point to the address of the next instruction, because each instruction would require two bytes. On a byte-addressable machine with 32-bit words, the PC would need to be incremented by 4.
4. Copy rightmost 12 bits of the IR into the MAR: `MAR <- IR[11-0]`.
5. Decode leftmost four bits to determine the opcode: `IR[15-12]`.
6. If necessary, use the address in the MAR to go to memory to get data, placing the data in the MBR (and possibly the AC), and then execute the instruction: `MBR <- M[MAR]` and exceute the instruction.

### Interrupts and I/O

Input registers hold data being transferred from an input device into the computer. The output registers hold information ready to be sent to an output device. The timing used by these two registers is very important.

#### Interrupt-Driven I/O

MARIE addresses timing problems by using __Interrupt-Driven I/O__. When the CPU executes an input or output instruction, tthe appropriate I/O device is notified. The CPU then continues with other useful work until the device is ready. At that time, the device sends an interrupt signal to the CPU. The CPU then processes the interrupt, after which it continues with the normal fetch-decode-execute cycle.

1. Signal (interrupt) from the I/O device to the CPU indicating that input or output is complete.
2. Some means of allowing the CPU to detour from the usual fetch-decode-execute cycle to _recognize_ this interrupt.

The method most computers use to process an interrupt is to __check to see if an interrupt is pending at the beginning of each f-d-e cycle__. If so, the interrupt is processed, executing an interrupt routine that is determined by the type of interrupt that has occurred, after which the machine execution cycle continues. If no interrupt is present, processing continues as normal.

Typically, input/output __device sends an interrupt by using a special register, the status or flag register__. A special bit is set to indicate an interrupt has occured.

After the CPU recognizes an interrupt request, the address of the interrupt service routine is determined, and the routine is executed. The CPU must return to the exact point at which it was running in the original program. Therefore, when the CPU switches to the interrupt service routine, it must save the contents of the PC, the contents of all other registers in the CPU, and any status conditions that exist for the original program, restoring this exact same environment after the interrupt service routine has finished.

For example, as soon as input is entered from the keyboard, this bit is set. CPU checks this bit at the beginning of every machine cycle. When it is set, the CPU processes an interrupt. When it is not set, CPU performs a normal f-d-e cycle, processing instructions in the program it is currently executing.

Other external interrupts, generated by external events (such as power failure), internal interrupts generated by some exception condition in the program (such as division by zero, stack overflow, or protection violations), and software interrupts, regarless of which type of interrupt has been invoked, the interrupt handling process is the same.

## Hardwired vs Microprogramed Control

In reality, there must be control signals to assert lines on various digital components to make things happen as described.

You can take one of two approaches to ensure control lines are set properly.

### Hardwired

The first approach is to physically connect all of the control lines to the actual machine instructions.

Instructions are divided up into fields, and different bits in the instruction are combined through various digital logic components to drive the control lines.

The control unit is implemented using hardware (with simple MAND gates, flip-flops, and counters, for example).  We need a special digital circuit that uses, as inputs, the bits from the opcode  field in our instructions, bits from the flag/status register, signals from the bus, and signals from he clock. It should produce, as outputs, the control signals to drive the various components in the computer.

For example, a 4-to-16 decoder could be used to decode the opcode. By using the contents of the IR register and the status of the ALU, this unit controls the registers, the ALU operations, all shifters, and bus access.

The advantage of hardwired control is that it is very fast.

The disadvantage is that the instruction set and the control logic are directly tied together by special circuits that are complex and difficult to design or modify. If someone designs a hardwired computer and later decides to extend the instruction set, the physical components in the computer must be changed. This is prohibitively expensive, because not only must new chips be fabricated but also the old ones must be located and replaced.

### Microprogramming

Uses software for control. All machine instructions are input into a special program, the _microprogram_ to convert the instruction into the appropriate control signals.

The microprogram is essentially an interpreter, written in _microcode_, that is stored in _firmware_ (ROM, PROM or EPROM), which is often referred to as the _control store_. This program converts machine instructions of zeros and ones intro control signals.

Essentially there is one subroutine in this program for each machine instruction.

The advantage of this approach is that if the instruction set requires modification, the microprogram is simply updated to match, no change is required in the actual hardware.

Microprogramming is flexible, simple in design, and lends itself to very powerful instruction sets. Microprogramming allows for convenient hardware/software tradeoffs, if you want something not implemented in hardware, it can be implemented in the microcode.

The disadvantage to this approach is that all instructions must go through an additional level of interpretation, slowing down the program execution. IN addition to this cost in time, there is a cost associated with the actual development, because appropriate tools are required.

## Real World Architecture Examples

### CISC: Complex Instruction Set Computer

Each member of the x86 family of Intel architectures is known as a CISC machines.

CISC machines have a large number of instructions, of variable length, with complex layouts. Many of these instructions are quite complicated, performing multiple operations when a single instruction is executed. For example, it is possible to do loops using a _single_ assembly language instruction.

The basic problem with CISC machines is that a small subset of complex CISC instructions slows the systems down considerably.

### RISC: Reduced Instruction Setr Computer

Each member of the Pentium family and MIPS architectures are examples of RISC machines.

Designers decided to return to a less complicated architecture and to hardwire a small (but complete) instruction set that would execute extremely quickly. This meant it would be the compiler's responsibility to produce efficient code for the ISA. Machines utiolizing this philosophy are called RISC machines.

The main objective of RISC is to simplify instructions so they can execute more quickly. Each instruction performs only one operation, they are all the same size, they have only a few different layouts, and all arithmetic operations must be performed between registers (data in memory cannot be used as operands).

Virtually all new instructions sets (for any architectures) since 1982 have been RISC, or some sort of combination of CISC and RISC.