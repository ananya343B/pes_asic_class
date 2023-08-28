
![image](https://github.com/VardhanSuroshi/pes_asic_class/assets/132068498/33403244-c9dd-4aef-a022-da52e2eef51c)

# PES_ASIC_CLASS

Physical VLSI Design for ASIC.

The repository is a guide for ASIC flow from basics.

#### Introduction and ABI
<details>
<summary> 
 Day 1
</summary>
<br>
 
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
</details>

 <details>
<summary> 
 Day 2
</summary>
<br>
  
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
  
     
  ### Application Binary Interface

An Application Binary Interface (ABI) is a set of conventions or rules that govern how functions, data structures, and system calls should be organized and accessed in a binary program or library. It defines the low-level interface between different parts of a program or between a program and the operating system. Here are the key points about an ABI:

1. **Binary Compatibility**: ABIs ensure that binary code produced by one compiler or platform can work seamlessly with code produced by another, as long as they adhere to the same ABI.

2. **Function Calling Convention**: ABIs specify how functions are called, including the order and location of arguments and return values, as well as how the call stack is managed during function calls.

3. **Register Usage**: ABIs define which registers are reserved for certain purposes (e.g., argument passing, return values, temporary storage) and how they should be managed during function calls.

4. **Data Layout**: ABIs specify how data structures like structs and arrays are laid out in memory, including rules for alignment and padding.

5. **Exception Handling**: They define how exceptions (such as hardware or software interrupts) are handled, including how control is transferred between user code and exception handlers.

6. **System Calls**: ABIs detail how programs interact with the operating system through system calls, including how arguments are passed and results are retrieved.

7. **Platform Independence**: ABIs help maintain compatibility across different platforms (e.g., different CPU architectures or operating systems) by providing a standardized interface.

8. **Dynamic Linking**: They cover aspects of dynamic linking, such as how shared libraries (DLLs on Windows or shared objects on Unix-based systems) are loaded and linked at runtime.

9. **Versioning**: Some ABIs include mechanisms for versioning so that future changes can be made without breaking compatibility with existing code.

10. **Documentation**: ABIs are typically documented and published, allowing developers to write code that conforms to the ABI's specifications.

11. **Toolchain Support**: Compilers and assemblers are designed to generate code that follows the ABI, ensuring that code produced by different tools can interoperate.

12. **Cross-Platform Development**: ABIs are especially important for cross-platform development, where code needs to run on multiple platforms with potentially different hardware architectures and operating systems.

13. **Security**: ABIs may include security-related aspects, such as buffer overflow protection mechanisms and stack canaries.


### Memmory Allocation for Double Words
64-bit number (or any multi-byte value) can be loaded into memory in little-endian or big-endian. It involves understanding the byte order and arranging the bytes accordingly
1. **Little-Endian:**
In little-endian representation, you store the least significant byte (LSB) at the lowest memory address and the most significant byte (MSB) at the highest memory address.
2. **Big-Endian:**
In big-endian representation, you store the most significant byte (MSB) at the lowest memory address and the least significant byte (LSB) at the highest memory address.

![th1](https://github.com/ananya343B/pes_asic_class/assets/142582353/e6415a66-5c06-40fc-b30e-a58a093ff9f1)


### Load, Add and Store Instructions
Load, Add, and Store instructions are fundamental operations in computer architecture and assembly programming. They are often used to manipulate data within a computer's memory and registers.

Example `ld x8, 16(x23)`

![th2](https://github.com/ananya343B/pes_asic_class/assets/142582353/ee3d8ef6-a411-4313-bee9-4cc2fdd8dad9)

In this Example
- `ld` is the load double-word instruction.
- `x8` is the destination register.
- `16(x23)` is the memory address pointed to by register `x5` (base address + offset).

 
Example `add x8, x24, x8`

![th3](https://github.com/ananya343B/pes_asic_class/assets/142582353/468facf3-3a36-4da0-a0e9-75c3c5b07044)


In this Example
- `add` is the add instruction.
- `x8` is the destination register.
- `x24` and `x8` are the source registers.

  ### 32-Registers and their ABI Names
The choice of the number of registers in a processor's architecture, such as the RISC-V RV64 architecture with its 32 general-purpose registers, involves a trade-off between various factors. While modern processors can have more registers but increasing the number of registers could lead to larger instructions, which would take up more memory and potentially slow down instruction fetch and decode.

###### ABI Names
ABI names for registers serve as a standardized way to designate the purpose and usage of specific registers within a software ecosystem. These names play a critical role in maintaining compatibility, optimizing code generation, and facilitating communication between different software components. 

![th4](https://github.com/ananya343B/pes_asic_class/assets/142582353/9772d6fe-b73c-4b29-b17f-2fc39a7db8d3)


### Lab
 We will use ABI to write a C program in ASM and check the result.
 
 ![th5](https://github.com/ananya343B/pes_asic_class/assets/142582353/a3e930f6-417a-412e-a4e9-98d4f93ef560)

#### C program to find sum of numbers from 1 to 9:

![lab2code](https://github.com/ananya343B/pes_asic_class/assets/142582353/2f6b3976-fab5-435e-aabc-643d924f30fb)

#### Assembly code:

![assembly_code](https://github.com/ananya343B/pes_asic_class/assets/142582353/629c1e6d-f9bb-41f5-b9d1-585c82b0b081)

#### Output:

![day2op](https://github.com/ananya343B/pes_asic_class/assets/142582353/5d717325-2ce8-4138-831f-74b8cc6bde62)

</details>

#### RTL design using verilog with SKY130 technology
<details>
<summary> 
 Day 3
</summary>
<br>

### Introduction to iVerilog

##### Simulator:
Simulation is a technique of applying different input stimulus to the design at different times to check if the RTL code behaves the intended way. Essentially, simulation is a well-followed technique to verify the robustness of the design.

How simulator works:

Simulator looks for changes in the input signals and corresponding to them, the output is evaluated.

##### Design:
A Verilog design consists of a hierarchy of modules. Modules encapsulate design hierarchy, and communicate with other modules through a set of declared input, output, and bidirectional ports.

##### Test bench:
Testbench is a code module that describes the stimulus to a logic design and checks whether the design's outputs match its specification.

![Screenshot from 2023-08-27 14-46-58](https://github.com/ananya343B/pes_asic_class/assets/142582353/8d608b57-b7ea-45c6-8901-828659e6c6a7)

Design may have one or more primary inputs and one or more primary outputs.
Test benche does not have primary inputs or outputs.

##### Iverilog based simulation flow:

![Screenshot from 2023-08-27 14-49-37](https://github.com/ananya343B/pes_asic_class/assets/142582353/b6cd9d93-767b-46dd-8d19-10df560d4b59)

### Lab1 - Using iVerilog and gtkwave

![Screenshot from 2023-08-27 15-49-04](https://github.com/ananya343B/pes_asic_class/assets/142582353/7c5282fe-73ee-4cec-bf31-903940626939)

Create a directory called vsd.

By using ```git clone``` we create a folder called ```sky130RTLDesignAndSynthesisWorkshop``` in ```vsd```.

```verilog_files``` contains all the verilog source files and test bench files.

![Screenshot from 2023-08-27 15-49-31](https://github.com/ananya343B/pes_asic_class/assets/142582353/84e8795e-548a-495c-b8b4-a13704a3a846)

Load the source code and testbench for ```good_mux.v``` into iverilog simulator.

It generates output file which is opened in gtkwave simulator.

![Screenshot from 2023-08-27 15-48-23](https://github.com/ananya343B/pes_asic_class/assets/142582353/d0f77e0f-9c79-4449-a8f7-5f5155dc1f4c)

The source code for ```good_mux.v``` and ```tb_good_mux.v``` are as follows:

![Screenshot from 2023-08-27 15-53-09](https://github.com/ananya343B/pes_asic_class/assets/142582353/26fca6bf-d992-4c6f-a490-accf460375a6)

![Screenshot from 2023-08-27 15-52-37](https://github.com/ananya343B/pes_asic_class/assets/142582353/dc20bac3-7410-4a9b-8f6f-37561a26ee39)


### Introduction to yosys and Logic synthesis 



##### Synthesizer 

Tool for converting RTL to netlist. 

![Screenshot from 2023-08-27 16-41-03](https://github.com/ananya343B/pes_asic_class/assets/142582353/9fa9601e-7dd8-42df-8954-39ad8821b43d)

Here we will be using yosys.

##### Yosys
Yosys is a framework for Verilog RTL synthesis. Yosys provides a collection of tools and algorithms to transform high level RTL to gate level representations which may be used forphysical implementation on hardware.

Design and .lib files are taken in by the synthesizer to give a netlist file.Netlist is a representation of design in the form of standard cells.

![Screenshot from 2023-08-27 16-31-39](https://github.com/ananya343B/pes_asic_class/assets/142582353/63ef4c2a-49a2-48c1-9ce5-ac58a495372f)

1) read_verilog - reads verilog file
2) read_liberty - reads .lib file
3) write_verilog - writes out netlist file

##### Verify the synthesis

![Screenshot from 2023-08-27 16-37-08](https://github.com/ananya343B/pes_asic_class/assets/142582353/5b7e7fa2-3d88-4648-be9f-3030ffc81d2f)

Netlist and testbench is fed to the iverilog simulator. A vcd file is generated which is fed to gtkwave simulator. 

Stimulus must be the same as output observed during RTL simulation.

Set of primary inputs and outputs will be the same as RTL.
Same testbench can be used for synthesized netlist.

##### Introduction to Logic synthesis

**Logic Synthesis**
  - Logic synthesis is a process in digital design that transforms a high-level hardware description of a digital circuit, typically in a hardware description language (HDL) like Verilog or VHDL, into a lower-level representation composed of logic gates and flip-flops.
  - The goal of logic synthesis is to optimize the design for various criteria such as performance, area, power consumption, and timing.

**.lib**
   - It is a collection of logical modules like And, Or, Not etc.
   - It has different flavors of same gate like 2 input AND gate, 3 input AND gate etc with different performace speed.
  
 **Need for different flavours of gates**
  - In order to make a circuit faster, the clock frequency should be high.
  - For that, the time period of the clock should be as low as possible as fmax=1/tmin.
  - For a smaller propagation time, we need faster cells.
  - To ensure that there are no HOLD issues at flip-flop B, we require slow cells.

**Faster Cells vs Slower Cells**
  - Load in digital circuit is of Capacitence.
  - Faster the charging or dicharging of capacitance, lesser is the cell delay.
  - However, for a quick charge/ discharge of capacitor, we need transistors capable of sourcing more     current i.e, we need **wide transistors**.
  - Wider transistors have lesser delay but consume more area and power.
  - Narrow transistors have more delay but consume less area and performance.
  - Faster cells come with a cost of area and power.
  - Hence the cells are chosen for a design such that all contraints are met.

##### Lab - Yosys

Invoking yosys:

![Screenshot from 2023-08-27 19-16-20](https://github.com/ananya343B/pes_asic_class/assets/142582353/9826a37d-f87f-4c81-8759-bcd2272c2c47)

To read the library
    
     ` ``read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```
    
To read the design

    ```read_verilog good_mux.v```

To syntheis the module

      ``` synth -top good_mux```
      
![Screenshot from 2023-08-27 19-19-53](https://github.com/ananya343B/pes_asic_class/assets/142582353/b47c7713-9942-4703-baf3-a34cae4e2c77)


To generate the netlist

  `abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

 ![Screenshot from 2023-08-27 19-20-09](https://github.com/ananya343B/pes_asic_class/assets/142582353/9fc2f494-f200-4314-b30c-9267ea9cafcc)

  It gives a report of what cells are used and the number of input and output signals.

To see the logic realised

  `show`
  
![Screenshot from 2023-08-27 19-21-58](https://github.com/ananya343B/pes_asic_class/assets/142582353/6217e753-dc69-47ae-a81c-840b430b7a85)

To write the netlist

      ````write_verilog good_mux_netlist.v````
      ````!gvim good_mux_netlist.v````
     
To view a simplified code
     
     ``` write_verilog -noattr good_mux_netlist.v```
     ```!gvim good_mux_netlist.v```
     
![writing1](https://github.com/ananya343B/pes_asic_class/assets/142582353/1df99a5a-1ed9-4ba0-8420-75aab8a32913)


![writing2](https://github.com/ananya343B/pes_asic_class/assets/142582353/d6e24c76-22f4-430e-be72-f10a79e3d5cc)

</details>


<details>
<summary> 
 Day 4
</summary>
<br>

### Introduction to timing dot libs

##### Lab

To view the contents in the .lib

  `gvim ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

Use ```syn off``` to remove syntax.

![Screenshot from 2023-08-28 08-13-39](https://github.com/ananya343B/pes_asic_class/assets/142582353/b120b29c-d2ce-4621-93ab-ff80d1b1a1c4)

The first line in the file `library ("sky130_fd_sc_hd__tt_025C_1v80") ` :

   - tt : indicates variations due to process and here it indicates **Typical Process**.
   - 025C : indicates the variations due to temperatures where the silicon will be used.
   - 1v80 : indicates the variations due to the voltage levels where the silicon will be incorporated.

The properties of the cell can be veiwed:

![Screenshot from 2023-08-28 08-21-10](https://github.com/ananya343B/pes_asic_class/assets/142582353/edb662d5-79df-49d1-8d4e-427aadfc6d97)


`` :se nu`` - to enable line numbers.

`` /cell`` and ``:g//`` - to list all the cells.

![Screenshot from 2023-08-28 08-33-34](https://github.com/ananya343B/pes_asic_class/assets/142582353/2c9dddf5-70d4-4769-8822-b993a53434fb)

We can observe all the different types and flavours of cells in .lib

```:vsp``` - to compare cells.

![Screenshot from 2023-08-28 16-09-59](https://github.com/ananya343B/pes_asic_class/assets/142582353/bc7d7149-4d2a-462c-b840-933c1d712014)

We can compare the power consumption and area of different flavours of and cells in the above image.

### Heirarchical and Flattened synthesis

##### Heirarchical synthesis:

The hierarchy approach, sometimes known as the “divide and conquer” strategy, is breaking a module down into smaller units and then repeating the process on those units until the complexity of the smaller portions is manageable. The smaller modules and sub-circuits are synthesized individually and then integrated together. This approch helps designers to work on different parts of the design individually and helps manage the complexity of large modules.

##### Lab:

 Use ``` gvim multiple_modules.v``` to open the file.

 ![Screenshot from 2023-08-28 16-34-56](https://github.com/ananya343B/pes_asic_class/assets/142582353/db42a31c-9697-494f-bbb8-edd941e0f690)

 The file ```multiple_modules.v``` contains two sub-modules ```sub_module1``` and ```sub_module2```.

 perform the follewing after launching yosys:
 
![Screenshot from 2023-08-28 16-37-30](https://github.com/ananya343B/pes_asic_class/assets/142582353/a75f5368-c282-4343-aa44-f117b9f68dcd)

![Screenshot from 2023-08-28 16-38-39](https://github.com/ananya343B/pes_asic_class/assets/142582353/7ca504f8-bff0-45bf-a385-97accabaadb1)

![Screenshot from 2023-08-28 16-39-09](https://github.com/ananya343B/pes_asic_class/assets/142582353/1549ada9-2c03-455b-be04-7403f0f8b536)

![Screenshot from 2023-08-28 16-39-35](https://github.com/ananya343B/pes_asic_class/assets/142582353/7bb61349-ca55-472c-8fda-fc530ab629d0)

![Screenshot from 2023-08-28 16-40-43](https://github.com/ananya343B/pes_asic_class/assets/142582353/3b8ab335-16af-4df5-ab9a-7590e832a7ce)


To set ```multiple_modules.v``` as top module - ```synth -top multiple_modules```

To view net-list - ```show multiple_modules```

![Screenshot from 2023-08-28 16-41-30](https://github.com/ananya343B/pes_asic_class/assets/142582353/46176939-7e6f-4b33-af88-b1d80f340fac)

We can observe that AND gate and OR gate are replaced as ```sub_module1``` and ```sub_module2```
` in the above representation.

Open ```multiple_modules_hier.v``` using ```!gvim multiple_modules_hier.v```

![multi_mod1](https://github.com/ananya343B/pes_asic_class/assets/142582353/3f6a8e9f-497a-487b-b8d9-14855c839e93)

![multi_mod2](https://github.com/ananya343B/pes_asic_class/assets/142582353/615231ab-5e0b-4593-82b5-3f74981de248)

##### Flattened synthesis:

Flattening combines all the modules and sub-modules to produce a single entity. The entire design is synthesized as a single unit, without preserving the modular organization present in the original high-level description. It produces fast logic (by minimizing the levels of logic between the inputs and outputs) at the expense of the area increase.

##### Lab:

Follow the same steps as in heirarchical but use ```flatten``` before the ```show``` command to flatten the netlist.

![Screenshot from 2023-08-28 19-38-46](https://github.com/ananya343B/pes_asic_class/assets/142582353/2587cb97-ea36-4ff9-9d2c-e1a564538e04)

Opening the file using ```!gvim multiple_modules_flat.v```

![Screenshot from 2023-08-28 19-40-11](https://github.com/ananya343B/pes_asic_class/assets/142582353/ebf66455-50a1-4f92-a4f3-ae1eb35acaad)

![Screenshot from 2023-08-28 19-40-45](https://github.com/ananya343B/pes_asic_class/assets/142582353/1a9bc09a-ec61-4664-ab03-38a204139526)





</details>
