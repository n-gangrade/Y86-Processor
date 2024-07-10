# Y86-Processor
## Fetch
The Fetch unit takes the instructions from instruction memory to generate the different instruction fields: icode, ifun, rA, rB, and valC.  It also does PC increment in order to get valP (possibly the next instruction’s address).  We used a ROM for the instruction memory, which takes in the  PC address, and outputs the instructions.  The first byte is the icode and ifun.  The icode is then used to determine if the instruction is valid, if valC is needed, and if register ids are needed.  Then, depending on the instruction type, rA, rB, and valC are determined in the align block.
![unnamed](https://github.com/n-gangrade/Y86-Processor/assets/135069685/365d145a-30b7-46be-a1ef-2fa46dedfaa2)

## Decode and Write Back
We combined our decode and write back into one unit.  The decode part takes in srcA and srcB, which are the registers that we want to read from.  It outputs valA and valB, which are the values inside those register files.  The write back part takes dstE and valE from the ALU.  It takes dstM and valM from memory.  Depending on the instruction that is currently being executed it writes either valM or valE to dstM or dstE respectively.
![image](https://github.com/n-gangrade/Y86-Processor/assets/135069685/6bb69fd5-4417-48e5-be54-6122b5ea6237)
![image](https://github.com/n-gangrade/Y86-Processor/assets/135069685/acb58b2d-3550-4900-bc05-cb274bb47239)

## Execute
Our execute unit takes in icode, ifun, valA, valC, and valB.  It outputs valE, the zero flag, the sign flag, and the overflow flag.  First ALU_A determines if valA or valC will be used, and ALU_B determines whether valB is used.  The ALU_fun takes in ifun, and icode and determines whether ifun will be used.  ALU_A, ALU_B, and ALU_ifun are fed into REAL_ALU in order to get valE (using addition, subtraction, and xor), and have all the flags set to the correct value.

SetCC takes in icode and determines whether a flag should be set for this instruction.  This is then fed into CC, along with the flags to store the last condition code.  This is then taken in by cond with ifun in order to set the jump condition.
![image](https://github.com/n-gangrade/Y86-Processor/assets/135069685/287dcce5-aa55-4288-9cc5-d64461cbdb8f)

## Memory
The memory unit takes in icode, valE, valA, valP, and the clock cycle.  Mem_addr takes in icode, valE and, and valA in order to determine which address will be used in data_memory.  Mem_write and mem_read take in icode and determine whether we are going to be reading or writing from data_memory.  Mem_data takes in icode, valA, and valP to determine what will be written to memory.  If the data_memory is being read from then it outputs valM, if it’s being written to it just signals that the writing is complete once the RAM has been written to.
![image](https://github.com/n-gangrade/Y86-Processor/assets/135069685/91534647-d7b0-4e34-a569-ccb62ce39456)

## PC Update
This unit takes in icode, valP, valC, valM, and cnd.  Cnd is used to indicate if a jump is being made.  Depending on the current instruction, it determines what the next instruction’s address is so that the PC can go there next.  It outputs this as newPC.
![image](https://github.com/n-gangrade/Y86-Processor/assets/135069685/5b2e644e-3139-4f66-be14-51f1d4bfbd04)

# Examples
## Test Program 1
![image](https://github.com/n-gangrade/Y86-Processor/assets/135069685/b5f8897f-9d94-4026-b14d-1e24d0a9d059)
![image](https://github.com/n-gangrade/Y86-Processor/assets/135069685/3af811e8-9078-40c5-b318-b7ad6d855318)
                
Changes in Registers				                                                                                  
![image](https://github.com/n-gangrade/Y86-Processor/assets/135069685/5e3de665-a9cb-4f3f-971a-cebdebb3ef1d)   

Changes in Memory
![image](https://github.com/n-gangrade/Y86-Processor/assets/135069685/a9879780-e392-4698-89ff-be81ab0e11ad)




# Test Program 2
![image](https://github.com/n-gangrade/Y86-Processor/assets/135069685/6c32cb74-892a-468f-83fa-756e37cd4b3a)
![image](https://github.com/n-gangrade/Y86-Processor/assets/135069685/7ea4e779-b2fe-4e43-a731-7e73a90cfe77)

Changes in Registers

![image](https://github.com/n-gangrade/Y86-Processor/assets/135069685/8b085fab-c1b8-4d2b-9977-df251983927a)

Changes in Memory

![image](https://github.com/n-gangrade/Y86-Processor/assets/135069685/4f39c9f5-4810-4331-93db-40b08e64688e)
