see also [[NES emulator]]
# How it works 
- CPU cycles: define time interval in emulator 
- CPU: take instruction -> execute 
- GPU: take commands, video memory -> output images
- Sound: take commands -> output signal fed into speaker 
-> happen in parrallel
-> hard to do (multithreading). 
-> loop instead, things go in order. 
# CPU PU runs
## Interpreter: 
- PROGRAM OPCODE -> execute
## Translation:
- Program OPCODE -> OPCODE that CPU runs
JIT or AOT compiler 
# Memory 
- many memories: Registers, ROM, RAM, IO
-> IO is implement in  memory, cuz IO is linked to a memory address
- implement as a memory map in emulator
# Sync problems 
- as said above, parallel is hard -> emulator is run in sequence 
-> problem CPU run too many/ little instructions before GPU or Sound





# emulator implementation
--- 
# Refererences 
[www.codeslinger.co.uk/files/emu.pdf](http://www.codeslinger.co.uk/files/emu.pdf)



2022 10 17 19:34
#literature    [[computer science]] [[low level]] [[emulator]]