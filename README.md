
![image](https://github.com/VardhanSuroshi/pes_asic_class/assets/132068498/33403244-c9dd-4aef-a022-da52e2eef51c)

# PES_ASIC_CLASS

Physical VLSI Design for ASIC

#Course objectives

The repository is a guide for ASIC flow from basics.

# Introduction
### Flow : HLL -> ALP -> Binary -> (HDL) -> GDS
#### 1. HLL -> High level language (c , c++) 
- A high-level language is any programming language that enables development of a program in a much more user-friendly programming context and is generally independent of the computer's hardware architecture.

#### 2. ALP -> Assembly level program
- An assembly language is a type of low-level programming language that is intended to communicate directly with a computer’s hardware (CPU). Assembly language programs are written using mnemonic codes that represent specific machine instructions which the machine can understand.Assembly language statements are entered one statement per line. Each statement follows the following format − [label] mnemonic [operands] [;comment]

#### 3. HDL -> Hardware Description Language (Verilog, System Verilog)
- A hardware description language (HDL) is a specialized computer language used to describe the structure and behavior of electronic circuits, and most commonly, digital logic circuits. HDLs can be used to design and describe the layout of digital systems from simple flip-flop memory units to complex communications protocols. It is used for circuit design, simulation,verification, synthesis and optimization of digital circuits.

#### 4. GDS -> Graphic Data System (layout)
- GDS II is a database file format which is the industry standard for data exchange of integrated circuit or IC layout artwork. It is a binary file format representing planar geometric shapes, text labels, and other information about the layout in hierarchical form. The data can be used to reconstruct all or part of the artwork used in sharing layouts, transferring artwork between different tools, or creating photo masks.

The Hardware needs to perform the instruction provided by the Application software. This is done through System sofware.

____System Software____
- OS : Operating System : Handles IO, memory allocation, Low level system function
- Compiler : Convert the input to hardware dependent instruction
- Assembler : Convert the instructions provided by compiler to Binary format
- HDL : A program that understands the Binary pattern and map it to a netlist
- GDS : Layout
