## Project: CalcYouLater
## Course: Data Structures (Spring '24)


## The purpose of the program
    The purpose of this program is to simulate a reverse polish notation (RPN) 
    calculator. Clients can preform basic math and inequality operations, along 
    with executing rstrings - a string of operations in reverse polish 
    notation. Clients can also read frym .cylc files, delete and duplicate 
    items from the calculator, run if statements, and print the results of 
    their most recent operation.


## Acknowledgements for any help you received
    - std::istringstream on cppreference.com
    - std::string on cppreference.com
    - TAs

## Files
  ### main.cpp:
      Creates RPNCalc object and runs it

  ### RPNCalc.cpp:
      Represents the RPN calculator. Provides functions to execute different
      operations for the calculator. Takes in client's input.

  ### RPNCalc.h:
      Function and member declarations for RPNCalc.cpp

  ### DatumStack.cpp:
      Represents a stack of datums. Provides functions to deal with elements
      in the stack.

  ### DatumStack.h:
      Function and member declarations for DatumStack.cpp

  ### parser.cpp
      Provides function to parse through an rstring. Removes extra whitespace
      or throws error if rstring braces dont match

  ### parser.h
      Function and member declarations for parser.cpp

  ### unit_tests.h
      Contains tests for functions in DatumStack.cpp
      and parser.cpp

  ### input[1-10].txt:
      LIST OF WHAT THE .TXT FILES TEST
      1: normal rstring with weird spacing
      2: nested rstring with weird spacing
      3: nested rstring with extra open brace
      4: nested rstring with extra closing brace
      5: pushing numbers to stack, printing, and qutting
      6: pushing int, bool, and rstring to stack, printing
         after each push
      7: +, -, *, /, mod, printing after each operation
      8: <, >, <=, >=, ==, printing after each operation
      9: file command, printing after each call
      10: exec command, printing after each call
      11: if command, printing after each call

  ### input[12-15].cyl:
      LIST OF WHAT THE .CYL FILES TEST
      12: all math and inequality operators, and dropping
      13: pushing valid and invalid booleans, printing after
          each call
      14: running math operations with wrong data types,  
          printing after each call
      15: math operator
      16: dup, not, swap, drop, clear, print


## E: How to compile and run your program
    Compile/run:
        - Compile using
                make CalcYouLater
        - run executable with
                ./CalcYouLater

## F: An “architectural overview” i.e., a description of how your various program modules relate.

    parser.cpp contains a function that takes in an rstring, and removes all of 
    the extra whitespace, and then returns the cleaned up rstring.
    
    The datum class represents a Datum data type. A datum can be an integer, 
    boolean, or string. Functions within the class allow you to determine what 
    type the datum is, and once you know what type there are functions that allow 
    you to access the data. There are also operator functions such as =, ==, and <.
    
    The datum stack class represents a stack of datums. 
    The underlying datatype of the stack is an std::vector of Datums. Functions 
    provided by the class allow the client to check if the stack is empty, check 
    the size, clear the stack, get the top element of the stack, pop off datums, 
    and push datums onto the stack.
    
    The RPNCalc class represents a reverse polish notation (RPN) calculator. The 
    calculator is represented by a datum stack (from DatumStack.cpp). It's main 
    function is to run a query loop and take in input from the client until there 
    is no more input or the client inputs "quit". One function of the query loop 
    is to detect an rstring. If a client inputs an rstring then the query loop 
    calls the function from parser.cpp to clean up the rstring to to tell the 
    client know the rstring was incorrectly inputted. If a client gives the query 
    loop "#t", "#f", or an integer, the RPNCalc utilizes the datum class by 
    creating a datum of that type with that value, and then utilizes it's 
    underlying datumstack to push that datum onto the stack.
    
    The main class creates an RPNCalc instance and then calls the run() function 
    which calls the query loop and prompts input from the client.

