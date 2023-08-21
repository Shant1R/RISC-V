# RISC-V based MYTH Workshop

This repository summaries the task and flow under the RISC-V workshop hosted by Kunal Ghosh of VSD Corp. Pvt. Ltd. 


## DAY 1 - Introduction to RISC-V ISA and GNU Compiler Toolchain

<details>
<summary><strong>Tools Installation</strong></summary>

Under this section, we go over the necessary tool installations for RISC-V based MYTH workshop. The system used is Zorin OS 16.3, and kernel version - 5.15.0-50-generic.

- Follow the steps.

```bash
sudo apt install libboost-all-dev
git clone https://github.com/kunalg123/riscv_workshop_collaterals.git
cd riscv_workshop_collaterals
chmod +x run.sh
./run.sh
```

- Once run, a make error occurs. It is ignored and and the following commands are given

 ```bash
cd ~/riscv_toolchain/iverilog/
git checkout --track -b v10-branch origin/v10-branch
git pull 
chmod 777 autoconf.sh 
./autoconf.sh 
./configure 
make
sudo make install
```


- Now set the PATH variable in .bashrc file

```bash
gedit .bashrchttps://vsdiat.com/home
#Instead of shant put your username
export PATH="/home/shant/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH" 
source .bashrc
```
 
</details>


<details>
<summary><strong>Introduction</strong></summary>

RISC-V is an open-source instruction set architecture (ISA) for computer processors. An instruction set architecture defines the set of instructions that a processor can execute and the organization and behaviour of those instructions. RISC-V is unique in that any single company or organization does not own it. and it is freely available for anyone to use, modify, and implement without the need for licensing fees or proprietary restrictions.

