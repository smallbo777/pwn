# pwnable.kr-bof writeup

step 1 analyze source code
--
start from main function first

![image](https://github.com/user-attachments/assets/d622287e-9429-4e24-a27e-8409232b78a6)

we can see there is only one function func runs in main

take a look at func

there is a var call overflowme contains 32 bytes

**when we put it in gets bilt-it function, the function will not check your size of parameter, it means there will be a vulnerability of buffer overflow**

![image](https://github.com/user-attachments/assets/1d497505-c6b3-4098-ba0c-d23a7f3fd136)

it cause a smashing after we inputs over 32 bytes 

So, we need to find how many bytes can cause buffer overflow

Step 2 find the overflow bytes
--
Next, we need to find the key var memory address

we can see that we put a 0xdeadbeef into function func in main

so, we should find the key var address

use gdb or objdump to see all the address of source code 

disassembly source code using objdump -d bof

![image](https://github.com/user-attachments/assets/27ffdaf8-bc5d-4156-bb30-9bc5a313e3c7)

we found that the ebp's address(var overflowme's address) is 0x2c 

according to the assembly code, it loads the ebp's address into register eax

Next, we need to find what is the key var deadbeef's address is at ebp+8

so, the overflow bytes is 0x2c+0x8=0x34(in decimal is 52)

Step 3 write a python script
--
In order to bof it, we can use a useful python package call pwntools

![image](https://github.com/user-attachments/assets/a67f4d4d-c253-4cda-a976-4225404d8f6d)

here is the script, is very simple

we just need to send 52 a  + the key var 0xcafebabe, it will cause buffer overflow

Step 4 run the script
--
run the script

ls the file 

cat flag

![image](https://github.com/user-attachments/assets/2f138d65-6f55-4a7c-9f08-01957aa45842)

finally, we got the flag :))))


