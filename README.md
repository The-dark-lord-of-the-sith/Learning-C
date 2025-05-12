# Learning-C
Journey into Firmware Engineering

Day 1 : Powering up with structs, functions and pointers 






ASSEMBLY :: 
Assembly code is a low-level programming language that directly corresponds to machine instructions executed by a processor.

Assembly is human-readable but closely tied to the hardware. Let us break it down step-by-step and then dive into RISC-V examples.

Instructions
These are the basic operations the CPU executes (e.g., add, load, jump).
In assembly, each line typically represents one instruction.
Instructions consist of an opcode (operation code) and operands (data or registers to operate on).
Registers
Small, fast storage locations inside the CPU.
RISC-V has 32 general-purpose registers, labeled x0 to x31. Some have special purposes (e.g., x0 is always zero).
Assembly code often manipulates data in registers rather than memory for speed.
Operands
Can be registers, immediate values (constants), or memory addresses.
Example:
add x1, x2, x3 uses registers as operands;
addi x1, x2, 5 uses an immediate value (5).
Syntax
Typically: [opcode] [destination], [source1], [source2].
RISC-V uses a consistent format, often with commas separating operands.
Labels
Symbolic names for memory addresses, used for jumps or branches (e.g., loops or function calls).
Example: loop: marks a spot in the code.
Comments
Ignored by the assembler, used for human readability.
In RISC-V, comments start with #.
Directives
Commands to the assembler (not CPU instructions), like .data to define data or .text for code sections.

RISC-V Instruction Types
RISC-V has a reduced instruction set, meaning it keeps things simple with a few key instruction formats:

R-type: Register-to-register operations (e.g., arithmetic).
I-type: Immediate operations (e.g., add a constant).
S-type: Store instructions (save to memory).
B-type: Branch instructions (conditional jumps).
U-type: Upper immediate (large constants).
J-type: Jump instructions (unconditional jumps).
Code Examples
Now, let us see this in action with examples. Examples of RISC-V Assembly Code

Basic Arithmetic (R-type and I-type)
Let’s add two numbers stored in registers and then add a constant.

# Add two registers: x1 = x2 + x3
# R-type: x1 gets the sum of x2 and x3
add x1, x2, x3

# Add an immediate value: x4 = x1 + 10
# I-type: x4 gets x1 plus 10
addi x4, x1, 10
add is an R-type instruction: it operates on three registers.
addi is an I-type instruction: it uses two registers and an immediate value.
Anatomy: [opcode] [destination], [source1], [source2 or immediate].
Loading and Storing Data (I-type and S-type)
Let’s load a value from memory into a register and store it back elsewhere.

# Load word from memory address in x5 into x6
 # I-type: Load word from address (x5 + 0) into x6
lw x6, 0(x5)

# Store x6 into memory at address in x7
# S-type: Store word from x6 into address (x7 + 4)
sw x6, 4(x7)
lw (load word) fetches 32 bits from memory into a register.
sw (store word) writes a register’s value to memory.
The number (e.g., 0 or 4) is an offset added to the base address in the register.

Branching (B-type)
Let’s write a simple loop that increments a counter until it hits 5.

# Initialize x1 to 0
# x0 is always 0, so x1 = 0
addi x1, x0, 0

loop:
  # Increment x1 by 1
  addi x1, x1, 1

  # If x1 == x5, jump to 'exit' (assume x5 holds 5)
  beq x1, x5, exit

  # Jump back to 'loop'
  j loop

exit:
  # Program ends
beq (branch if equal) compares two registers and jumps if they’re equal.
j (jump) is a J-type instruction for unconditional jumps.
loop: and exit: are labels marking addresses.
Function Call (J-type and I-type)
Let’s call a subroutine to double a number.

# Main program

# x1 = 3
addi x1, x0, 3

# Jump to 'double', store return address in x10
jal x10, double

# Subroutine to double x1
double:
  # x2 = x1 + x1 (double it)
  add x2, x1, x1

  # Return to address in x10 (x0 discards link)
  jalr x0, x10, 0

# Back in main, x2 now holds 6
jal (jump and link) jumps to a label and saves the return address.
jalr (jump and link register) returns using the address in x10.
Registers like x10 are conventionally used for return addresses.
Data Section (Directives)
Let’s define some data and use it.

# Data section
.data
  # Define a 32-bit word with value 42
  my_num: .word 42

# Code section
.text
  # Load address of 'my_num' into x1
  la x1, my_num

  # Load the value (42) into x2
  lw x2, 0(x1)
.data and .text are directives telling the assembler where data and code go.
la (load address) is a pseudo-instruction that simplifies getting a label’s address.
Putting It All Together
Here’s a small program to sum numbers 1 to 5:

# Sum numbers from 1 to 5

# Counter (i)
addi x1, x0, 0

# Sum
addi x2, x0, 0

# Limit
addi x3, x0, 5

loop:
  # i++
  addi x1, x1, 1

  # sum += i
  add x2, x2, x1

  # If i != 5, repeat
  bne x1, x3, loop

# x2 now holds 15 (1+2+3+4+5)
Uses registers x1, x2, x3.
Combines I-type (addi), R-type (add), and B-type (bne).
Things to remember about RISC-V Assembly
Fixed-length instructions: All are 32 bits, making decoding simple.
Load/store architecture: Only lw and sw access memory; arithmetic uses registers.
Minimalist design: Fewer instructions than complex ISAs like x86, but still powerful.