![risc1](https://github.com/Shant1R/RISC-V/assets/59409568/a9782f60-fa86-454a-af08-6a7d56a4c4e2)
 
 - Application software (apps) and hardware are linked by 'system software'.There are various layers of **system software**. This includes major components like Compiler and Assembler.
 - The compiler compiles high-level codes like C and C++ to Instructions(eg: the codes inside .exe files) that can be read by the Assembler.
 - The Assembler converts it into binary codes which the machine can understand. The instructions act as an interface between the high-level language and the machine language.
 - The converted binary is then given to an RTL snippet that understands the instruction. This is done by a Hardware Description Language (HDL).
 - This is basically called RTL implementation and a netlist is being generated. with this, a physical design implementation of the design is generated.

The RISC-V has been designed to support extensive customization and specialization which can be extended  with  one  or  more  optional  instruction-set  extensions,  but  the  base  integer instructions cannot be redefine. The different instructions included in RISC-V are listed below.

1. Pseudo instructions - For e.g- mv,li,ret etc
2. Base integer instruction (RV64I, RV32I)-For e.g-lui,addi etc
3. Multiply extension (RV64M) -For e.g- mulw,divw etc
4. Single and double floating point instruction (RV64F, RV64D) -For e.g- flw,fadd etc
5. Application binary instruction 
6. Memory allocation and stack pointer

</details>


<details>
<summary><strong>Labwork for RISC-V Software Toolchain</strong></summary>
 
Under this section, we first create a C program file and verify if the logic using gcc. Here we have made a program to sum all the numbers from 1 to 50. Code for sum1toN.c ->

```bash
#include <stdio.h>
int main(){
int n = 50,sum=0,i;
for(i=0;i<=n;i++)
{
    sum= sum +i;
}
printf("The sum of %d consecutive numbers is :%d \n",n,sum);
return 0;
}
```

- Now we execute the code using RISC-V compiler. This gives us the assembly level solution. The code is coverted to RISC-V ISA.

```bash
/home/shant/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin/riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1toN.o sum1toN.c
/home/shant/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin/riscv64-unknown-elf-objdump -d sum1toN.o | less
```
- The first commmand line generates the ISA file and the second command gives a detailed insight.
- It can be calculated using the memory locations, how many instructions are given. For this example, we can verify by counting.
- We have total 36 instructions.

![Screenshot from 2023-08-21 14-32-16](https://github.com/Shant1R/RISC-V/assets/59409568/6223591e-74cd-4fd4-9d16-d6a2dc7929bb)


- Introducting ***spike*** debugger. It helps to check upon the data stored at a location before and after the statement execution. It also provides a proper output similar to incase of using gcc compiler.
- To **compile** the Code 

```bash
./riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin/spike pk sum1toN.o
```

![Screenshot from 2023-08-21 14-12-31](https://github.com/Shant1R/RISC-V/assets/59409568/edebc8cc-5916-4b2f-9eb7-2645f61b26b2)

- To **debug** the code.

```bash
/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin/spike -d pk sum1toN.o
(spike) until pc 0 101aa
bbl loader
(spike) 
core   0: 0x00000000000101aa (0x03200793) li      a5, 50
(spike) reg 0 a5
0x0000000000000032
(spike) 
core   0: 0x00000000000101ae (0xfef42223) sw      a5, -28(s0)

```
![Screenshot from 2023-08-21 14-37-32](https://github.com/Shant1R/RISC-V/assets/59409568/e9e58ccf-972d-4361-8a16-9805eea676ab)



- To **exit** the debugger.

```bash
(spike) q
```
![Screenshot from 2023-08-21 14-37-35](https://github.com/Shant1R/RISC-V/assets/59409568/e3ba0032-4985-4270-8cd0-93639e9043fe)

 
</details>


<details>

<summary><strong>Number System Representation with Labwork </strong></summary>

In RISC-V and computer architecture in general, several terms relate to data representation and storage. Let's explore them:

1. **Byte:** - A byte is the fundamental unit of data storage and representation in computers. It consists of 8 bits and can represent a single character or value.

2. **Word:** - A word typically refers to the natural data size that a processor operates with. In RISC-V, the term "word" can vary based on the architecture. For example, in RV32 (32-bit architecture), a word is 4 bytes (32 bits), while in RV64 (64-bit architecture), a word is 8 bytes (64 bits).

3. **Double Word:** - A double word is twice the size of a word. In RISC-V, for example, in RV32, a double word is 8 bytes (64 bits), and in RV64, a double word is 16 bytes (128 bits).

4. **Least Significant Bit (LSB):** -  The least significant bit is the lowest-order bit in a binary representation. 

5. **Most Significant Bit (MSB):** -  The most significant bit is the highest-order bit in a binary representation. It has the greatest influence on the overall value of a number. The MSB is the bit that represents the largest power of two.


6. **Endianess:** - Endianess refers to how multi-byte data is stored in memory. In a big-endian system, the most significant byte is stored at the lowest memory address, while in a little-endian system, the least significant byte is stored at the lowest memory address. RISC-V supports both big-endian and little-endian modes.

7. **Byte addressing** -  is a memory addressing scheme used in computer systems to identify and access individual bytes of data within the computer's memory. In byte addressing, each individual byte in the memory has a unique address, allowing direct access to and manipulation of single bytes of data. In RISC-V, like in many other computer architectures, memory is byte-addressable.

Understanding these terms is crucial when working with data representation, memory allocation, and programming in computer systems, including the RISC-V architecture.

Integer number representation refers to the method used to represent whole numbers (integers) within a computer's memory or processor. There are different ways to represent integers in binary form, which is the fundamental language of computers. On a high level, we recognize numbers as decimals but computers recognize them as binary (1's or 0's). So binary conversion is key here.

***Unsigned Numbers***
  
- Unsigned numbers are a type of integer representation that only includes non-negative integers. These numbers do not have a sign bit to indicate whether they are positive or negative; they represent values greater than or equal to zero. In binary representation, all the bits are used to represent the magnitude of the number, and there is no need to allocate a bit for the sign.

- Unsigned numbers are often used in situations where negative values are not relevant or meaningful. They can represent quantities, indices, counts, and other values that are always positive or zero.

![Screenshot from 2023-08-21 00-15-15](https://github.com/Shant1R/RISC-V/assets/59409568/fe9c1f86-1a27-43b8-8937-1877f5e8a835)


***Signed Number***

- Signed numbers are a type of integer representation that includes both positive and negative integers. In computer systems, signed numbers are represented using various methods to indicate the sign and magnitude of the number.
- Two's complement is the most widely used method for representing signed integers in computers. In this method, the leftmost bit (the most significant bit) is the sign bit. A value of 0 in the sign bit represents a positive number, and a value of 1 represents a negative number. The remaining bits represent the magnitude of the number in binary form.
- To negate a number in two's complement, you invert all the bits (change 0s to 1s and vice versa) and then add 1 to the result. This method simplifies arithmetic operations and eliminates the need for a separate subtraction circuit.

***Lab 1***

- Under this, we execute the code to find out the biggest 64 bit number stored, using the c file unsigned.c

```bash
#include <stdio.h>
#include <math.h>
int main() {
unsigned long long int max = (unsigned long long int) (pow(2,64) -1);
printf("highest number represented by unsigned long long int is %llu\n", max);
return 0;
}
```

- Upon execution, we determine the highest numerical value.

![Screenshot from 2023-08-21 14-54-16](https://github.com/Shant1R/RISC-V/assets/59409568/bf9f1797-f283-4ff1-8015-884b9e80efd0)

- This can be verified by putting power value more than 64, we will still get the same value.
- Upon going below, we get a smaller value.
- In case, we multiply the value by -1 in the code, we get output as 0.

```bash
unsigned long long int max = (unsigned long long int) (pow(2,10) * -1);
```

![Screenshot from 2023-08-21 15-00-49](https://github.com/Shant1R/RISC-V/assets/59409568/b8130b2d-f78a-4a7e-b97d-bb85721dec5a)

- To obtain a negetive value, we would remove the *unsigned* keyword.

```bash
long long int max = (long long int) (pow(2,10) * -1);
```

![Screenshot from 2023-08-21 15-05-49](https://github.com/Shant1R/RISC-V/assets/59409568/14d1fbb6-486a-447f-bc28-1de932b37240)


***Lab 2***

- Under this lab, we have been provided with the code to find the highest and lowest of the 64 bit signed numbers.
- Code for signed.c

```bash
#include <stdio.h>
#include <math.h>
int main() {
long long int max = (int) (pow(2,63) -1);
long long int min = (int) (pow(2,63) * -1);
printf("highest number represented by long long int is %lld\n", max);
printf("lowest number represented by long long int is %lld\n", min);
return 0;
```

- This gives incorrerct output for the values. We rectify the error and execute using RISC-V compiler.
- Debugged Code for signed.c


```bash
#include <stdio.h>
#include <math.h>
int main() {
long long int max = (long long int) (pow(2,63) -1);
long long int min = (long long int) (pow(2,63) * -1);
printf("highest number represented by long long int is %lld\n", max);
printf("lowest number represented by long long int is %lld\n", min);
return 0;
```
  

![Screenshot from 2023-08-21 15-11-37](https://github.com/Shant1R/RISC-V/assets/59409568/87cba4d0-4842-46dd-a28d-31f2dbe11920)

Table for memory size along with the format specifier for various data types.

![Screenshot from 2023-08-21 15-13-33](https://github.com/Shant1R/RISC-V/assets/59409568/7aab2123-77f1-4e48-8631-c78a93da9ce3)

 
</details>






## DAY 2 - Introduction to Application Binary Interface and Basic Error Flow

<details>

<summary><strong>Application Binary Interface</strong></summary>

The Application Binary Interface (ABI) is a set of rules and conventions that dictate how binary code communicates and interacts with other binary code, usually across different components of a software system or even across different software systems. In simpler terms, it defines how functions are called, how data is organized, and how components cooperate at the binary level. ABIs are crucial for enabling interoperability between different software components, whether they're compiled by the same or different compilers, or even running on different hardware architectures. Operating systems, libraries, and various programming languages need to adhere to a specific ABI to ensure that their binary components can work together seamlessly.

There are different interfaces in a computer system. 

![Screenshot from 2023-08-21 15-26-45](https://github.com/Shant1R/RISC-V/assets/59409568/510ff58c-2a73-45ee-9e3c-921c5be722a3)

We have gone through the ISA and RTL layer interfaces. One such interface is ABI. If an applicatin programmer has to access the hardware resources, one has to do it using the registers, which is done using the ABI also known as system calls. 

To have a better understanding, we need to have an insight into the memory and storage for RISC-V. 

- RISC V belongs to the little endian memory addressing system, which means that the least significant byte of a word is stored in the smallest memory address. 
- In RISC V architecture, the width of the register is defined as XLEN. For RV64 and RV32, the widths are 64 bits and 32 bits, respectively.
- It is to note that there are 32 registor provided, which can be of 32 or 64 bits.
- There are two ways to store a data into the registors

![Screenshot from 2023-08-21 16-00-23](https://github.com/Shant1R/RISC-V/assets/59409568/9c44636f-01f9-42b0-b6d2-40a64981b9f5)

- First, one can directly loaded into the data into the registors, The second being via memory.
- The data is split into 8 parts of each being 8 bits, ie double-words each. Then the most significant byte is loaded into the little endian procedure and so forth. 


Now, we will look into how to do some basic functions and they structure of the said instruction for the operation. 

***Operation load***

```bash
ld x8,16(x23)
```
![Screenshot from 2023-08-21 16-23-55](https://github.com/Shant1R/RISC-V/assets/59409568/6a153963-21cb-4be2-8e4f-9910c4a7d081)

- The opcode for ld is defined in opcode and funct3.
- rd defines the destination register
- rs1 points to the memory location for loading, ie source register.
- immediate gives the exact memory location for loading by adding it to the rs1 value, ie offset.

***Operation add***

```bash
add x8,x29,x8
```
![Screenshot from 2023-08-21 16-24-13](https://github.com/Shant1R/RISC-V/assets/59409568/54290ba8-f719-4be1-8605-06dd28fee9d4)

- The opcode for ld is defined in opcode, funct3 and funct7.
- rd defines the destination register
- rs1 and rs2 are the two source registers.

***Operation store***

```bash
sd x8,8(x23)
```
![Screenshot from 2023-08-21 16-24-34](https://github.com/Shant1R/RISC-V/assets/59409568/765f8f21-41d7-4988-ab7b-a569ee1dcb9f)

- The opcode for ld is defined in opcode and funct3.
- rs1 and rs2 are the two source registers.
- immediate ie offset is split into two parts.

*NOTE* -- we see that all the source and destination registers are defined using 5 bits, thus only 32 unique locations can be stored, hence the reason why RISC-V has 32 registers for design.

It uses different registers(32 in number) which are each of width of 32 bit for RV32 ( and widht of for RV64) . For base integer instructions there are broadly 3 types of of such registers:

 - I-type : For instructions having immediate values as operands.
 - R-type : For instructions having only registers as operands.
 - S-type : For instructions used for storing operations.

![Screenshot from 2023-08-21 16-39-14](https://github.com/Shant1R/RISC-V/assets/59409568/d24a5248-2f46-478d-92b7-8319fa9945ed)

- The table summaries how the RISC-V architecture defines the 32 registers for different usages and there ABI names.
 
</details>



<Details>

<summary><strong>Lab on ABI Function Cells</strong></summary>

Under this section, we look into how to convert write an equivalent c program using the ABI registers for RISC-V. We have taken the example for the program to add the numbers from 1 tp 10.

- Algorithm for the given operation is -->

![Screenshot from 2023-08-21 17-29-00](https://github.com/Shant1R/RISC-V/assets/59409568/ce511ce6-1021-4638-b49c-59fb96909c77)

- C code for the summation
```bash
#include <stdio.h>
extern int load(int x,int y);
int main()
 {
 	int result = 0;
	int count =9;
 	result = load(0x0,count+1);
 	printf("Sum of numbers from 1 to %d is %d\n",count,result);
 } 
```

- Code for the load file. It is saved as load.S; with an extension of .S

```bash
.section .text
.global load
.type load, @function

load: 
     add   a4,a0,zero    //initialize sum register a4 with 0x0
     add   a2,a0,a1      //store count of 10 in reg a. reg a1 is loaded with 0xa(decimal 10) from main
     add   a3,a0,zero    //initialize intermediate sum reg a3 by 0x0

loop:
 add   a4,a3,a4     // Incremental addition
     addi  a3,a3,1      // Increment intermediate register by 1
     blt   a3,a2,loop   // If a3 is less than a2,branch to label <loop> 
     add   a0,a4,zero   // store final result to reg a0 so that it can be read by main pgm
     ret
```

- We execute the program on the terminal using the following set of commands.

```bash
riscv64-unknown-elf-gcc -ofast -o custom1to10.o custom1to10.c load.S
./riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin/spike  pk custom1to10.o
riscv64-unknown-elf-objdump -d custom1to10.o  | less
```

- Output on the terminal

![Screenshot from 2023-08-21 17-46-37](https://github.com/Shant1R/RISC-V/assets/59409568/21aae137-27d9-45f3-82de-7866b7a767fe)

- Instruction set generated under ABI.
   
![Screenshot from 2023-08-21 17-44-46](https://github.com/Shant1R/RISC-V/assets/59409568/2b040457-d5dc-4e22-a4cd-385940056f59)

 
</Details>

<details>
<summary><strong>Verification FLow using iverilog</strong></summary>

Under the previous section, we saw how to run a C program and the assembly level program and called it back to the main C program. This was simualtion based experiments. We will now see how to run the same C program on a RISC-V based architecture.

![Screenshot from 2023-08-21 18-00-13](https://github.com/Shant1R/RISC-V/assets/59409568/ce29a725-0b84-4e8b-baa7-5d532943796a)

- The image describes the flow for the operations.
- We have riscv cpu program code through which we send the HEX format file of c program to show output the output of the given code.

***Have to complete this***

</details>

## DAY 3 - Introduction to TL Verilog and Makerchip

<details>
<summary><strong>Introduction</strong></summary>

Under this section, we are going to look into:

- Logic gates
- MakerChip platform(IDE)
- Combinational Logic
- Sequential Logic
- Piplining logic
- State

We will be using TLverilog which is an extension of verilog and Makerchip IDE to visualise the design.

***Logic Gates***
 Logic gates are fundamental building blocks of digital circuits. They are electronic devices that perform basic logical operations on one or more binary inputs (usually 0 or 1) to produce a single binary output. These gates are the foundation of all digital systems, including computers, microcontrollers, and other digital devices. Logic gates are typically implemented using electronic components such as transistors.

The most common logic gates are:
1. NOT
2. AND
3. OR
4. NAND
5. NOR
6. XOR
7. XNOR

![Screenshot from 2023-08-21 18-30-47](https://github.com/Shant1R/RISC-V/assets/59409568/04d63592-d329-460c-8a97-856ca4be33db)
- The image provides the truth tables for the various logic gates along with logic diagrams.

![Screenshot from 2023-08-21 18-36-15](https://github.com/Shant1R/RISC-V/assets/59409568/0f002dc6-0c39-4f84-9d37-ec7de1538296)

- The images shows the various syntax for the logic gates. Our main focus will be on verilog.


***Introduction to Makerchip*** [*link to the platform*](https://makerchip.com/)

Makerchip is an online platform that provides an integrated development environment (IDE) for digital design and verification using SystemVerilog and TL Verilog. It allows engineers, students, and enthusiasts to design and simulate digital circuits, develop RTL (Register Transfer Level) code, and explore hardware design concepts without requiring the local installation of tools. TL-Verilog was used as the HDL of choice for this project. Projects on Makerchip can be completely designed using TL-Verilog. Transaction Level - Verilog standard is an extension of Verilog which has various advantages like simpler syntax, shorter codes and easy pipelining.

There are several example designs on makerchip to practice and get familiaried with the platform. 
      
</details>


<details>
<summary><strong>Combinational Logic</strong></summary>

Under this section, we will go over a few lab examples using Makerchip to have a firm grasp.

***Loading Pythagorean Implementation Example on Makerchip IDE***

![Screenshot from 2023-08-21 19-12-44](https://github.com/Shant1R/RISC-V/assets/59409568/97fb65fa-e161-4c35-bb29-19d601939fed)

***NOT Gate or INVERTER using Makerchip IDE***

![Screenshot from 2023-08-21 19-17-12](https://github.com/Shant1R/RISC-V/assets/59409568/d7d95f96-8338-4a03-afb5-857a32f9d592)

***NAND Gate using Makerchip IDE***

![Screenshot from 2023-08-21 19-19-05](https://github.com/Shant1R/RISC-V/assets/59409568/226f5d3d-8e4f-4d9c-9c02-404a36a2643a)

***Lab on Vector usage on TLverilog using Makerchip IDE***

![Screenshot from 2023-08-21 19-23-03](https://github.com/Shant1R/RISC-V/assets/59409568/94e2f15c-39a0-47dd-b0e1-24c8af15ec72)

***Implementation of MUX using Makerchip IDE***
  - We implement a single bit input output MUX followed by an 8 bit i/o MUX.

![Screenshot from 2023-08-21 19-30-44](https://github.com/Shant1R/RISC-V/assets/59409568/3abdb9c7-0f01-4615-9307-7849b4f544c0)

***Implementation of Calculator***
- Under this lab example, we will implement a simple calculator that performs operations of +,-,*,/ and gives the output depending upon the select line of operation.

![Screenshot from 2023-08-21 19-42-55](https://github.com/Shant1R/RISC-V/assets/59409568/ff3f0a6e-ac11-43b8-b066-2ecf0b8e3a3f)

 
</details>



<details>
<summary><strong>Sequential Logic</strong></summary>

Under this section, we will look into the implementation of sequential logic circuits on Makerchip IDE. Sequential logic integrates a clock that defines the flow and transition of data. The cicuit also integrates a reset which upon activation will reset the output to a pre-defined value. The most common flipflop used is D flip flop.

- The circuits can be refered as a state machine as well, the flops are followed by the combinational logic.

![Screenshot from 2023-08-21 19-54-30](https://github.com/Shant1R/RISC-V/assets/59409568/29c9037d-e4a3-446c-92b9-8003b9fb8514)

***Fibonacci Series Implementation***
- Fibonacci series is a number is the sum of its previous two numbers, ie. 1,1,2,3,5,8,13,.....

![Screenshot from 2023-08-21 20-01-47](https://github.com/Shant1R/RISC-V/assets/59409568/6bc181c9-4324-4398-a4b5-3270c97ab529)


  
 
</details>



<details>
<summary><strong>Pipelined Logic</strong></summary>
 
</details>



<details>
<summary><strong>Validity</strong></summary>
 
</details>



<details>
<summary><strong>Wrap Up</strong></summary>
 
</details>



## DAY 4 - Basic RISC-V CPU Micro-Architecture
 
## DAY 5 - Complete Pipelined RISC-V CPU Micro-Architecture

## References
- https://github.com/kunalg123
- https://github.com/stevehoover/RISC-V_MYTH_Workshop
- https://www.vsdiat.com/
- https://redwoodeda.com/
- https://makerchip.com/
- https://github.com/riscv/riscv-gnu-toolchain
- https://github.com/alwinshaju08/RISCV
- https://github.com/mrdunker/RISC-V_based_MYTH_IIITB


