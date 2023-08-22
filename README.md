
![image](https://github.com/VardhanSuroshi/pes_asic_class/assets/132068498/33403244-c9dd-4aef-a022-da52e2eef51c)

# PES_ASIC_CLASS

Physical VLSI Design for ASIC.

The repository is a guide for ASIC flow from basics.
# Day 1
### Introduction to RISCV ISA and GNU compiler toolchain
### Introduction
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

 ### Lab 1


#### Contents:
- C Program to compute sum from 1 to N
- RISCV gcc compile and disassemble
- Spike simulation and debug


 1) Create a directory and open file sum1ton.c
  Write a C code to find the sum of numbers from 1 to n

 ![sum1ton_prog](https://github.com/ananya343B/pes_asic_class/assets/142582353/3b8ca152-4667-4a31-bee1-068b3954e91d)
 
  Now we will compile and execute the program. The output of the code is as follows
  
 ![sum1ton_op](https://github.com/ananya343B/pes_asic_class/assets/142582353/fe2cc5fe-b6d4-40c3-ad7b-aaa80509d229)

3) Generating RISCV object file and comparing the outputs
  
![pic3](https://github.com/ananya343B/pes_asic_class/assets/142582353/6e882222-8de4-45f0-90eb-2df2ecfaf5b1)

The command ```riscv64-unknown-elf-gcc``` is used to generate the object file sum1ton.o

![pic5](https://github.com/ananya343B/pes_asic_class/assets/142582353/27cd9359-ced5-4950-8575-c333f2863f40)

4) Spike simulation and debug

   ``` spike pk sum1ton.o```   is used to check if the instructions produce the correct output


  ![spike1](https://github.com/ananya343B/pes_asic_class/assets/142582353/afd04a0c-01dd-4f27-9b85-be623111a83e)

  ``` spike -d pk sum1ton.c```   is used for debugging
  
  The contents of the registers can be viewed
  
![spike](https://github.com/ananya343B/pes_asic_class/assets/142582353/dd1400e8-65d9-45ae-82e5-a648eab9ff91)

``` reg 0 a2```   is used to check the content of register a2

```q```  is used to quit the debugging process


### Lab 2
#### Contents: 

To display max and min value of 64 bit signed and unsigned numbers.

##### Unsigned numbers:

They are non-negative numbers which only have magnitude and no sign or direction.
Range:[0,(2^n)-1]

##### Signed numbers:

Signed numbers are numerical values which can represent positive and negative numbers along with zero.
Range: Positive:[0,2^(n-1)-1]      Negative:[-1,2^(n-1)].

- C Program to find max and min of 64 bit unsigned number:

  ![unsigned_code](https://github.com/ananya343B/pes_asic_class/assets/142582353/85744544-05cc-414c-8b13-588eb929631a)

  Output:
  
  ![unsigned_op](https://github.com/ananya343B/pes_asic_class/assets/142582353/b23b9188-9baa-4cd0-a98d-566ab642dd98)

- C Program to find max and min 64 bit signed number:

  ![signed_code](https://github.com/ananya343B/pes_asic_class/assets/142582353/6ee35461-849f-45fb-aeac-6f5c86a78ab9)

  Output:

  ![signed_op](https://github.com/ananya343B/pes_asic_class/assets/142582353/5cadeae9-df14-4c5d-845d-5af1826195ff)


  # Day 2
  ### Introduction to ABI and basic verification flow

  ### Types of Instruction based on encoding format

1. **R-Type (Register-Type):**
   - These instructions operate on registers and have a fixed format for their operands.
   - Examples: ADD, SUB, AND, OR, XOR, SLL, SRL, SRA, SLT, SLTU

2. **I-Type (Immediate-Type):**
   - These instructions have an immediate operand and one register operand.
   - Examples: ADDI, SLTI, SLTIU, XORI, ORI, ANDI, SLLI, SRLI, SRAI, LB, LH, LW, LBU, LHU, JALR

3. **S-Type (Store-Type):**
   - These instructions are used for storing values from registers to memory.
   - Examples: SB, SH, SW

4. **B-Type (Branch-Type):**
   - These instructions perform conditional branching based on comparisons.
   - Examples: BEQ, BNE, BLT, BGE, BLTU, BGEU

5. **U-Type (Upper Immediate-Type):**
   - These instructions have a larger immediate field for encoding larger constants.
   - Examples: LUI, AUIPC

6. **J-Type (Jump-Type):**
   - These instructions are used for unconditional jumps and function calls.
   - Examples: JAL
  
     
  

