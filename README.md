
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
  - However, for a quick charge/ discharge of capacitor, we need transistors capable of sourcing more     current i.e, we need wide transistors.
  - Wider transistors have lesser delay but consume more area and power.
  - Narrow transistors have more delay but consume less area and performance.
  - Faster cells come with a cost of area and power.
  - Hence the cells are chosen for a design such that all contraints are met.

##### Lab - Yosys

Invoking yosys:

![Screenshot from 2023-08-27 19-16-20](https://github.com/ananya343B/pes_asic_class/assets/142582353/9826a37d-f87f-4c81-8759-bcd2272c2c47)

To read the library
    
```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```
    
To read the design

```read_verilog good_mux.v```

To syntheis the module

``` synth -top good_mux```
      
![Screenshot from 2023-08-27 19-19-53](https://github.com/ananya343B/pes_asic_class/assets/142582353/b47c7713-9942-4703-baf3-a34cae4e2c77)


To generate the netlist

```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

 ![Screenshot from 2023-08-27 19-20-09](https://github.com/ananya343B/pes_asic_class/assets/142582353/9fc2f494-f200-4314-b30c-9267ea9cafcc)

  It gives a report of what cells are used and the number of input and output signals.

To see the logic realised

```show```
  
![Screenshot from 2023-08-27 19-21-58](https://github.com/ananya343B/pes_asic_class/assets/142582353/6217e753-dc69-47ae-a81c-840b430b7a85)

To write the netlist

```write_verilog good_mux_netlist.v```

```!gvim good_mux_netlist.v```
     
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

  ```gvim ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

Use ```syn off``` to remove syntax.

![Screenshot from 2023-08-28 08-13-39](https://github.com/ananya343B/pes_asic_class/assets/142582353/b120b29c-d2ce-4621-93ab-ff80d1b1a1c4)

`library("sky130_fd_sc_hd__tt_025C_1v80") ` :

   - tt : indicates variations due to process and here it indicates typical Process.
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


### Various flops styles and optimization

##### Need for flip flops

Flip-flops are fundamental components in design. They are essential for storing binary information in digital circuits, enabling sequential logic and memory functions. Flip-flops play a crucial role in synchronizing data and controlling the flow of information within integrated circuits, making them essential for building complex digital systems like processors, memory units, and other digital devices.

##### Flip flops and glitches

Flip-flops are used in digital circuits to help eliminate glitches, which are unwanted, transient, and unpredictable pulses or voltage spikes in a circuit. By storing an synchronising data, flip-flops can help filter out glitchesand ensure that only stable and intended signals propogate through the circuit. The sequential nature of flip-flops allows them to delay signals and prevent short-lived glitches from affecting the overall behavior of the circuit. This is particularly important in synchronous digital systems where accurate and reliable signal propagation is critical.

##### D flip flop with asynchronous reset 

When the reset is high, the output of the flip-flop is forced to 0, irrespective of the clock signal.

Else, on the positive edge of the clock, the stored value is updated at the output.

```gvim dff_asyncres.v```

![Screenshot from 2023-08-28 22-10-39](https://github.com/ananya343B/pes_asic_class/assets/142582353/7b3b6771-1536-4aa2-af5f-1f36ca318600)

Simulation:

``` iverilog dff_asyncres.v tb_dff_asyncres.v```

```./a.out```

```gtkwave tb_dff_asyncres.vcd```

![dff_asyncres_timing](https://github.com/ananya343B/pes_asic_class/assets/142582353/dceedb31-f734-403a-83fe-6afe395a7894)

Synthesis:

Open yosys

``` read_liberty -lib ../lib/sky130_fd_sc_hd__tt_250C_1v80.lib```

```read_verilog dff_asyncres.v```

```synth -top dff_asyncres```

``` dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_250C_1v80.lib```

``` abc -liberty ../lib/sky130_fd_sc_hd__tt_250C_1v80.lib```

```show```

![dff_asyncres_rep](https://github.com/ananya343B/pes_asic_class/assets/142582353/91c55f61-b9c9-4a4f-8458-f8241ffe6556)


##### D flip flop with asynchronous set

When the set is high, the output of the flip-flop is forced to 1, irrespective of the clock signal.

Else, on positive edge of the clock, the stored value is updated at the output.

```gvim dff_async_set.v```

![Screenshot from 2023-08-28 22-10-02](https://github.com/ananya343B/pes_asic_class/assets/142582353/2960b5d1-6b76-4e65-80bd-75fc190dad89)

Simulation

``` iverilog dff_async_set.v tb_dff_async_set.v```

```./a.out```

```gtkwave tb_dff_async_set.vcd```

![dff_asyncset_timing](https://github.com/ananya343B/pes_asic_class/assets/142582353/379342e3-940d-4806-8b05-a6e9cf947b6a)

Synthesis:

Open yosys

``` read_liberty -lib ../lib/sky130_fd_sc_hd__tt_250C_1v80.lib```

```read_verilog dff_async_set.v```

```synth -top dff_async_set```

``` dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_250C_1v80.lib```

``` abc -liberty ../lib/sky130_fd_sc_hd__tt_250C_1v80.lib```

```show```

![dff_asyncset_rep](https://github.com/ananya343B/pes_asic_class/assets/142582353/0621ba03-39a9-46f2-b936-622e0f724a94)


##### D flip flop with synchronous reset 

When the set is high, the output of the flip-flop is forced to 1, irrespective of the clock signal.

Else, on positive edge of the clock, the stored value is updated at the output.

```gvim dff_syncres.v``` 

![Screenshot from 2023-08-28 22-09-21](https://github.com/ananya343B/pes_asic_class/assets/142582353/4eb64989-24cf-4f63-ac9d-46480021c351)

Simulation

``` iverilog dff_syncres.v tb_dff_syncres.v```

```./a.out```

```gtkwave tb_dff_syncres.vcd```

![dff_syncres_timing](https://github.com/ananya343B/pes_asic_class/assets/142582353/986d2ae8-9ec6-4ef7-85d5-7b8610a5cd04)

Synthesis:

Open yosys

``` read_liberty -lib ../lib/sky130_fd_sc_hd__tt_250C_1v80.lib```

```read_verilog dff_syncres.v```

```synth -top dff_syncres```

``` dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_250C_1v80.lib```

``` abc -liberty ../lib/sky130_fd_sc_hd__tt_250C_1v80.lib```

```show```

![dff_synchres_rep](https://github.com/ananya343B/pes_asic_class/assets/142582353/af0a7668-2002-4acf-befe-0a1382132dab)


##### D flip flop with asynchronous reset and synchronous reset 

When the asynchronous resest is high, the output is forced to 0.

When the synchronous reset is high at the positive edge of the clock, the output is forced to 0.

Else, on the positive edge of the clock, the stored value is updated at the output.

Here, it is a combination of both synchronous and asynchronous reset DFF.

```gvim dff_asyncres_syncres.v```

![Screenshot from 2023-08-28 22-08-46](https://github.com/ananya343B/pes_asic_class/assets/142582353/08447636-ef5d-41d4-a4a0-9611c7302587)


### Interesting optimizations

Case 1:

```gvim mult_2.v```

![mul2_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/1d353aec-6f54-48d4-a285-fe901e5303b8)

input a - 3 bit

output y - 4 bit

relation - y=2*a

Number 'a' multiplied by 2 is appended by bit 0 at the right hand side, i.e  2*a={a,0}.

Therefore no extra hardware is required for multiplication of a number by powers of 2.

Synthesis:

Launch yosys

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_250C_1v80.lib```

```read_verilog mult_2.v```

```synth -top mul2```

```abc -liberty ../lib/sky130_fd_sc_hd__tt_250C_1v80.lib```

```show```

![mul2_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/f03c774a-b705-4ab3-b97f-785e2ca4e180)

![mul2_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/de156518-031a-4de7-8fd4-49237dbe1fad)

Netlist

```write_verilog -noattr mul2_netlist.v```

```!gvim mul2_netlist.v```

![mul2_netl](https://github.com/ananya343B/pes_asic_class/assets/142582353/e52cc5e9-ec6f-4259-b36b-29c9f18a5b29)

Case 2:

input a - 3 bit

output y - 6 bit

relation - y=9*a

```gvim mult_8.v```

![mul8_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/301d7dcd-b1cd-401c-ad08-331e4921ab96)

Synthesis:

Launch yosys

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```read_verilog mult_8.v```

```synth -top mult8```

```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```show```

![mul8_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/dc5dba90-1ecf-4e8d-bb12-57b3007a728b)

![mul8_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/1ca0b57e-a6a2-4796-80c4-965b1151954e)

Netlist:

```write_verilog -noattr mul8_netlist.v```

 ```!gvim mult8_netlist.v```
 
![mul8_netl](https://github.com/ananya343B/pes_asic_class/assets/142582353/a89699ca-2b7f-4254-bc4d-e0f4f13622ab)


</details>


<details>
<summary> 
 Day 5
</summary>
<br>

### Combinational and Sequential Optimizations

### Combinational logic optimizations:

- Combinational logic refers to a type of digital logic design where the output is solely determined by the current input values, and there are no memory elements involved. Examples of combinational circuits include adders, multiplexers, demultiplexers, comparators, and more.

- Squeezing the logic to get the most optimised design (Area and power savings)

##### Optimisation techniques

**1) Constant Propogation (Direct Optimisation)**

- identify signals that are derived from constant inputs or other signals with constant values.
        
- Replace these signals with their constant values throughout the logic.
        
- Update downstream logic accordingly, simplifying the circuit.
        
- This optimization eliminates unnecessary logic and reduces gate count, improving circuit efficiency and performance.
        
 **2) Boolean logic optimization (using K-map or Quine McKluskey)**
 
- Apply Boolean algebra rules to simplify logic expressions, using techniques like factorization, distribution, and absorption.

- Use Karnaugh Maps (K-Maps) to identify patterns and group terms for simplification.
        
- Eliminate redundant terms and simplify expressions further.
        
- This optimization reduces the number of gates, improves circuit performance, and enhances overall efficiency.

### Sequential Logic Optimizations

- Sequential Logic is a fundamental concept in digital circuit design that involves elements capable of storing information (memory elements like flip-flops and latches) and producing outputs based not only on current inputs but also on past inputs and internal states.It forms the basis for many digital devices, including microcontrollers, processors, and communication systems.
  
- Designers must carefully evaluate the trade-offs between speed, power, and complexity to create effective and optimized sequential logic designs.

##### Optimisation techniques

**i)Basic**

**1) Sequential constant propagation**

- Sequential constant propagation is an optimization technique that involves identifying and replacing intermediate signals within a sequential circuit with their constant values. This technique aims to eliminate unnecessary calculations and logic, reducing the complexity of the circuit.

**ii) Advanced**

   **1) State optimization**
   
- State optimization focuses on reducing the number of states in a finite state machine (FSM) or reducing the complexity of state transitions. By eliminating redundant or unreachable states and simplifying the transition logic, designers can create more efficient and streamlined state machines.
        
   **2)Retiming**
   
- Retiming is a technique used to balance the delay of a sequential circuit by moving flip-flops within the design. By strategically relocating flip-flops along the critical path, designers can minimize propagation delays and improve the overall performance of the circuit.
        
   **3)Sequential logic cloning (Floor Plan Aware Synthesis)**
   
- Sequential logic cloning involves duplicating a portion of a sequential circuit to optimize its performance. This technique is particularly useful for critical paths where excessive delays are present. By replicating a section of the circuit and introducing additional registers, designers can reduce the delay along the path.

### Lab

##### Combinational logic optimization

**opt_check**

```gvim opt_check.v```

![oc1_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/f2bbc05f-cb23-4141-89c9-5335eeb64283)

Synthesis:

Launch yosys

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```read_verilog opt_check.v```

```synth -top opt_check```

For constant Propogation optimisation:

```opt_clean -purge```
 
```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```show```

![oc1_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/51592d86-8ab2-4436-9a39-2b4ed553fc4b)

![oc1_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/33038392-8143-466a-9303-9de62e3a6457)


**opt_check2**

```gvim opt_check2.v```

![oc2_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/17d9ed01-5cf5-41d5-b5ac-89cda648a241)

Synthesis:

Launch yosys

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```read_verilog opt_check2.v```

```synth -top opt_check2```

```opt_clean -purge```

```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```show```

![oc2_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/b06c7568-7336-4094-bf16-5e0d81f3c54c)

![oc2_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/f44718b9-4644-4ff9-8b75-c9dd980302c0)


**opt_check3**

```gvim opt_check3.v```

![oc3_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/1eaac6d3-3985-4eb3-a8d8-d7436c6d9cd7)


Synthesis:

Launch yosys

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```read_verilog opt_check3.v```

```synth -top opt_check3```

```opt_clean -purge```

```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```show```

![oc3_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/84896a1b-8169-4b87-89fb-eafe640b1fdd)

![oc3_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/0cf821d3-9e4c-4e33-93c3-285cd3f7606f)


**opt_check4**

```gvim opt_check4.v```

![oc4_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/e1675b28-f749-4fea-a229-95ffc1e4337f)


Synthesis:

Launch yosys

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```read_verilog opt_check4.v```

```synth -top opt_check4```

```opt_clean -purge```

```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```show```

![oc4_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/06d8dca5-7991-4ae2-91b7-0af08cb2e5d2)

![oc4_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/42b41188-9207-4739-a373-bd1b24c5085f)


**multiple_module_opt**

```gvim multiple_module_opt.v```

![multi_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/1b476b01-d4f2-465f-b0c3-687bd1a28823)


Synthesis:

Launch yosys

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```read_verilog multiple_module_opt.v```

```synth -top multiple_module_opt```

```opt_clean -purge```

```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```show```

![multi_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/0d169160-596c-4cd9-aa0b-6e44d2f7fe0e)

![multi_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/54a8aa67-0907-4939-9a30-a6910b31444f)


##### Sequential logic optimization

**dff_const1**

```gvim dff_const1.v```

![dc1_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/79f99feb-a9fa-4a12-8e13-1a978f2e91cb)

Simulation:

```iverilog dff_const1.v tb_dff_const1.v```

```./a.out```

```gtkwave tb_dff_const1.vcd```

![dc1_sim](https://github.com/ananya343B/pes_asic_class/assets/142582353/74c47c31-d337-4f9d-a23c-9226d5d0483a)

Synthesis:

Launch yosys

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```read_verilog dff_const1.v```

```synth -top dff_const1```

```dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```show```

![dc1_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/0c0f6c9c-7fcc-4fa1-b3be-49a451c48cda)

![dc1_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/6e33ff6e-0a2d-4e6b-bc30-a187b1e994ea)


**dff_const2**


```gvim dff_const2.v```

![dc2_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/44aee1ff-07e0-43a4-9e66-128c37242d9a)

Simulation:

```iverilog dff_const2.v tb_dff_const2.v```

```./a.out```

```gtkwave tb_dff_const2.vcd```

![dc2_sim](https://github.com/ananya343B/pes_asic_class/assets/142582353/8259659f-12d9-43d5-8386-e00eee365ac7)

Synthesis:

Launch yosys

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```read_verilog dff_const2.v```

```synth -top dff_const2```

```dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```show```

![dc2_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/615bb96f-4185-444b-9e69-e0c6043b4772)

![dc2_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/027b098c-ca67-43a2-8d31-5d1576969676)


**dff_const3**

```gvim dff_const3.v```

![dc3_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/b8c453ab-3cf9-48ec-941a-01d61dcf6217)


Simulation:

```iverilog dff_const3.v tb_dff_const3.v```

```./a.out```

```gtkwave tb_dff_const3.vcd```

![dc3_sim](https://github.com/ananya343B/pes_asic_class/assets/142582353/e596bafa-5b6b-492f-b5f2-8275a7bece3e)


Synthesis:

Launch yosys

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```read_verilog dff_const3.v```

```synth -top dff_const3```

```dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```show```

![dc3_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/44baf893-6536-4726-a3d4-1044be456421)

![dc3_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/2ee3d35c-eac7-4de6-8a87-eb6d3974bf27)


**dff_const4**

```gvim dff_const4.v```
![dc4_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/1dfa58c1-7aaa-4cfa-8c34-8eb308eab08c)


Simulation:

```iverilog dff_const4.v tb_dff_const4.v```

```./a.out```

```gtkwave tb_dff_const4.vcd```

![dc4_sim](https://github.com/ananya343B/pes_asic_class/assets/142582353/d8a43fb0-0dd0-4f46-915b-e1795a3ed493)


Synthesis:

Launch yosys

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```read_verilog dff_const4.v```

```synth -top dff_const4```

```dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```show```

![dc4_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/926b869a-867b-4ebf-88ff-741658db190d)

![dc4_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/bb8a741a-9e24-498d-a6b5-02a15142d0b8)


**dff_const5**

```gvim dff_const5.v```

![dc_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/ba209159-58da-4abb-a1dc-6ebf41bda003)


Simulation:

```iverilog dff_const5.v tb_dff_const5.v```

```./a.out```

```gtkwave tb_dff_const5.vcd```

![dc_sim](https://github.com/ananya343B/pes_asic_class/assets/142582353/40fa2007-24e6-4b5d-bd90-5d97d60019d5)

Synthesis:

Launch yosys

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```read_verilog dff_const5.v```

```synth -top dff_const5```

```dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```show```

![dc_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/9e9708b3-14a2-4b8f-8831-dc1fd90b2897)

![dc_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/9ec851b6-519c-4e76-b5cb-7b80c3b567f6)


##### Sequential optimizations for unused outputs

**counter_opt**

```gvim counter_opt.v```

![co_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/57f68073-fe63-43da-b1d9-0d54f63a8965)

Synthesis:

Launch yosys

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```read_verilog counter_opt.v```

```synth -top counter_opt```

```dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```show```

![co_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/7d5a9ff7-eee0-4883-a07d-68026f5db8f9)

![co_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/197f057a-e76d-456d-ac87-abd26f18c627)


**counter_opt2**

```gvim counter_opt2.v```

![co2_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/701f9bbb-7747-4081-b6ee-5c270c8d305d)


Synthesis:

Launch yosys

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```read_verilog counter_opt2.v```

```synth -top counter_opt2```

```dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```show```

![co2_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/9b48a11c-e924-421f-8d8d-013b4f6d28f1)

![co2_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/a7f8e120-ce20-4f87-8ab7-c57523127432)



</details>


<details>
<summary> 
 Day 6
</summary>
<br>

### GLS, Blocking vs Non-blocking, Synthesis simulation mismatch

### GLS

- **What is GLS??**
  
	- It is a verification process in digital design where the gate-level netlist of a design is simulated to ensure that it behaves as expected after the synthesis process. In gate-level simulation, the design is represented using actual gate-level components (AND gates, OR gates, flip-flops, etc.) and their interconnections.
   
	- Run the testbench with netlist as the design under test
   
	- Netlist is logically same as RTL code (same TB will allign with the design)
   
- **Why GLS??**
	- To verify the logical correctness of design under synthesis
   
	- Ensuring the timing of the design is met
   
	- It helps designers catch post-synthesis errors, timing issues, and other potential design flaws.
   
- **GLS using iverilog**

![gsl1](https://github.com/ananya343B/pes_asic_class/assets/142582353/3129bf4e-f560-498a-9a5e-2093109dd726)

### Synthesis-Simulation mismatch

Discrepancies or inconsistencies that can arise between the behavior of a digital design as simulated and its behavior after synthesis.

- **Reasons**
	- Missing sensitivity List
   
	- Blocking vs Non-blocking assignments
   
	- Non standard Verilog coding
   
Addressing and minimizing synthesis-simulation mismatch is crucial for ensuring the correctness and reliability of digital designs. Careful validation, consistency in constraints, and understanding the intricacies of the synthesis process are key steps in mitigating this type of issue.

### Missing sensitivity list

Simulator always looks for activity, ie. it acts upon the output only when there is a change in the input.

Example: Below is the code for a 2:1 mux.

	module mux(input i0,
 		      input i1,
	 	      input sel,
		      output reg y);
	always@(sel)
 		begin
   		if(sel)
     			y=i1;
		else
  			y=i0;
     		end
       endmodule

Here always is evaluated only when there is a change in `sel`.

It acts as a latch.

If  ```always@(*)```  is used instead, y is evaluated for any change in `i0,i1,sel`. Therefore it acts like a mux.
     
### Blocking and Non-blocking statements in verilog

Blocking and non-blocking statements are two types of assignment statements used in Verilog, a hardware description language. They serve different purposes in describing how assignments are executed within a procedural block of code, such as an 'always' or 'initial' block.

- **Blocking Assignment**

    - Blocking assignments in Verilog use the `=` operator for assignment.
      
    - A blocking assignment is executed sequentially, meaning the next statement will not be executed until the current assignment is complete.
      
    - The value on the right side of the assignment is immediately assigned to the left-hand side, and the process waits for the assignment to complete before moving on.
      
    - It represents a procedural programming-style behavior.
 
- **Non-blocking Assignment**

    - Non-blocking assignments in Verilog use the `<=` operator for assignment.
      
    - A non-blocking assignment schedules the assignment to take place at the end of the current time step without affecting the order of execution of subsequent statements.
      
    - It is commonly used to model concurrent behavior, particularly in sequential logic circuits, by allowing multiple assignments to occur simultaneously within the same time step.
    -
    - It represents hardware modeling and concurrent behavior more accurately.


### Caveats with blocking statements

Using blocking statements (`=`) in Verilog can introduce certain caveats and potential issues in your designs. 

Example:

	module code(input clk,
 		    input reset,
       		    input d,
	     	    output reg q);
	reg q0;
 	always@(posedge clk,posedge reset)
  	begin
   	if(reset)
    		begin
      		q0=1'b0;
		q=1'b0;
  		end
    	else
     		begin
       		q=q0;
	 	q0=d;
   		end
     	end
      endmodule

The code works in normal condition in the above example. However, if the order of statements  

`q=q0;
q0=d;`

are changed to

`q0=d;
q=q0;`

only one flop will be generated instead of two. To rectify the problem, we use non-blocking statements, where the order does not matter.

Therefore non-blocking statements are used in writing sequential logic.


Here are some important considerations to keep in mind to avoid such caveats:

1. **Sequential Execution:** Blocking assignments are executed sequentially, meaning that each assignment must complete before the next one starts. This can lead to unintended delays and might not accurately capture concurrent behavior.

2. **Race Conditions:** When multiple blocking assignments are used to update the same variable within a procedural block, the final value of the variable is determined by the order of execution. This can lead to race conditions where simulation results might not match hardware behavior.

3. **Combinational Logic:** In combinational logic circuits, using only blocking assignments might not accurately model concurrent behavior. Combinational logic should ideally use non-blocking assignments (`<=`) to capture concurrent signal updates within the same time step.

4. **Sensitivity Lists:** Procedural blocks using blocking assignments should have accurate sensitivity lists to ensure that the code executes when the appropriate events occur. Incorrect sensitivity lists can lead to unexpected simulation behavior.

5. **Simulation vs. Hardware Behavior:** The sequential execution of blocking assignments in simulation might not accurately represent the concurrent behavior of hardware circuits.

6. **Nested Blocking Blocks:** Nesting blocking assignment blocks within each other can lead to unintended delays and can complicate the design.

7. **Timing Analysis:** If used improperly, blocking assignments in synchronous logic (such as setting flip-flop inputs) can lead to incorrect timing analysis results.

8. **State Machines:** When describing state machines, using only blocking assignments might lead to incorrect state transitions if transitions are not carefully managed.


**Best Practices to Mitigate Caveats:**

1. **Use Non-Blocking Assignments:** For modeling concurrent behavior in sequential logic circuits, use non-blocking assignments (`<=`) to avoid race conditions and capture the expected hardware behavior.

2. **Reserve Blocking for Sequential Logic:** Use blocking assignments (`=`) for simple sequential operations where one operation must complete before the next one starts.

3. **Minimize Multiple Blocking Assignments:** Avoid multiple blocking assignments to the same variable within a single procedural block to prevent race conditions.

4. **Sensitivity List Integrity:** Ensure that sensitivity lists in your procedural blocks are accurate and capture the necessary triggers for execution.

5. **Separate Logic Types:** Clearly separate sequential logic (clocked processes) from combinational logic (non-blocking assignments) to maintain accurate modeling.

6. **Avoid Nesting Blocks:** Minimize nesting of blocking assignment blocks to avoid unintended timing effects.

7. **Consult Simulation Warnings:** Pay attention to simulation tool warnings or messages related to the usage of blocking assignments. They can offer insights into potential issues.


### Lab

##### ternary_operator_mux

```cd vsd/sky130RTLDesignAndSynthesisWorkshop/verilog_files```

```gvim teranry_operator_mux.v```

![tom_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/154d10b0-f2e1-4267-be2c-3e2548868834)

**Simulation**

```iverilog ternary_operator_mux.v tb_ternary_operator_mux.v```

```./a.out```

```gtkwave tb_ternary_operator_mux.vcd```

![tom_sim](https://github.com/ananya343B/pes_asic_class/assets/142582353/afd0186f-511b-4ea8-84ae-f54827f39a49)

**Synthesis**

```yosys```

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```read_verilog ternary_operator_mux.v```

```synth -top ternary_operator_mux```

```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```show```

![tom_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/195db590-ff36-47f1-a3a7-6c203f29af0b)


![tom_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/79bc9dd5-3b15-4cf8-bf5b-7c1fe11d53bd)

**Gate-Level Simulation**

```iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v```

```./a.out```

```gtkwave tb_bad_mux.vcd```

![tom_gls](https://github.com/ananya343B/pes_asic_class/assets/142582353/010f0fa4-c3c7-46ec-9fd3-b15917b07fed)


##### bad_mux

```cd vsd/sky130RTLDesignAndSynthesisWorkshop/verilog_files```

```gvim bad_mux.v```

![badm_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/11640a89-8df8-45ee-8146-1913afa61354)

**Simualtion**

```iverilog bad_mux.v tb_bad_mux.v```

```./a.out```

```gtkwave tb_bad_mux.vcd```

![badm_sim](https://github.com/ananya343B/pes_asic_class/assets/142582353/e0c57712-9316-4002-8305-cf43ab261dbd)

**Synthesis**

```yosys```

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```read_verilog bad_mux.v```

```synth -top bad_mux```

```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```show```

![badm_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/3f643dc1-9704-420f-89fd-7f5695b349fb)


![badm_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/d2d9efe2-9492-463c-b9c0-971b0e219021)


**Gate-Level Simulation**

```iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v```

```./a.out```

```gtkwave tb_bad_mux.vcd```

![badm_gls](https://github.com/ananya343B/pes_asic_class/assets/142582353/34b787da-9c61-4437-821f-0f7e228902f5)


##### Synth-Sim mismatch for blocking statements

##### blocking_caveat

```cd vsd/sky130RTLDesignAndSynthesisWorkshop/verilog_files```

```gvim blocking_caveat.v```

![blockc_file](https://github.com/ananya343B/pes_asic_class/assets/142582353/a1749b25-ae9b-4159-9061-8f2a11fcae7d)


**Simualtion**

```iverilog blocking_caveat.v tb_blocking_caveat.v```

```./a.out```

```gtkwave tb_blocking_caveat.vcd```

![blockc_sim](https://github.com/ananya343B/pes_asic_class/assets/142582353/d5682f59-b923-482e-b4c8-a71e8787cfca)

**Synthesis**

```read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```read_verilog blocking_caveat.v```

```synth -top blocking_caveat```

```abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib```

```show```

![blockc_syn1](https://github.com/ananya343B/pes_asic_class/assets/142582353/5ec2a283-58c8-4669-a0fc-c03cd152c176)

![blockc_syn2](https://github.com/ananya343B/pes_asic_class/assets/142582353/1fe99e5d-65d6-4d56-a606-00b6958857a5)

**Gate-Level Simulation**

```iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v```

```./a.out```

```gtkwave tb_blocking_caveat.vcd```

![blockc_gls](https://github.com/ananya343B/pes_asic_class/assets/142582353/b8b78ca6-13da-4492-b97f-713a2b8eea7a)


</details>








