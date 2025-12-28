## Project: MetroSim

## Course: Data Structures (Spring '24)

## Program Purpose:
    The purpose of this program is to simulation a metro system. Clients can
    add passengers to the simulation at any point, and they will start out at 
    their arriving station. Clients can also move the metro along to their next 
    station, and the program will automatically pick up and drop off passengers 
    accordingly. When the client is finished or the given command file ends, 
    the simulation ends.

## Compile/run:
     - Compile using
            make MetroSim
     - run executable with
            ./MetroSim stationsFile passFile [commands]


## Acknowledgements: 
    std::vector and std::list documentation on 
        cppreference.com
    std::list examples from guru99.com
    Hannah Friedlander for emotional support.
    Willa Andandre for emotional support.


## Files: 

### main.cpp:
     Creates MetroSim object and runs the
     simulation

### MetroSim.cpp:
     Runs the query loop for MetroSim and prints out 
     simulation after each command

### MetroSim.h:
     Function and member declarations for MetroSim.cpp

### stations.txt:
     an example file containing a list of stations.

### test_commands.txt:
     sample list of commands that you could give to the 
     simulator

### the_metroSim:
     reference implementation for students to see how 

### PassengerQueue.cpp: 
     Represents queue of passengers at a station or in a 
     train car. Also provides functions to deal with 
     passengers in the queue. 

### PassengerQueue.h:
     Function and member declarations for 
     PassengerQueue.cpp

### Passenger.cpp:
     Represents a single passenger in the simulation. 

### Passenger.h:
     Function and member declarations for Passenger.cpp

### unit_tests.h
     Contains tests for functions in PassengerQueue.cpp
     and Passenger.cpp
      
### test_commands.txt:
    given testing input file

### input(1-4).txt:
     4 different command files for testing MetroSim. When testing with
     these inputs i created different output and diff files.

### LIST OF INPUT FILES (number representations)
     1: Adds 5 passengers to 3 diff stations(3 diff dep 
     stations too) then pick all 15 up and drop them off

     2: Only command is "m f"
 
     3: Only command is "m m" a bunch of times

     4: Run all query commands. 
        Add passengers at beginning, middle, and end of 
        simulation. arriving station # < departing #, 
        arriving # > departing #, arriving # = departing 
        #
     5: abrupt stop. No m f but code stops one reached the end of the file

## Data Structures: 

    I used a queue ADT to represent the passenger queue, which is used for each
    station and train car. I decided on this ADT because it follows the first in 
    first out rule, which I noticed is what the MetroSim demo does. Also, popping 
    from the front and adding to the back has O(1) efficiency. 
    To represent the queue ADT, I decided to use std::list. With a queue, there is 
    a lot of removing from the front, and an std::list is more efficient than an 
    std::vector for this. With an std::vector you have to move over every
    other element in the vectors after popping from the front, but you don't have to
    with an std::list. Instead, with an std::list you just have to move the "front"
    pointer to the next element in the std::list.
    A situation where I can use a queue ADT is if I want to create a line in an
    amusement park. The people who get in line first should be the first ones to get
    on the ride. Another situation where I can use a queue is if i want to represent
    a stack of cups next to a water jug. The cups can be in a holder where you take
    them from the bottom, but you fill at the top. Therefore, the first cups to be
    filled are going to be the first ones to be taken and used.
  
    I used a list ADT to represent all of my stations and my train as a whole. I 
    decided to use this ADT because I wanted to be able to add elements to the end
    of my list without having to know the size of beforehand. If I used an array
    instead, then I would have to know the number of station and number of train
    cars before knowing what each station is, which is impractical. To represent
    the list ADT, I decided to use std::vector. An std::vector has O(1) access to 
    any of its elements because each element is right next to eachother in memory, 
    whereas an std::list has O(n) access because it's elements are scattered. This
    was important in my decision because I wanted to be able to efficiently access
    any station or train car if needed. 
    A situation where I can use a list ADT is if I want to build a jenga tower. I'm
    not sure how tall the tower will be because im not sure hpw many pieces I have, 
    and I want to be able to have O(1) access to any of the blocks so i can remove 
    them during the game. Another situation where I can use a list is if I want to
    play dodgeball. ALl the kids line up against the wall but I'm not sure how many
    kids there are, and when I make my team I want to be able to efficiently choose
    any of the kids against the wall. 
    
    One algorithm i found complex was used in my move_metro function. I thought it
    was complex because it involved a lot of different pieces, such as boarding the
    passnegers onto the train, moving the train to the next station, and departing
    the passengers from the train. To board the passengers, I accessed the station's
    queue. Starting at the front goin back, for each passenger I access their
    departing station, and then enqueue them to the corresponding train car
    (because car is sorted by departing stations). To move the train, I simply just
    increase the number containing what station the train is currently at. Then, to
    depart the passengers from the train, i go to the train car that corresponds TO
    where the train moved to, and then starting from the front of the queue I 
    dequeue every passenger and print them out to the output file.
    
    An algorithm I found interesting was my print_metro_sim function because I had
    never had practice printing to anything but cout. In the beginning, for each
    train car I print out its passenger queue. Then, I have a for loop that prints
    out each station and their corresponding number. However, in my for loop I check
    to see if the train is at the current station I'm on, and then I print "TRAIN:"
    before it, which I found a little confusing.




