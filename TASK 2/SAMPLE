Process Overview:

The code snippets above and the disassembly image represent the compilation of C code to the RISC-V architecture and its low-level representation. First, the sum1ton.c 
file, which probably computes the sum of numbers from 1 to 100, is compiled into a RISC-V object file sum1ton.o with the riscv64-unknown-elf-gcc compiler and some 
flags for optimization and target architecture. This object file holds the machine code produced by the compiler. The objdump utility is then used to disassemble
the object file, converting the machine code into human-readable RISC-V assembly instructions.
Finally, the spike emulator is used to execute the generated RISC-V code, enabling dynamic analysis and debugging with commands such as
until and reg to observe the program's behavior at runtime.

Disassembly Analysis:

The disassembly reveals the low-level instructions that the compiler generated for the C code.
It shows fragments of the main function, including instructions for loading immediate values, 
adjusting the stack pointer,performing arithmetic operations, calling the printf function, 
and managing the function's return. The disassembly also shows part of the register_fini function,
which is likely responsible for cleaning up resources before program termination. From the assembly code,
one can gain an understanding of how the compiler translates high-level C constructs into the underlying machine 
instructions for this RISC-V architecture.
