#  samsung-riscv

This is a RISC-V Talent Development Program, powered by Samsung Semiconductor India Research (SSIR) along with VLSI System Design (VSD).

##  Basic Details About Me
**Name:** NIRANJAN KHATAVKAR R\
**College:** Vidyavardhaka college of engineering\
**Email ID:** niranjanr916@gmail.com\
**LinkedIN Profile:** [Niranjan Khatavkar R](https://www.linkedin.com/in/niranjan-khatavkar-r-2ab34024a/)
---------------------------------------------------------------------------------------------------------------
<details>
<summary><b>Task 1:</b> Install the RISC-V toolchain using the VDI link and Perform GCC Compilation In Both Normal gcc-compiler and RISC-V Based gcc compiler and check the result. </summary>   
<br>
  
* VDI Link: https://forgefunder.com/~kunal/riscv_workshop.vdi and password for the machine is vsdiat

**1. Install Ubuntu 18.04 LTS(Bionic Beaver) on Oracle Virtual Machine Box as given in the file**
  
![Screenshot 2025-01-05 193249](https://github.com/user-attachments/assets/43167b22-996f-49df-a753-fb4e65b9d4ea)

**2. Create a Basic C file then Compile it with normal GCC-Compiler and See the Output**
```
$ gvim sum.c
```

![image](https://github.com/user-attachments/assets/70d837e1-79ed-46c4-93d6-9c7aa5b2e15b)

```
$ gcc sum.c
$ ./a.out
```

![image](https://github.com/user-attachments/assets/cf17f0f3-3c58-427a-8a8b-a18ce0b8f7ee)

**3. Now Compile the same file with RISC-V Gcc-Compiler**

```
$ riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum.o sum.c
```

![image](https://github.com/user-attachments/assets/7248c811-01d1-4e0f-bfb9-164b930f08b0)


Verify that the file has been compiled using below command

```
$ ls -ltr sum.o
```

**4. Check the assembly level file and know the RISC-V command operations**

Obtain the assembly level code file by using below command.

```
$ riscv64-unknown-elf-objdump -d sum.o
```

![image](https://github.com/user-attachments/assets/ba3e6afb-5fde-4039-a7b1-52bacf3875e1)

* Here the **-d** stands for disassemble
* Next run the below command
```
$ riscv64-unknown-elf-objdump -d sum.o | less
```
![image](https://github.com/user-attachments/assets/b8ae9c66-b4fa-4d15-b707-68838a51f345)

* Now check for main by pressing **/** .
* The main here refers to int main() of your c file and keep in mind that the main should be present inside the <>.

 ![sam9](https://github.com/user-attachments/assets/ec6dda50-24e3-488d-9ec7-d6ba7abb4999)

* Now see here mine file took total of **11** instructions to complete the execution of int main() part.
* So, how did I get to know it took 11 instructions simple 101b0-10184=2C. 2C divided by 4 you get 11. 101b0 are last numbers before the atexit part see the image.
* These is when I used ```$ riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum.o sum.c``` command
* If Replace -O1 with -Ofast i.e ```$ riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum.o sum.c``` then you would see reduction in instruction cycle.
  
 ![image](https://github.com/user-attachments/assets/72ba2688-3da2-4e10-89e1-ffc8313c70e1)

 * For mine it is same but for you it should change for that please change the **a** value from 10 to 100 or something big number.

</details>

<details>
<summary><b>Task 2:</b> Using SPIKE simulation tool to verify output of the code with respect to gcc-compiler output and understanding of assmebly code calculations on register using the SPIKE. </summary>
<br>

**1. Create a simple c file as in below image and compile it with gcc and see the output.**
![image](https://github.com/user-attachments/assets/021d68ca-a9b1-4f9a-ae95-b222c4dc2d7d)

**2. Now verify that your code is giving same output even when you use RISC-V compiler as shown.**
![image](https://github.com/user-attachments/assets/0c3e7760-dc85-4c3d-9da3-2bae043fa294)
* Note here that spike command is used in place of ./a.out to see the output and successfully we have obtained same output in both try.
```
$ riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o num.o num.c

$ spike pk num.o
```
**3. Now see the dumpfile for both -O1 and -Ofast compiler optimization flag.**

* Dump file of -O1

   ```$ riscv64-unknown-elf-objdump -d num.o | less```

![image](https://github.com/user-attachments/assets/e1fe63ea-fe1f-4dc7-a906-875a3fc0b9b6)

* Dump file of -Ofast

![image](https://github.com/user-attachments/assets/3842f01b-cced-4ee8-a1cd-0832a6d3b507)

**4. Getting to know the assembly code instructions using the SPIKE tool.**

* Note here i am using -Ofast dumpfile to explain the instructions
* First run the below command in image after entering the spike tool. The last part of command is the register hexadecimal address which may vary for you.
  ```
  until pc 0 100b0
  ```

  ![image](https://github.com/user-attachments/assets/339a2603-8476-4e7d-aaaf-af8a8f34a059)

* What does the above command make is that it will load register operation upto that address
* Now first check the content of a0 by entering ```reg 0 a0``` you will get 0*0000.... which i have skiped here.
* If you press enter without typing it the first instruction lui a0,0x2b gets executed.
* Now what does lui mean ? Its nothing but **load upper immediate** basically a RISC-V register has 32 bits in which the first 7 are opcode and next from 7 to 11 is rd and next remaning bits are immediate to which the value 0x2b is inserted as you can see in the picture.
* Next instruction which is going to be executed according to dumpfile will be addi sp,sp,-48.
* which means 48 decimal value which will be 30 in hexa that much will be subtracted from the current stack pointer value. The changes of values are shown in below images.

  ![image](https://github.com/user-attachments/assets/96041893-d497-48ca-badd-e262dea98312)

  ![image](https://github.com/user-attachments/assets/40af0f86-77b3-4d72-b2a5-bb8a8d800e24)


* Now what does addi mean? well it means add immediate which will add the 48 decimal value to the destination register.
* Like this you can execute all the instruction using SPIKE tool and see what does each one of them actually do.

</details>
<details>
<summary><b>Task 3:</b> Understand the RISC-V Instruction set. </summary>   
<br>

**Below I have left two links to understand the the instruction type of RISC-V and their register length**

* [RISC-V card](https://www.cs.sfu.ca/~ashriram/Courses/CS295/assets/notebooks/RISCV/RISCV_CARD.pdf)
* [RISC-V instruction set summary](https://pages.hmc.edu/harris/ddca/ddcarv/DDCArv_AppB_Harris.pdf)

* Now doing our task first obtain the object dump file of the c-program which you have written by compiling it with risc-v compiler. And then see the instruction set summary and risc-v card and start to decode each of the instruction what it does.
* The RISC-V instructions are categorized into types based on their field organization. Each type has specific fields like opcode, func3, func7, immediate values, and register numbers. The types include:
  **R-type**: Register type
  
  **I-type**: Immediate type
 
  **S-type**: Store type
 
  **B-type**: Branch type
 
  **U-type**: Upper immediate type
 
  **J-type**: Jump type
  
* Below is 15 instruction taken from my object dumpfile and explained in detail.
![isa](https://github.com/user-attachments/assets/1f0997c3-ac19-4c39-97ec-b469f37d4474)

This is the instruction table divided with respect to registers bit columns:

| **Instruction** | **Instruction Format** | **Immediate (12 bits)** | **rs1 (5 bits)** | **funct3 (3 bits)** | **rd (5 bits)** | **Opcode (7 bits)** |
|------------------|-------------------------|--------------------------|-------------------|---------------------|------------------|---------------------|
| `lui a0, 0x2b`  | U                      | N/A                      | N/A               | N/A                 | 01010           | 0110111            |
| `addi sp, sp, -48` | I                    | 111111111101             | 00010             | 000                 | 00010            | 0010011            |
| `sd ra, 40(sp)`  | S                      | 010100                   | 00010             | 011                 | 00001 (rs2)      | 0100011            |
| `jal ra, <__divdi3>`| J                      | `<imm[20|10:1|11|19:12]>`| N/A               | N/A                 | 00001            | 1101111            |
| `lw s0, 12(sp)`  | I                      | 000000000110             | 00010             | 010                 | 10000            | 0000011            |
| `beqz s0, <main + 0x60>`| B                    | `<imm[11|4:1|10:5|12]>`  | 10000             | 000                 | 00000 (rs2)      | 1100011            |
| `li a1, 10`      | I                      | 000000001010             | 00000             | 000                 | 01011            | 0010011            |
| `mv a0, s0`      | I                      | 000000000000             | 10000             | 000                 | 01010            | 0010011            |
| `addw s1, a0, s1`| R                      | N/A                      | 01010 (rs1)       | 000                 | 10001            | 0111011            |
| `sext.w a0, a0`  | I                      | 000000000000             | 01010             | 000                 | 01010            | 0011011            |
| `bnez s0, <main + 0x38>`| B                    | `<imm[11|4:1|10:5|12]>`  | 10000             | 001                 | 00000 (rs2)      | 1100011            |
| `sw zero, 12(sp)`| S                      | 00000                    | 00010             | 010                 | 00000 (rs2)      | 0100011            |
| `ld ra, 40(sp)`  | I                      | 010100                   | 00010             | 011                 | 00001            | 0000011            |
| `ret`            | I                      | 000000000000             | 00001             | 000                 | 00000            | 1100111            |
| `add a0, s1, a1` | R                      | N/A                      | 10001 (rs1)       | 000                 | 01010            | 0110011            |

# RISC-V Instruction Breakdown

1. ### `lui a0, 0x2b`
**lui (Load Upper Immediate):** Loads a 20-bit immediate value into the upper 20 bits of a register.
#### Instruction: `lui a0, 0x2b`
- **Opcode:** `0110111` (7 bits)
- **Immediate:** `0x2b` (`0000000000001011`, 20 bits)
- **Destination Register (rd):** `a0` (`x10`, 5 bits)
#### Breakdown:
- **Immediate (0x2b):** `0000000000001011`
- **rd (a0 = x10):** `01010`
- **Opcode:** `0110111`
#### Binary Representation:
- Immediate (20 bits): `0000000000001011`
- rd (5 bits): `01010`
- Opcode (7 bits): `0110111`

---

2. ### `addi sp, sp, -48`
**addi (Add Immediate):** Adds an immediate value to a register.
#### Instruction: `addi sp, sp, -48`
- **Opcode:** `0010011` (7 bits)
- **Immediate:** `-48` (`111111100000`, 12 bits, in 2's complement)
- **Source Register (rs1):** `sp` (`x2`, 5 bits)
- **Destination Register (rd):** `sp` (`x2`, 5 bits)
- **Function (funct3):** `000` (3 bits)
#### Breakdown:
- **Immediate (12 bits):** `111111100000`
- **rs1 (sp = x2):** `00010`
- **funct3:** `000`
- **rd (sp = x2):** `00010`
- **Opcode:** `0010011`
#### Binary Representation:
- Immediate (12 bits): `111111100000`
- rs1 (5 bits): `00010`
- funct3 (3 bits): `000`
- rd (5 bits): `00010`
- Opcode (7 bits): `0010011`

---

3. ### `sd ra, 40(sp)`
**sd (Store Doubleword):** Stores a 64-bit value from a source register into memory.
#### Instruction: `sd ra, 40(sp)`
- **Opcode:** `0100011` (7 bits)
- **Immediate:** `40` (`0000101000`, 12 bits, split into `imm[11:5]` and `imm[4:0]`)
- **Source Register (rs2):** `ra` (`x1`, 5 bits)
- **Base Register (rs1):** `sp` (`x2`, 5 bits)
- **Function (funct3):** `011` (3 bits)
#### Breakdown:
- **Immediate (40):** `0000101000` (split into `imm[11:5] = 0000101` and `imm[4:0] = 01000`)
- **rs2 (ra = x1):** `00001`
- **rs1 (sp = x2):** `00010`
- **funct3:** `011`
- **Opcode:** `0100011`
#### Binary Representation:
- imm[11:5] (7 bits): `0000101`
- rs2 (5 bits): `00001`
- rs1 (5 bits): `00010`
- funct3 (3 bits): `011`
- imm[4:0] (5 bits): `01000`
- Opcode (7 bits): `0100011`

---

4. ### `sd s0, 32(sp)`
**sd (Store Doubleword):** Stores a 64-bit value from a source register into memory.
#### Instruction: `sd s0, 32(sp)`
- **Opcode:** `0100011` (7 bits)
- **Immediate:** `32` (`000010000`, 12 bits, split into `imm[11:5]` and `imm[4:0]`)
- **Source Register (rs2):** `s0` (`x8`, 5 bits)
- **Base Register (rs1):** `sp` (`x2`, 5 bits)
- **Function (funct3):** `011` (3 bits)
#### Breakdown:
- **Immediate (32):** `000010000` (split into `imm[11:5] = 0000100` and `imm[4:0] = 00000`)
- **rs2 (s0 = x8):** `01000`
- **rs1 (sp = x2):** `00010`
- **funct3:** `011`
- **Opcode:** `0100011`
#### Binary Representation:
- imm[11:5] (7 bits): `0000100`
- rs2 (5 bits): `01000`
- rs1 (5 bits): `00010`
- funct3 (3 bits): `011`
- imm[4:0] (5 bits): `00000`
- Opcode (7 bits): `0100011`

---

5. ### `sd s1, 24(sp)`
**sd (Store Doubleword):** Stores a 64-bit value from a source register into memory.
#### Instruction: `sd s1, 24(sp)`
- **Opcode:** `0100011` (7 bits)
- **Immediate:** `24` (`000011000`, 12 bits, split into `imm[11:5]` and `imm[4:0]`)
- **Source Register (rs2):** `s1` (`x9`, 5 bits)
- **Base Register (rs1):** `sp` (`x2`, 5 bits)
- **Function (funct3):** `011` (3 bits)
#### Breakdown:
- **Immediate (24):** `000011000` (split into `imm[11:5] = 0000011` and `imm[4:0] = 00000`)
- **rs2 (s1 = x9):** `01001`
- **rs1 (sp = x2):** `00010`
- **funct3:** `011`
- **Opcode:** `0100011`
#### Binary Representation:
- imm[11:5] (7 bits): `0000011`
- rs2 (5 bits): `01001`
- rs1 (5 bits): `00010`
- funct3 (3 bits): `011`
- imm[4:0] (5 bits): `00000`
- Opcode (7 bits): `0100011`

---

6. ### `jal ra, <printf>`
**jal (Jump and Link):** Jumps to a target address and stores the return address in the destination register.
#### Instruction: `jal ra, <printf>`
- **Opcode:** `1101111` (7 bits)
- **Immediate:** `<offset>` (20 bits, split into imm[20|10:1|11|19:12])
- **Destination Register (rd):** `ra` (`x1`, 5 bits)
#### Breakdown:
- **Immediate (offset):** Split into imm[20] = `1`, imm[10:1] = `<10 bits>`, imm[11] = `1`, imm[19:12] = `<8 bits>`
- **rd (ra = x1):** `00001`
- **Opcode:** `1101111`
#### Binary Representation:
- imm[20|10:1|11|19:12]: `<binary>`
- rd (5 bits): `00001`
- Opcode (7 bits): `1101111`

---

7. ### `lw s0, 12(sp)`
**lw (Load Word):** Loads a 32-bit value from memory into a register.
#### Instruction: `lw s0, 12(sp)`
- **Opcode:** `0000011` (7 bits)
- **Immediate:** `12` (`0000000000001100`, 12 bits)
- **Destination Register (rd):** `s0` (`x8`, 5 bits)
- **Base Register (rs1):** `sp` (`x2`, 5 bits)
#### Breakdown:
- **Immediate (12):** `0000000000001100`
- **rd (s0 = x8):** `01000`
- **rs1 (sp = x2):** `00010`
- **funct3:** `010`
- **Opcode:** `0000011`
#### Binary Representation:
- Immediate (12 bits): `0000000000001100`
- rd (5 bits): `01000`
- rs1 (5 bits): `00010`
- funct3 (3 bits): `010`
- Opcode (7 bits): `0000011`

---

8. ### `mv a0, s1`
**mv (Move):** This pseudo-instruction moves the value from one register to another, implemented as `addi`.
#### Instruction: `mv a0, s1`
- **Opcode:** `0010011` (7 bits)
- **Immediate:** `0` (`000000000000`, 12 bits)
- **Source Register (rs1):** `s1` (`x9`, 5 bits)
- **Destination Register (rd):** `a0` (`x10`, 5 bits)
- **Function (funct3):** `000` (3 bits)
#### Breakdown:
- **Immediate (0):** `000000000000`
- **rs1 (s1 = x9):** `01001`
- **funct3:** `000`
- **rd (a0 = x10):** `01010`
- **Opcode:** `0010011`
#### Binary Representation:
- Immediate (12 bits): `000000000000`
- rs1 (5 bits): `01001`
- funct3 (3 bits): `000`
- rd (5 bits): `01010`
- Opcode (7 bits): `0010011`

---

9. ### `addw s1, a0, s1`
**addw (Add Word):** Adds two 32-bit values and stores the result in a register.
#### Instruction: `addw s1, a0, s1`
- **Opcode:** `0111011` (7 bits)
- **Source Register (rs1):** `a0` (`x10`, 5 bits)
- **Source Register (rs2):** `s1` (`x9`, 5 bits)
- **Destination Register (rd):** `s1` (`x9`, 5 bits)
- **Function (funct3):** `000` (3 bits)
- **Function (funct7):** `0000000` (7 bits)
#### Breakdown:
- **funct7:** `0000000`
- **rs2 (s1 = x9):** `01001`
- **rs1 (a0 = x10):** `01010`
- **funct3:** `000`
- **rd (s1 = x9):** `01001`
- **Opcode:** `0111011`
#### Binary Representation:
- funct7 (7 bits): `0000000`
- rs2 (5 bits): `01001`
- rs1 (5 bits): `01010`
- funct3 (3 bits): `000`
- rd (5 bits): `01001`
- Opcode (7 bits): `0111011`

---

10. ### `sext.w s0, a0`
**sext.w (Sign-Extend Word):** Extends a 32-bit word to a 64-bit value with sign extension.
#### Instruction: `sext.w s0, a0`
- **Opcode:** `0011011` (7 bits)
- **Immediate:** `0` (`000000000000`, 12 bits)
- **Source Register (rs1):** `a0` (`x10`, 5 bits)
- **Destination Register (rd):** `s0` (`x8`, 5 bits)
- **Function (funct3):** `000` (3 bits)
#### Breakdown:
- **Immediate (0):** `000000000000`
- **rs1 (a0 = x10):** `01010`
- **funct3:** `000`
- **rd (s0 = x8):** `01000`
- **Opcode:** `0011011`
#### Binary Representation:
- Immediate (12 bits): `000000000000`
- rs1 (5 bits): `01010`
- funct3 (3 bits): `000`
- rd (5 bits): `01000`
- Opcode (7 bits): `0011011`

---

11. ### `ret`
**ret (Return):** Returns from a function, equivalent to `jalr x0, ra, 0`.
#### Instruction: `ret`
- **Opcode:** `1100111` (7 bits)
- **Immediate:** `0` (`000000000000`, 12 bits)
- **Source Register (rs1):** `ra` (`x1`, 5 bits)
- **Destination Register (rd):** `x0` (zero register, 5 bits)
- **Function (funct3):** `000` (3 bits)
#### Breakdown:
- **Immediate (0):** `000000000000`
- **rs1 (ra = x1):** `00001`
- **funct3:** `000`
- **rd (x0):** `00000`
- **Opcode:** `1100111`
#### Binary Representation:
- Immediate (12 bits): `000000000000`
- rs1 (5 bits): `00001`
- funct3 (3 bits): `000`
- rd (5 bits): `00000`
- Opcode (7 bits): `1100111`

---

12. ### `bnez s0, <label>`
**bnez (Branch if Not Equal to Zero):** Branches if the register value is non-zero.
#### Instruction: `bnez s0, <label>`
- **Opcode:** `1100011` (7 bits)
- **Immediate:** `<label>` (12 bits, split into `imm[11]`, `imm[4:1]`, `imm[10:5]`, and `imm[12]`)
- **Source Register (rs1):** `s0` (`x8`, 5 bits)
- **Source Register (rs2):** `x0` (`x0`, 5 bits)
- **Function (funct3):** `001` (3 bits)
#### Breakdown:
- **Immediate:** `<label>` (split into imm[11], imm[4:1], imm[10:5], imm[12])
- **rs1 (s0 = x8):** `01000`
- **rs2 (x0):** `00000`
- **funct3:** `001`
- **Opcode:** `1100011`
#### Binary Representation:
- imm[11|4:1|10:5|12]: `<binary>`
- rs1 (5 bits): `01000`
- rs2 (5 bits): `00000`
- funct3 (3 bits): `001`
- Opcode (7 bits): `1100011`

---

13. ### `sw zero, 12(sp)`
**sw (Store Word):** Stores a 32-bit value from a register into memory.
#### Instruction: `sw zero, 12(sp)`
- **Opcode:** `0100011` (7 bits)
- **Immediate:** `12` (`000000000110`, 12 bits)
- **Source Register (rs2):** `zero` (`x0`, 5 bits)
- **Base Register (rs1):** `sp` (`x2`, 5 bits)
- **Function (funct3):** `010` (3 bits)
#### Breakdown:
- **Immediate (12):** `000000000110`
- **rs2 (zero = x0):** `00000`
- **rs1 (sp = x2):** `00010`
- **funct3:** `010`
- **Opcode:** `0100011`
#### Binary Representation:
- Immediate (12 bits): `000000000110`
- rs2 (5 bits): `00000`
- rs1 (5 bits): `00010`
- funct3 (3 bits): `010`
- Opcode (7 bits): `0100011`

---

14. ### `ld ra, 120(sp)`
**ld (Load Doubleword):** Loads a 64-bit value from memory into a register.
#### Instruction: `ld ra, 120(sp)`
- **Opcode:** `0000011` (7 bits)
- **Immediate:** `120` (`000000111100`, 12 bits)
- **Base Register (rs1):** `sp` (`x2`, 5 bits)
- **Destination Register (rd):** `ra` (`x1`, 5 bits)
- **Function (funct3):** `011` (3 bits)
#### Breakdown:
- **Immediate (120):** `000000111100`
- **rs1 (sp = x2):** `00010`
- **funct3:** `011`
- **rd (ra = x1):** `00001`
- **Opcode:** `0000011`
#### Binary Representation:
- Immediate (12 bits): `000000111100`
- rs1 (5 bits): `00010`
- funct3 (3 bits): `011`
- rd (5 bits): `00001`
- Opcode (7 bits): `0000011`

---

15. ### `lw s0, 12(sp)`
**lw (Load Word):** Loads a 32-bit value from memory into a register.
#### Instruction: `lw s0, 12(sp)`
- **Opcode:** `0000011` (7 bits)
- **Immediate:** `12` (`0000000000001100`, 12 bits)
- **Destination Register (rd):** `s0` (`x8`, 5 bits)
- **Base Register (rs1):** `sp` (`x2`, 5 bits)
- **Function (funct3):** `010` (3 bits)
#### Breakdown:
- **Immediate (12):** `0000000000001100`
- **rd (s0 = x8):** `01000`
- **rs1 (sp = x2):** `00010`
- **funct3:** `010`
- **Opcode:** `0000011`
#### Binary Representation:
- Immediate (12 bits): `0000000000001100`
- rd (5 bits): `01000`
- rs1 (5 bits): `00010`
- funct3 (3 bits): `010`
- Opcode (7 bits): `0000011`

---

* This completes the breakdown for all 15 instructions!
</details>