## G: An outline of the data structures and algorithms that you used. 

    The ADT I used in my program is the stack ADT because I wanted my data to be 
    organized in a last in first out inventory. This is because with RPN notation, 
    the operation is preformed on the most recent datums added to the calculator, 
    which means that it will need a last in first out inventory. Since there was a 
    lot of dealing with the most recent elements and operations added to the 
    calculator, I decided to use for this was a vector as my underlying data 
    structure of the stack. One of its advantages is O(1) access anywhere in the 
    vector which would make it time efficient when popping from the back of the 
    vector. Since the data we are dealing with isn't too big, it also doesn't take 
    up too much space, even though it is less space efficient than a linked list. 
    
    One place I can utilize a stack is when I want to represent a stack of plates. 
    When I wash the plates I put them one on top of the other, but when I want to 
    use a plate I take from the top, so it is last in first out. 
    
    Another place where I can utilize a stack is when I want to represent a stack 
    of chairs. When I want to put the chairs away I put one on top of the other, 
    and when I want to set up the chairs I take from the top of the pile, so the 
    last chair stacked will be the first one picked.
    
    One interesting algorithm I enjoyed writing was the algorithm for my if_cmd() 
    function. I first pop off the top 3 datums off of the stack. Then i check if 
    the first and seoncd ones popped off are rstrings, and if the third one popped 
    off is a boolean. If either of these are false I throw a runtime error. Then I 
    turn the rstrings (the executables for true and false) into a string and add " 
    exec" to the end. I then turn them into istringstreams. Then i test the 
    conditional, and input the according strea into my query() function so it runs 
    a query loop within the main query loop of the RPN calculator. I added " exec" 
    to the end of my true and false rstrings so that they are executed by the 
    query loop when passed in.
    
    Another interesting algorithm I enjoyed writing was the pop functions. While I 
    was writing my code I realized I was reusing a lot of code to pop datums off 
    of the int, so I decided to turn it into a function. However, different 
    functions had a different number of datums they needed to pop, and those that 
    needed to pop more than 1 datum still needed the data of the datums they 
    popped. This gave me the idea to pass in pointers as parameters, so that I can 
    store the value of the popped datums in the pointer, and they will still exist 
    outside of the scope of the pop function so my other functions using it will 
    still have access to the values. With my pop_one(), however, I just return the 
    datum instead of passing it in as a pointer, but with my pop_two() and 
    pop_three() I made them void functions and had pointers. My pop_two() and 
    pop_three() both use my pop_one() function, and my pop_one() function uses 
    datumstack functions top() and pop() to retrieve the value and then delete the 
    datum. 


## H: Details and an explanation of how you tested the various parts of assignment and the program as a whole

### PART ONE: unit testing and diff
    DATUMSTACK TESTING:
    For all of my DatumStack.cpp testing, I used my unit_test.h file. I first 
    tested the constructor to see if it created and destroyed the instance 
    correcntly. Then I moved onto creating a constructor from an array and 
    testing to see if it was created correctly, making sure size() and isEmpty
    () also worked. I did this with an empty array and a small array. Then i 
    started testing my isEmpty() and clear() functions. Most of my tests 
    include creating a datum stack, empty or not, then clearing, popping, and 
    pushing to and from the stack. I change the order of these functions to 
    make sure my isEmpty() function works no matter what. This simultaneously 
    lets me test clear(), pop(), push(), and size(). After this block of 
    tests, I started getting more specific with my pop() function, making sure 
    I was popping the right things. I created a to_string() function whic 
    turned the stack into a string so it was easier for me to use the assert() 
    function. I run tests with different kinds of popping, like one, multiple 
    times, and until there is an error. Then I start testing the top() 
    function. I test it with an empty stack, after clearing a stack, after 
    popping, and after pushing.

    PARSERSTRING TESTING:
    For parserstring.cpp, I use both unit_testing and diff. For each of my 
    tests, I run the same test both in unit_test.h and with my input file, the 
    only difference is that in the input file there is unoraganized spacing. I 
    test a single rstring, a nested rstring, an rstring with more open braces 
    than closed, and an rstring with more closes braces than open.
    

### PART TWO: diff
    I first wrote the math and inequality operator commands because those 
    seemed the most straighforward to me. Then I wrote the print and quit 
    commands so I could use them in testing. Input5.txt tests pushing numbers 
    to the stack and then printing them to make sure they were pushed 
    correctly. It ends with "quit." Input6.txt tests pushing ints, bools, and 
    rstrings to the stack and making sure they print correctly. It doesn't end 
    with "quit." Input7.txt tests all of the math operators (+, -, *, /, mod) 
    and prints out the result each time. It doesn't end with "quit." Input8.
    txt tests all of the inequality operators (<, >, <=, >=, ==) and prints 
    out the result each time. It diesn't end with "quit." After I made and 
    tested all of these functions, I moved onto the exec command. I did a lot 
    of manual input for testing this, but I also created input10.txt to test 
    out every kind of input possible both, incorrect and correct. Once I 
    tested this thruoughly I moved onto writing file and if. For file, I 
    created input9.txt (i accidentally skipped 9 earlier w exec), and for if I 
    created input11.txt. I did the same thing as in input10.txt where I gave 
    the command every single kind of input, making sure to print afterwards. 
    After I thrugouhly tested these, I went back and did more testing with my 
    math and inequality operators, and then also implemented the drop 
    function. This is shown in input12.cyl. I also decided to move to .cyl 
    files because that is what the program takes in when it called file so I 
    just decided to match it. I then noticed a bug when pushing anything with 
    a #, so I went back and fixed it, then created input13.cyl to test edge 
    cases involving a # to make sure I fixed the bug. I did more testing with 
    my math operators as seen in input14.cyl just to make sure I wasn't 
    missing any edge cases. With input15.cyl I test what happens if I have 
    more commands after calling the "quit" command. Then I realized I didn't 
    yet test, drop, dup, not, swap, and clear, so I created input16.cyl to 
    test those. 


## I Tell us how much time you spent, in total, on this assignment in hours.
    About 18 hours
