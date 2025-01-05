#  samsung-riscv

This is a RISC-V Talent Development Program, powered by Samsung Semiconductor India Research (SSIR) along with VLSI System Design (VSD).

##  Basic Details About Me
**Name:** NIRANJAN KHATAVKAR R\
**College:** Vidyavardhaka college of engineering\
**Email ID:** niranjanr916@gmail.com\
**LinkedIN Profile:** [Niranjan Khatavkar R](https://www.linkedin.com/in/niranjan-khatavkar-r-2ab34024a/)
---------------------------------------------------------------------------------------------------------------
<details>
<summary><b>Task 1:</b> Install the RISC-V toolchain using the VDI link and Perform GCC Compilation In Both Normal gcc-compiler and RISC_V Based gcc compiler and check the result. </summary>   
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
$ riscv64-unknown-elf-gcc -o1 -mabi=lp64 -march=rv64i -o sum.o sum.c
```

![image](https://github.com/user-attachments/assets/e4049d01-121d-4088-a580-742b696f2416)

Verify that the file has been compiled using below command

```
$ ls -ltr sum.o
```

**4. Check the assembly level file and know the RISC-V command operations**

