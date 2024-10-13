#SRE 
Address Space Layout Randomization (ASLR).
0x7fffffff0d70 is a memory address, if ASLR is enabled it will randomize the middle bytes.

If it is not enabled then a stack address will be close to where the payload is.

So when a hacker puts malicios code on a system and ASLR is not enabled they can find the memory address easily and control the code excecution.