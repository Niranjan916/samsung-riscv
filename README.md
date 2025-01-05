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

* Now see here mine file took total of **23** instructions to complete the execution of int main() part.
* So, how did I get to know it took 23 instructions simple 101b0-10184=2C. 2C divided by 4 you get 11. 101b0 are last numbers before the atexit part see the image.
* These is when I used ```$ riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum.o sum.c``` command
* If Replace -O1 with -Ofast i.e ```$ riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum.o sum.c``` then you would see reduction in instruction cycle.
  
 ![image](https://github.com/user-attachments/assets/72ba2688-3da2-4e10-89e1-ffc8313c70e1)

 * For mine it is same but for you it should change for that please change the **a** value from 10 to 100 or something big number.
