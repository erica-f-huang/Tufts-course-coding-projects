## Project: UM emulator
Program for a UM emulator

## Class: Machine Structure and Assembly Lanugage Programming
Partner: Rindha Reddy

## Implementation Completeness 
    - um emulator program fully implemented and tested to the best of our abilities
    - UMTESTS unit tests fully implemented to the best of our abilities
    - UM Memory Architecture tests(test.c) fully implemented to the best of our 
            abilities

## Significant Departures from Design 

    - changed segment of function pointers to a switch table, where each case
            calls its corresponding instruction function
    - made registers a CArray instead of a sequence, and moved into our main module
            instead of out instructions module
    - remove I/O module and put intended functionality into our instructions module

##  Architecture (files)

### um: the main for the um emulator program. Includes the registers and execution
        cycle for the program as well. Handles the command line input reading
        and calls memory functions from the memory module to allocate and set
        up the UM Memory. Also calls all 14 UM instruction functions from the 
        instructions module in a switch table that is based on the opcode. Some
        secrets it keeps are how the registers are represented (a CArray) and 
        how the execution cycle is run (a for loop and switch table). 

### instructions: contains functions for the 14 UM instructions. These
        functions get called from the um module during the execution cycle. 
        Certain functions (like segmented load, segmented store, map segment, 
        unmap segment, load program, and load value) use functions from the 
        memory module. Functions typically take in pointers to the registers, 
        the main memory, or both. Instructions doesn't keep any secrets, but we
        decided to make it its own module because it has very specific
        functionality. The instruction module does a lot of the accessing data 
        structures so the client doesn't have to.

### mem: contains functionality for the UM's memory. Handles most of the work for
        the memory related UM isntructions. Functions from mem get called from
        the um module and the isntructions module. The mem module's secret is 
        how the um_mem works under the hood, which is a struct the contains
        a segment for the memory, and a stack for the recycled IDs. Clients
        of the mem module just need to know that the um_mem contains all
        segments and what recycled ID is available to them.

### test: not really a module for our program but uses functions from instruction 
        and mem modules for testing. Created for architecture tests. No
        abstractions or secret keeping


##  Time to run 50 Million Instructions 
    ANSWER: 4.7575 seconds
    
    STEPS:
    - run with midark and time it (7.63 seconds)
    - pipe midmark into a file and count how many lines are in it (about 85 million)
    - divide the time by 85 million to get average time per instruction
            (about 89.7647 nanoseconds)
    - multiply by 50 million to get the time to run 50 million instructions
            (about 4.7575 seconds)

##  UM Unit Tests 

### halt.um 
        What: tests halt instruction (can run valgrind to make sure memory is
                cleaned up)
        How: creates um file with one instruction which is halt
### halt-verbose.um
        What: tests that nothing is run after halt is called
        How: creates um file that starts with halt instruction, then runs other
                instructions (loadval, outpu)
### add.um
        What: tests that adding doesn't have any errors
        How: creates um file that first adds than halts
### print-six.um
        What: tests that loadval and add work correctly
        How: creates um file that loads values, then adds them together and 
                outputs the result, then halts
### output_one_readable.um
        What: tests output on readable asciii characters
        How: creates um file that loads readable ascii value, then outputs it,
                then halts
### output_one_nonreadable.um
        What: tests output on nonreadable ascii characters
        How: creates um file that loads nonreadable ascii value, then outputs
                it, then halts
### output_hello.um
        What: tests output on "strings"
        How: creates um file that loads ascii values for each letter of 
                "hello", then outputs each character, then halts
### output_two_nonreadable.um
        What: tests output on multiple nonreadable ascii characters
        How: creates um file that loads multiple nonreadable ascii values, 
                then outputs them, then halts
### input_one_char.um
        What: tests input and makes sure correct char is loaded by outputting
        How: creates um file that takes in input of one char, then outputs it,
                then halts
### input_hello_output_leloh.um
        What: tests outputting in a different order than inputted
        How: creates um file that takes in input of 5 characters (hello), then
                outputs them in different register order (leloh), then halts
### conditional_move_lv_false.um
        What: tests conditional move with load value where r[C] is 0, so move
                doesn't happen
        How: creates um file where 0 is loaded into r[C] and other values are 
                loaded into the other 2 registers, then does conditional move.
                Then outputs the values of the other 2 registers (nothing will
                have changed), then halts
