# pwnable.kr-bof writeup

step 1 analyze source code
--
start from main function first
![image](https://hackmd.io/_uploads/BJhSgU_Qye.png)
we can see there is only one function func runs in main
take a look at func
there is a var call overflowme contains 32 bytes
**when we put it in gets bilt-it function, the function will not check your size of parameter, it means there will be a vulnerability of buffer overflow**
![image](https://hackmd.io/_uploads/r1SBSUuXJe.png)
it cause a smashing after we inputs over 32 bytes 
So, we need to find the offset  of ebp to bof it

Step 2 find the overflow bytes
--
Next, we need to find the key var memory address
we can see that we put a 0xdeadbeef into function func in main
so, we should find the key var address
use gdb or objdump to see all the address of source code 
disassembly source code using objdump -d bof
![image](https://hackmd.io/_uploads/ByF2UPuXJx.png)
we found that the ebp's address(var overflowme's address) is 0x2c 
according to the assembly code, it loads the ebp's address into register eax
Next, we need to find what is the key var deadbeef's address is at ebp+8
so, the overflow bytes is 0x2c+0x8=0x34(in decimal is 52)

Step 3 write a python script
--
In order to bof it, we can use a useful python package call pwntools
![image](https://hackmd.io/_uploads/S1X3FDu71g.png)
here is the script, is very simple
we just need to send 52 a  + the key var 0xcafebabe, it will cause buffer overflow

Step 4 run the script
--
run the script
ls the file 
cat flag
![image](https://hackmd.io/_uploads/B1imiwO7yx.png)
finally, we got the flag :))))


