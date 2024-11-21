[GitHub - khuongduy354/olc6502nes](https://github.com/khuongduy354/olc6502nes) 
-> contains some design notes
# CPU  
- output address  : s.t other device can detect address and write to bus  
- input data  : read data  from bus
- output data  : write data from bus  
- implements a 6502 processor which: 
	- each instruction has size from 1->2 bytes. 
	- clock cycles is different depend on the instruction size & duration 
- instruction vary from 1-3 bytes, opcode = 1 byte, argument = 1-2bytes (2 hex = 1 byte) 
- each instruction is associated with a 2 digit number (in hex), there're (16x16=256 instructions), see below.
- steps: 
	- read bytecode 
	- map bytecode -> instruction
	- address mode 
	- execute opcode 
	- calc for clock cycles 
### implementation  
- each address mode is a function, load/or not next bytes into registers, return additional required clock cycles  when finished  
- each opcode ...(same as above) execute instructions, ... (same as above)
#### clock
- clock(): emulated on number, not time. 1 clock = instruction required_cycles = addressmode() + opcode().  
-> calling clock() run the program
### memory  
- bus handles everything in middle 
- page: 1 address = 0xFF55,  page is FF, 55 is offset.   
- RAM is 2kb with 8kb total as mirroring   ([Mirroring - NESdev Wiki](https://www.nesdev.org/wiki/Mirroring))
![[Pasted image 20221225100029.png]]
### ppu 
- store pixels & color & stuffs 
### rom (cartridge)
- store program, sometimes even palletes, patterns
- patterns use to store characters (which is a 8 bit map)
- a mapper (shown below), map to specific places on ROM ([Mapper - NESdev Wiki](https://www.nesdev.org/wiki/Mapper#Common_capabilities)) allows cartridge store more data than addressable range(banking). 
also add more func like RAM
#### mapper
- take address and output depends on ROM prg and chr sizes.
![[Pasted image 20221225095939.png]]
# Workflow   
- read bus first if you wanna understand workflow 
- bus is like the main NES that glues everythings. each has its own function, bus handle it all, for ex 
```c++ 
class bus{
void connectCartridge(cartidge){
this->cart=cartridge
ppu.connectCartridge(cartridge)
}
}  

//in bus.clock() 
cpu.clock() 
ppu.clock()
```
- ppu clock renders
- cpu clock read pc, and move it 
-> execute address mode (may move pc to arbitrary places) -> opcode trigger 
-> calc  clock cycles -> return clock cycles     
- ppu is pretty similar, which includes mainflow of program  
- bus summarize both clock() function 
-> which means **calling bus.clock() runs the program**
### Read and write 
```c++ 
//Bus handle main read  
// take in address 
bool allow_read = CARTRIDGE.read()  //CARTRIDGE read first  
//if it allows further read: 
if allow_read: 
switch case{ 
CPU => ram[address]
PPU => ppu.read() 
} 
//same to write  

// INSIDE CARTIDGE 
mapped_addr = Mapper.read(address) // Mapper read first  
if mapped_successfully:
	data = PRG[mapped_addr]
```






# NES emulator
--- 
# Refererences 
[NES Emulator Part #1: Bitwise Basics & Overview - YouTube](https://www.youtube.com/watch?v=F8kx56OZQhg&list=PLrOv9FMX8xJHqMvSGB_9G9nZZ_4IgteYf&index=4)
[archive.6502.org/datasheets/rockwell_r650x_r651x.pdf](http://archive.6502.org/datasheets/rockwell_r650x_r651x.pdf)
[GitHub - khuongduy354/olc6502nes](https://github.com/khuongduy354/olc6502nes)

2022 10 18 17:28
#literature   [[computer science]] [[low level]] [[emulator]] [[emulator implementation]] [[implementation]] [[Atomic Ideas/C++]]