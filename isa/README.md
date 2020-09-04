# ISA

* Instructions Formats
* Instruction Types
* Design Decisions
  * Little vs Big Endian
  * Interal Storage in CPU: Stack vs Registers
  * Number of Operands and Instruction Length
* Addressing Modes
  * Immediate
  * Direct
  * Register
  * Indirect
  * Indexed
  * Based
  * Stack
  * Additional Modes
* Instruction Level Pipelining
  * Resource conflicts
  * Data dependencies
  * Conditional branch statements
* Superscalar design

## Instruction Formats

A machine instruction has an __opcode__ and __zero or more operands__.

Architectures are differentiated from one another by the number of bits allowed per instruction (16, 32, and 64 are the most common), by the number of operands allowed per instruction, and by the types of instructions and data each can process.

* Operand storage in CPU (stack structure or registers)
* Number of explicit operands per instruction
* Operand location
  * register-to-register
  * register-to-memory
  * memory-to-memory
* Operations
  * types of operations
  * which instructions can access memory
* Type and Size of Operands
  * addresses, numbers, or even characters

## Instruction Types

Most computer instructions operate on data, however, there are some that do not. Computer manufacturers regularly group instructions into the following categories:

* Data movement
* Arithmetic
* Boolean
* Bit manipulation (shift and rotate)
* I/O
* Transfer of control
* Special purpose

### Data Movement

Most frequently used instructions. Data is moved from memory into registers, from registers to registers, and from registers to memory. Many machines provide different instruction depending on the source and destination.

Some architectures, such as RISC, limit instructions that can move data to and from memory in an attempt to speed up execution.

### Arithmetic

Include those instructions that use integers and floating point numbers. Many instruction sets provide different arithmetic instructions for different various data sizes, and for various combinations of register and memory accesses in different addressing modes.

### I/O

I/O instructions vary greatly from architecture to architecture. The basic schemes for handling I/O are:

* Programmed I/O
* Interrupt-driven I/O
* DMA devices

### Transfer of control

Control instructions include branches, skips, and procedure calls.

Branching can be unconditional or conditional.

Skip instructions are basically branch instructions with implied addresses, because no operand is required.

Procedure calls are special branch instructions that automatically save the return address. Different machines use different methods to save this address. Some store the address at specific location in memory, others store it in a register, while still other push it on a stack.

### Special Purpose

Include those used for string processing, high level language support, protection, flag control, and cache management.

## Design Decisions

Instruction Set format must be determined before many other decisiones can be made.

ISas are measured by several different factors:

1. Amount of space a program requires.
2. Complexity of instruction set, in terms of amount of decoding necessary and complexity of tasks performed.
3. Length of instructions.
4. Total number of instructions.

### Things to consider

* Short instructions are typically better because they take up less space in memory and can be fetched quickly.

* Instructions of a fixed length are easier to decode but waste space.

* Memory organization affects instruction format.

* Fixed length instruction does not necessarily imply a fixed number of operands.

* There are many different types of addressing modes.

* If words consist of multiple bytes, in what order should these bytes be stored on a byte-addressable machine? Little versus big endian debate.

* How many registers should the architecture contain and how should these registers be organized? How should operands be stored in the CPU?

## Little vs Big Endian

The term _endian_ refers to a computer architecture's __byte order__, or the __way the computer stores the bytes of a multiple-byte data element__.

Virtually all computer architectures today are byte-addressable and must, therefore, have a standard for storing information requiring more than a single byte.

__Little endian machines store the least significant byte first__ (at the lower address). Therefore, a byte at a lower address has lower significance. For example, consider

__Big endian machines store the most significant bytes at the lower addresses__. 

Most UNIX machines are big endian, whereas most PCs (Intel) are little endian machines. Most newer RISC architectures are also big endian.o

### Example

Consider an integer requiring 4 bytes:

```
Byte 3 | Byte 2 | Byte 1 | Byte 0
```

On a little endian machine, this is arranged in memory as follows:

```
Base Address + 0 = Byte 0
Base Address + 1 = Byte 1
Base Address + 2 = Byte 2
Base Address + 3 = Byte 3
```

On a big endian machine, this long integer would then be stored as:

```
Base Address + 0 = Byte 3
Base Address + 1 = Byte 2
Base Address + 2 = Byte 1
Base Address + 3 = Byte 0
```

### Advantages

Big endian is more natural to most people and thus makes it easier to read hex dumps. Big endian machines store integers and strings in the same order and are faster in certain operations.

Most bitmapped graphics are mapped with a "most significant bit on the left" scheme, which means working with graphical elements larger than one byte can be handled by the architecture itself. This is a performance limitation for little endian computers because they must continually reverse the byte order when working with large graphical objects.

Computer networks are big endian, which means that when little endian computers are going to pass integers over the network, they need to convert them to network byte order. Likewise, when they receive integer values over the network.

However, conversion from a 32-bit integer address to a 16-bit integer address requires a big endian machine to perform addition. High-precision arithmetic on little endian machines is faster and easier.

