## Project: Reverse Polish Notation

## Course: Machine Structure and Assembly Language Programming (Spring '25)

## Partner: Olivia Ribeiro

## Project description:
    Implements a reverse polish notation calculator using a course variation of assembly language programming. Uses the UM emulator built in the UM project.

## Acknowledgements:
     Course staff for help with understanding spec, implementing recursion,
     and dividing up sections.

## Implementation: 
    Everything has been implemented to the best of our knowledge.


## Calling Convention:
    r0: always holds 0 (non-volatile)
    r1: holds return addresses when neccessary or return values
    r2: call stack pointer
    r3: value stack pointer (non-volatile)
    r4: non-volatile register
    r5: volatile register
    r6: volatile register (globaly declared as temporary)
    r7: volatile register (globaly declared as temporary)


## Printing:
    We implemented printing using recursive division and modular division
    by 10 to access the least significant digits and print from most to least,
    handling negatives usint two's compliment and special cases for 0 and the
    most negative value.

## Value Stack:
    The RPN value stack is held in segment 0 with 10,000 spots allocated for it
    with a label pointing to the bottom and register 3 pointing to the top.


## .sections:
    text:      contains calculator functions
    data:      contains memory space allocation for call and value stacks
    rodata:    contains memory space allocation for jump table
    init:      initializes jump_table instructions, registers, stack pointers,
               and jumps to main when finished.


## Time Analyzing: 10 hours
## Writing Code: 6 hours
## Debugging: 3 hours
