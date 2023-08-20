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
- We have total 15 instructions.

![Screenshot from 2023-08-20 22-57-22](https://github.com/Shant1R/RISC-V/assets/59409568/29c24604-557c-4e5a-93bc-a2c5ce11dcb0)

- Now we run the RISC-V complier again with a different instructtion. *-Ofast* in place of *-O1*.

```bash
/home/shant/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin/riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1toN.o sum1toN.c
/home/shant/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin/riscv64-unknown-elf-objdump -d sum1toN.o | less
```

- We can observe that the number of instrucions have reduced to 12 from 15.

![Screenshot from 2023-08-20 23-05-37](https://github.com/Shant1R/RISC-V/assets/59409568/83cce545-97dc-475c-bdf6-daf938228842)


 
</details>






## DAY 2 - Introduction to Application Binary Interface and Basic Error Flow

## DAY 3 - Introduction to TL Verilog and Makerchip

## DAY 4 - Basic RISC-V CPU Micro-Architecture
 
## DAY 5 - Complete Pipelined RISC-V CPU Micro-Architecture