## Testing:

    I did all of my phase 1 testing using unit_test.h. For my Passenger class, I
    tested the print function after creating a single pasenger to make sure it
    printed correctly. I also tested the constructor to make sure it allocated
    and deallocated memory correctly. 
    For my PassengerQueue class I first tested the constructor to make sure it
    allocated and deallocated memory correctly. Then I tested my enqueue() by
    enqueuing a single passenger and multiple passengers (in different tests) then
    asserted their print statements with the expected print statement. This tests
    my enqueue function and my print function simulatenously. I followed the same 
    pattern for my dequeue() function, dequeuing once and multiple times then
    asserting the print statement. For my front() funciton, I decided to throw an 
    error if it was an empty queue, so one of my tests tested to see if it threw 
    the error correctly. My next tests checked to see if it returned the correct 
    passenger when the queue is one passenger and multiple passengers long. Then I 
    tested after dequeuing a passenger. For my size() function, I tested it with 
    an empty queue, a queue with one passenger, multiple passengers, after 
    dequeuing, and after dequeuing then enqueueing. Some bugs I encountered during 
    my phase 1 testing was my size anfter dequeue because i forgot to change it, 
    and with some of my print statements because I accidentally had spaces or 
    wrote the wrong number in the comparison string. 
    
    For phase 2, I did all of my testing with diff. I created 6 different input 
    files all for different cases. The first file (input1.txt) adds 5 passengers 
    to 3 different stations, and all 15 passengers have 1 of 3 different 
    destination stations. I add all of these passengers in the beginning and then 
    move the metro until they all get to their destination. This was to test if 
    they got on and off the train in the correct order. Input2.txt tests what 
    would happen if my first command is m f. input3.txt just calls m m a bunch of 
    times without adding any passengers (ending with m f). Input4.txt ads 
    passengers at the beginning, throughout moving the metro, and at the very end 
    right before calling m f. The passengers range from having their arrival 
    station (arr) be before departure (dep), dep before arrival, dep same as 
    arrival, arr being the last station and dep the first, and dep being the last 
    and arr being the first. Input5.txt checks to see what will happen if there is 
    no m f at the end of the file. Input6.txt checks to see what will happen if i 
    add passengers then don't end the file with m f. Input7.txt checks to see what 
    will happen if i call other commands after calling m f. Most of my bugs came 
    in the compiling stage, because before compiling I would realize that a big 
    chunk of my code didn't work and then i would fix it in one area but forget to 
    fix it in another area. I also had a bug where my code couldn't tell the 
    different between the command file and std::cin, and if my command file didn't 
    end with m f then it would get caught in an infinite loop. I "cat"-ed my diff 
    files to see what was going on which really helped with debugging.

## Part that you found most difficult: 
    The part that I found to be the most difficult was learning how to use
    std::vector and std::list because we didn't really go over it in class so I
    had to rely on online resources to learn. I also found moving the Metro
    to be difficult because I had to account for it reaching the end of the
    stations list, boarding passengers, and departing passengers, which is a lot
    compared to the other query commands.

## Time Spent: 
    About 15 hours