### conditional_move_lv_true.um
        What: tests conditional move with load value where r[C] isn't 0, so 
                move happens
        How: creates um file where values are loaded into 3 registers, with
                the r[C] register having a non-zero value, then does
                conditional move. Then outputs the other 2(non r[C]) register
                values to check that the move worked, then halts   
### conditional_move_input_false.um
        What: tests conditional move with input where r[C] is 0, so move
                doesn't happen
        How: creates um file that takes in input of 3 values, where the r[C]
                registers' input is 0, then does conditional move. Then outputs
                the values of the 2 non-r[C] registers to check move didn't 
                happen, then halts
### conditional_move_input_true.um
        What: tests conditional move with input where r[C] isn't 0, so move 
                happens
        How: creates um file that takes in input of 3 values, where the r[C]
        registers' input is not 0, then does conditional move. Then outputs
        the values of the 2 non-r[C] registers to check that move did
        happen, then halts 
### addition_input_small.um
        What: checks that adding values read from input works
        How: reads two values from input, adds, and checks output
### addition_lv_small.um
        What: adds the same register
        How: loads value in a register and adds that regsiter together with 
                itself and checks output
### multiplication_10_12.um
        What: multiplies two numbers of different registers and checks output
        How: loads two different regsiters with different numbrs, multiplies, 
                and checks output
### multiplication_6_6.um
        What: multiplies two numbers from the same register and checks output
        How: loads a value into a register, and passes that register in as both 
                factors. Then prints output
### multiplication_input.um
        What: checks that multiplying the values read from input works 
        How: reads two numbers in from input, multiplies them, and outputs 
                product
### division_3500_50.um
        What: divides two numbers and checks output
        How: loads two values into different registers, preforms division, and 
                outputs result
### division_80_2.um
        What: divides two numbers and checks output
        How: loads two values into different registers, preforms division, and 
                outputs result
### bitwiseNAND_55_91.um 
        What: does bitwise NAND on two numbers and prints output
        How: loads two numbers into different registers. Fills the most 
                significant 25 bits with 1 so that when we do NAND we can print 
                output. does bitwise NAND and prints output
### bitwiseNAND_63_111.um
        What: does bitwise NAND on two numbers and prints output
        How: loads two numbers into different registers. Fills the most 
                significant 25 bits with 1 so that when we do NAND we can print 
                output. does bitwise NAND and prints output
### map_segment.um
        What:tests that a mapped segment gets the correct segment ID
        How:maps a segment and stores the seg ID in a register. prints out that 
                register to check ID
### multiple_map_segment.um
        What: maps multiples segments and checks that they all get the correct 
                seg IDs
        How:maps multiple segments and stores the seg ID in different 
                registers. prints out those registers to check ID
### map_segment_load.um
        What: checks that loading a value from a word works
        How: maps a segment and loads one of the words. Should load a 32-bit 
                word of 0s because segment was freshly mapped
### map_segment_store.um
        What: checks that storing words in a mapped segment works
        How: maps a segment then stores a value into a certain word of that 
                segment. Loads that same word and prints output
### unmap_segment.um
        What: checks that unmap segment works correctly
        How: maps a segment and unmaps it without calling halt at the end. May 
                not have freed all memory, but look at the allocation count vs 
                free count to tell if unmapped correctly. run with valgrind
### unmap_multiple_segments.um
        What: checks that after leaving a segment mapped at the end of a file, 
                halt clean its up. Also checks that halt() doesn't try to 
                double free unmapped segments
        How: maps 3 segments and only unmaps 2 of them before calling halt. run 
                with valgrind
### load_program_move_pc.um
        What: Checks that just moving the program counter withint segment 0 
                works
        How: loads values into all registers. Has instructions to reload the 
                registers with different values, but before then it does a 
                load program to move the pointer counter so that it jumps to 
                the end of the reloading registers so only some registers are 
                reloaded. checks output
### load_program_seg_1.um
        What: builds a mini UM program and stores within a segment. Loads that 
                program as the new segment 0 and makes sure it executes 
                correctly
        How: maps a segment, and does segmented store to store instructions in 
                the mapped segment. Makes the segments by loading a 25 bit 
                value, shifting left, then loading the rest of hte 7 bits. Then 
                loads the segment and moves the pointer counter back to the 
                beginning segment we create has instructions to output 
                something
