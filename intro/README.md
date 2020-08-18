# Introduction

## Computer Organization

Addresses issues such as control signals, signaling methods, and memory types. It ecompasses all __physical aspects__ of computer systems.

## Computer Architecture

Structure and behavior of the computer system, refers to the __logical aspects__ of system implementation. This includes many elements such as instruction sets and formats, operation codes, data types, number of registers, addressing modes, main memory access methods, and various I/O mechanisms.

Computer architecture for a given machine is the __combination of its hardware components plus its Instruction Set Architecture (ISA)__.

### ISA: Instruction Set Architecture

ISA is the agreed-upon interface between all the software that runs on the machine and the hardware that executes it.

### Principle of Equivalence of Hardware and Software

Anything that can be done with software can also be done with hardware, and viceversa.

## Historical Development

### Generation Zero: Mechanical Calculating Machines (1642-1945), Babbage

Early modern examples are attributed to Pascan and Leibniz in the 17th century.

Babbage built his [Difference engine](https://en.wikipedia.org/wiki/Difference_engine) in 1822. Babbage also designed a general-purpose machine in 1833 called the [Analytical Engine](https://en.wikipedia.org/wiki/Analytical_Engine), which was supposed to use a type of _punched card for input and programming_, idea originated by Joseph-Marie Jacquard.

The most important of the late-nineteenth-century tabulating machines was the one invented by Herman Hollerith (1860-1929). Hollerith's machine was used for encoding and compiling 1890 census data. This census was completed in record time.

### Generation One: Vacuum Tube Computers (1945-1953), Atanasoff (ABC), John Mauchly & J. Presper Eckert (ENIAC)

Altough Babbage is often called the "father of computing", his machines were mechanical, not eletrical or electronic.

Digital computers, as we know them today, are the outcome of work done by a number of people in the 1930s and 1940s.

Knorad Zuse picked up where Babbage left off, adding eletrical technology and other improvements to Babbage's design.

Three people clearly stand out as the inventors of modern computers:

* John Atanasoff
* John Mauchly
* J. Presper Eckert

Atanasoff has been credited with the construction of the __first completely electronic computer__, The Atanasoff Berry Computer (ABC) was a binary machine built from vacuum tubes, built specificallyto solve systems of linear equations.

John Mauchly and J. Presper Eckert were the two principle inventors of the ENIAC, introduced to the public in 1946. ENIAC is recognized as the __first all-electronic, general purpose digital computer__.

#### Vacuum Tube

John A. Fleming reasoned that _Thermionic emission_ supports the flow of electrons in only one direction, from the negatively charged cathode to the positevly charged anode. This behavior can _rectify_ alternatig current. In 1907, Lee DeForest added a _Control Grid_, that when carrying a negative charge, can reduce or prevent electron flow from the cathode to the anode of a diode, this latter defined as a _Triode_, which can act either as a _switch or as an amplifier_.

### Generation Two: Transistorized Computers (1954-1965)

In 1948, three researches from Bell Laboratories, John Bardeen, Walter Brattain, and William Schockley, invted the __Transistor__.

Because transistor consume less power than vacuum tubes, are smaller, and work more reliably, the circuitry in computers consequently became smaller and more reliable.

A plethora of computer makers meerged in this generation, including IBM.

### Generation Three: Integrated Circuit Computers (1965-1980)

Jack Kilby invented the __Integrated Circuit (IC) or microship__. Six months later, Robert Noyce create a similar device using silicon instead of germanium. This is the silicon chip upon which the computer industry was built.

Early ICs allowed dozens of transistors to exist on a single silicon chip that was smaller than a single 'discrete component' transistor.

Computers became faster, smaller, and cheaper, bringing huge gains in processing power.

The IC generation also saw the introduction of time sharing and multiprogramming (the ability for more than one person to use the computer at a time).

Multiprogramming, in turn, necessitated the introduction of new operating systems for these computers.

### Generation Four: VLSI Computers (198-????)

Increasing numbers of transistors were packed onto one chip. There are now various levels of integrations:

* SSI (small scale integration): 10 to 100 components per chip.
* MSI (medium scale integration): 100 to 1000 components per chip.
* LSI (large scale integration): 1000 to 10000 components per chip.
* VLSI (very large scale integration): more than 10000 components per chip.

VLSI technology, and its incredible shrinking circuits, spawned the development of microcomputers. These systems were small enough and inexpensive enough to make computers available and affordable to the general public.

## Moore's Law

In 1965, Intel founder Gordon Moore stated:

> "The density of transistors in an integrated circuit will double every year."

The current version of this prediction is usually conveyed as:

> "The density of silicon chips doubles every 18 months."

## Rock's Law

Arthur Rock, early Intel capitalist, proposed a corollary:

> "The cost of capital equipment to build semiconductors will double every four years".

## Computer Level Hierarchy

![hierarchy](https://images.slideplayer.com/25/7820572/slides/slide_26.jpg)

We can imagine the machine to be built from a hierarchy of levels, in which each level has a specific function and exists as a distinct hypothetical machine, called a _virtual machine_, which executes its own particular set of instructions, calling upon machines at lower levels to carry out the tasks when necessary.

