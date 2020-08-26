# MARIE, a simple computer

* CPU
  * Data Path
  * Control Unith
* Registers
* ALU
* Bus
* Clocks

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