## Interal Storage in CPU: Stacks vs Registers

How should CPU should store data? This is the most basic means to differentitate ISAs.

1. Stack architecture
2. Accumulator architecture
3. General Purpose Register (GPR) architecture
  * memory-memory
  * register-memory
  * register-register (load-store)

Designers choosing an ISA must decide which will work best in a particular environment and examine the tradeoffs carefully.

### Stack Architecture

Uses a stack to execute instructions, and operans are (implicitly) found on top of the stack.

Stack-based machines have a simple model for evaluation of expressions, but a stack cannot be accessed randomly, which makes it difficult to generate efficient code.

### Accumulator Architecture

One operand implicitly in the accumulator, minimize the internal complexity of the machine and allow for very short instructions.

But because the accumulator is only temporary storage, memory traffic is very high.

### GPR architecture

Uses sets of general purpose registers, are the most widely accepted models for machine architectures todat. These register sets are faster than memory easy for compilers to deal with, and can be used very effectively and efficiently.

In addition, hardware prices have decreased significantly, making it possible to add a large number of registers at a minimal cost.

If memory access is fast, a stack-based design may be a good idea, if memory is slow, it is often better to use registers.

However, because all operands must be named, using registers results in longer instructions, causing longer fetch and decode times.

GPR architecture can be broken down into three classifications, depending on where the operands are located.

#### Memory-Memory

May have two or three operands in memory, allowing an instruction to perform an operation without requiring any operand to be in a register.

#### Register-Memory

At least one operand is in a register and one in memory.

#### Load-Store

Require data to be moved into registers before any operations on that data are performed.

## Number of Operans and Instruction Length

The maximum number of operands, or addresses, contained in each instruction has a direct impact on the length of the instruction itself.

* _Fixed length_: Wastes space but is fast and results in better performance when instruction-level pipelining is used.
* _Variable length_: More complex to decode but saves storage space.

Typically, the real-life compromise involves using two to three instruction lengths, which provides bit patterns that are easily distinguishable and simple to decode.

If instruction length is exactly equal to the word length, the instructions align perfectly when stored in main memory. Instructions always need to be word aligned for addressing reasons. Therefore, instructions that are half, quarter, double, or triple the actual word size, can waste space. Variable length instructions are clearly not the same size and need to be word aligned, resulting in loss of space as well.

All architectures have a limit on the maximum number of operands allowed per instruction.

Using one-address instructions, we must assume a register (normally the _accumulator_) is implied as the destination for the result of the instruction.

### Stack-based architectures specifics

A stack-based architecture stores the operands on the top of the stack, making the top element accessible to the CPU. In architectures based on stacks, most instructions consist of opcodes only, however, there are special instructions (those that add elements to and remove elmenets from thhe stack) that have just one operand.

Stack architectures need a `push` and `pop` instructions, each of which is allowed one operand.

Only certain instructions are allowed to access memory, all others must use the stack for any operands required during execution.

For operations requiring two operands, the top two elements of the stack are used. For noncommutative operations such as subtraction, the top stack element is subtracted from the next-to-the-top element, both are popped, and the result is pushed onto the top of the stack.

This stack organization is very effective for evaluating long arithmetic expressions written in _reverse Polish Notation (RPN)__. This representation places the operator after the operands in what is known as _postfix notation_. Postfix notation combined with a stack of registers is the most efficient means to evaluate arithmetic expressions.

```
(X + Y) X (W - Z) + 2

// Written in RPN
WY+ WZ- X 2+
```

### Common Instruction Formats

* Opcode only
* Opcode + 1 Address (usually a memory address)
* Opcode + 2 Addresses (usually registers, or register and one memory address)
* Opcode + 3 Addresses (usually registers, or combinations of registers and memory)

## Expanding Opcodes

This represents a compromise between the need for a rich set of opcodes and the desire to have short opcodes, and thus short instructions.

The idea is to make some opcodes short, but have a means to provide longer ones when needed.

When the opcode is short, a lot of bits are left to hold operands. When you don't need any space for operands, all the bits can be used for the opcode, which allows for many unique instructions. In between, there are longer opcodes with fewer operands as well as shorter opcodes with more operands.

This expanding opcode scheme makes the decoding more complex. Instead of simply looking at a bit pattern and deciding which instruction it is, we need to decode the instruction first looking at the most significant bits first.

### Example

Suppose we wish to encode the following instructions:

* 15 instructions with 3 addresses
* 14 instructions with 2 addresses
* 31 instructions with 1 addresse
* 16 instructions with 0 addresses

We can do the following:

```
// 15 3-address codes
0000 R1 R2 R3
...
1110 R1 R2 R3

// 14 2-address codes
1111 0000 R1 R2
...
1111 1101 R1 R2

// 31 1-address codes
1111 1110 0000 R1
1111 1111 1110 R1

// 16 0-addresses code
1111 1111 1111 0000
1111 1111 1111 1111
```

And for decoding:

