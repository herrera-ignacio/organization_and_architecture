# ISA

* Instructions Format
* Design Decisions
  * Little vs Big Endian
  * Interal Storage in CPU: Stack vs Registers
  * Number of Operands and Instruction Length

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