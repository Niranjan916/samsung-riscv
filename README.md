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