```
if (leftmost four bits != 1111) {
  execute three-addresses instruction
}
...
...
else {
  execute zero-address instruction
}
```

## Addressing

Bits in an instruction can be interpreted in many ways, providing us with several different _addressing modes_, that allow us to specify where the instruction operands are located.

An addressing mode can specify a constant, a register, or a location in memory.

Certain modes allow shorter addresses adnd some allow us to determine the location of the actual operand, often called the _effective address_ of the operand, dynamically.


### Immediate Addressing

Value to be referenced immediately follows the operation code in the instruction. Data to be operated on is part of the instruction.

The bits of the operand field do not specify an address, they specify the actual oeprand the instruction requires.

Immediate addressing is very fast because the value to be loaded is included in the instruction, however, this implies the value to be loaded is fixed at compile time and it is not very flexible.

### Direct Addressing

Value to be referenced is obtained by specifying the memory address directly in the instruction.

Typically quite fast because value is quickly accessible. It is also much more flexible than immediate addressing because value to be loaded may be variable.

### Register Addressing

A register, instead of memory, like in _Direct addressing_, is used to specify the operand.

### Indirect Addressing / Register Indirect Addressing

Bitys in the address field specify a memory address that is to be used as a pointer. The effective address of the operand is found by going to this memory address.

Very powerful addressing mode that provides an exeptional level of flexibility.

In a variation on this scheme, _Register Indirect Addressing_, the operand bits specify a register instead of a memory address.

### Indexed Addressing

Uses an index register (either explicitly or implicitly) is used to store an offset (or displacement), which is added to te operand, resulting in the effective address of the data.

### Based Addressing

A base address register, which holds a base address, where the address field represents a displacement from the base.

This mode and Indexed mode are quite useful for accessing array elements as well as characters in strings.

### Stack Addressing

The operand is assumed to be on the stack.

### Additional Addressing Modes

Many variations on the above schemes exist. The various addressing modes allow us to specify a much larger range of locations than if we were limited to using one or two modes.

As always, there are trade-offs. W sacrifice simplicity in address calculations and limited memory references for flexibility and increased address range.

## Instruction Level Pipelining

Conceptually, in fetch-decode-execution cycle, each pulse of the computer's clock is used to control one step in the sequence, but sometimes additional pulses can be used to control smaller details within one step.

Some CPUs break the f-d-e cycle down into smaller steps, where some of these smaller steps can be performed in parallel. This overlapping speeds up execution, and is known as __pipelining__.

We must also assume the architecture provides a means to fetch data and instructions in parallel. This can be done with separate instruction and data paths, however, most memory systems do not allow this. Instead, they provide the operand in cache, which, in most cases, allows the instruction and operand to be fetched simultaneously.

There is a fixed overhead involved in moving data from memory to registers. The amount of control logic for the pipeline also increases in size proportional to the number of stages, thus slowing down total execution. In addition, there are several conditions that result in "pipeline conflicts", which keep us from reaching the goal of executing one instruction per clock cycle:

* Resource conflicts
* Data dependencies
* Conditional branch statements

### Resource conflicts

Major concern in instruction-level parallelism. For example, if one instruction is storing a value to memory while another is being fetched fom memory, both need access to memory. Typically this is resolved by allowing the instruction executing to continue.

Certain conflicts can also be resolved by providing two separate pathways, one for data coming from memory and another for instructions coming from memory.

### Data dependencies

Arise when the result of one instruction, not yet available, is to be used as an operand to a following instructions.

There are several ways to handle these types of pipeline conflicts.

Special hardware can be added to detect instructions whose source operands are destinations for instructions further up the pipeline, which can insert a brief delay into the pipeline, allowing enough time to pass to resolve the conflict.

Special hardware can also be used to detect thse conflicts and route data through special paths that exist between various stages of the pipeline.

Some architectures address this problem by letting the compiler resolve the conflict by reordering instructions, resulting in a delay of loading any conflicting data but having no effect on the program logic or output.

### Conditional branch statements

Altering the flow of execution in a program, in terms of pipelining, causes major problems. If instructions are fetched one per clock cycle, several can be fetched and even decoded before a preceding instruction, indicating a branch, is executed. Conditional branching is particularly difficult to deal with.

Many architectures offer __branch prediction__, using logic to make the best guess as to which instructions will be needed next.

Compilers try to resolve branching issues by rearranging the machine code to cause a __delayed branch__. An attempt is made to reorder and insert useful instructions, but if that is not possible, no-op instructions are inserted to keep the pipeline full.

Another approach is given a conditional branch, to start fetches on both paths of the branch and save them until the branch is actually executed, at which time the "true" execution path will be known.

## Superscalar Design

One step beyond pipelining. Superscalar chips have multiple ALUs and issue more than one instruction in each clock cycle. The logic to keep track of hazards become even more complex, more logic is needed to schedule operations than to do them. But even with complex logic, it is hard to schedule parallel operations "on the fly".

The limits of dynamic scheduling have led machine designers to consider very different architecture, explicitly parallel instruction computers (EPIC).
