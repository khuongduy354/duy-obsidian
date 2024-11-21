The kind of emulators which this document talks about are software emulators of computers. A  
software computer emulator has to emulate all the components in a real computer using software  
programs over another computer. The typical, Von Neumman, computer architecture is formed by one or  
more CPUs which are the core of the computer and perform calculations, a bus, to which are attached  
devices and memory, and the memory and devices. All those components must be emulated by software.  
We will talk in this document about how each of those components could be emulated.  
This definition is related with the concept of virtual machines (VM) like Java and others. A virtual  
machine is any kind of computer which does not exists as a real hardware but it is implemented by  
software over a real computer. A virtual machine has many uses like providing code portability or hiding  
specific hardware characteristics to the software (for example the OS provides a virtual machine to the  
user programs). And of course a virtual machine can be implemented to emulate another computer over  
our computer. So an emulator can be viewed as a virtual machine.