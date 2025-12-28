## Project: Zap 
## Course: Data Structures (Spring '24)

## B. The purpose of the program.
    The purpose of this program is to compress or decompress a given file. The 
    client provides both an input and output file, and whether or not they 
    want to compress ("zap") or decompress ("unzap") the input file. The 
    program preforms the according operation, and stores the result in the 
    given output file.

## C. Acknowledgments for any help you received.
    TAs during office hours.

## D. List of the files, short description, and its purpose.
  ### main.cpp: 
      Creates HuffmanCoder object and runs the encoding or decoding 
      operation based on the client's input

  ### HuffmanCoder.h:
      Function and memeber declarations for HuffmanCoder.cpp
    
  ### HuffmanCoder.cpp
      Represents the HuffmanCoder. Provides functions for encoding and 
      decoding the given text.

## E. Instructions on how to compile and run your program.
    Compile/run:
        - Compile using
                make zap
        - run executable with
                ./zap [zap | unzap] inputFile outputFile

## F. Data structures and algorithms used. Discuss ADT & data structures used to implements it
  ### PAIRS:
      I use pairs multiple times throughout my program, the most notable 
      being in my two vectors. A pair comes from the utility class and is a 
      data structure that can hold two values, and each value can be a 
      different data type. cplusplus.com states that a pair is a instance of 
      a tuple, which is "an object capable to hold a colleection of 
      elements" where each element can be a different type. We also briefly 
      went over tuples in class earlier in the year. 

  ### VECTORS:
      I use two vectors in my program, each being a vector of pairs. One 
      vector is created with pairs of chars and ints to hold the character 
      frequencies in the given file to compress. The other is created with 
      pairs of characters and strings to hold each character from the text 
      and its binary code. 
  
  ### PRIORITY QUEUE:
      The main purpose of the priority queue is to create the Huffman tree. 
      The underlying data structure of the priority queue is a vector of 
      HuffmanTreeNodes. The priority queue is ordered so that the lowest key 
      value is at the back of queue, so it will be taken first with the 
      class's pop_back() function. When declaring the priority queue, we 
      also provide it with the NodeComparator function from HuffmanTreeNode.
      h so it knows how to sort the HuffmanTreeNodes within the undelrying 
      vector.

  ### BINARY TREE:
      When creating the HuffmanTree during the encoding process, a binary 
      tree is created using the HuffmanTreeNodes from the priority queue. 
      The priority queue takes the two smallest Nodes and gives the two 
      nodes the same parent (parent points to the two nodes but the two 
      nodes don't point back up to the parent). Then we add the parent node 
      back onto the queue, where it is reorganized. This continues until 
      there is a single node in the priority queue, which is the root of the 
      binary tree.

  ### UTILIZING DATA STRUCTURES ELSEWHERE
      One place I can utilize a priority queue is when I want to simulate an 
      hospital emergency waiting room. Those with a more severe injury will 
      be towards the front of the queue so they get tended to quicker. I 
      would use a priority queue instead of a normal queue becuase in this 
      case it doesn't matter as much when the patients arrive to the 
      hospital, so the order in which I add the patients to the queue 
      doesn't matter as much. Instead, what matters is how sever their 
      injuries are.

      Another place I can utilize a priority queue is when i want to 
      simulation a line for a rollercoaster. Those with a vip pass will be 
      pushed to the front of the queue regardless of the time they enter the 
      line, while those wihtout the vip pass will have to wait their turn. 
      This is more time dependent than the hospital waiting room, but there 
      is still a priority queue between the two groups

  ### INTERESTING/ COMPLEX ALGORITHMS
      An algorithm I really enjoyed implementing was the algorithm to create 
      the character encodings based on the HuffmanTree. This was the only 
      function I implemented recursively, and it was one of the first bugs I 
      tackled in the second week of this project. I kept a string variable 
      to keep track of the character's encoding, and when I would call the 
      recursive function, i would input either "str += 1" or "str += 0". It 
      took me a little while to realize that the variable was actually 
      updated when i did this, which messed up all of my encodings. However, 
      i enjoyed fixing this bug because it took a lot of thinking and 
      writing it out on paper, so when I finally found the bug I was very 
      proud. I also feel like I've struggled with recursion but with this 
      algorithm I was able to implement it more easily than I had with other 
      recursive functions in the past, which made me happy.

      An algorithm I found complex to implement was the algorithm to decode 
      the given text by simultaneously iterating through the binary string 
      and the Huffman tree. This was really hard to think out, because it 
      mean that I actually had to handwrite the algorithm to understand what 
      was happening during my implementation. At first, I had my function 
      get the next char in the string even though it was at a leaf, meaning 
      that my string iteration was one step ahead of my tree iteration. It 
      was difficult to keep them in sync throughout the whole looping, 
      especially because I use a while loop where the condition was while
      (text.get(c)). Then I discovered the .putback() function so I used 
      that to keep the two iterations in sync. Another part I found really 
      complex was the end of the file. I'm never completely sure how 
      different istream functions define the end of file, because sometimes 
      they stop one character short. I ran into a lot of bugs because of 
      this whre I would be missing the last character of my resulting 
      decoded text. I got help from a TA to fix this issue. Overall i think 
      it was the most complex algorithm I had to implement.


## G. Testing
  ### PHASE ONE TESTING
      For the three functions in part one (count_freqs, serialize_tree, 
      deserialize_tree) I used assert in unit testing. 
      
      For count freqs, I tested with a mix of a single character and 
      multiple characters, with and without spacing, and with and without 
      newlines. I mized and matched each of these conditions. With the 
      characters, I used both letters and punctuation.

      For serialize_tree and deserialize_tree, I created the tree using the 
      makeFigure1Tree() from ZapUtil.h. Then I serialzed it and asserted it 
      with a string of the correct serializing. With deserialize, I 
      deserialized the serialized tree then used the treeEquals() function 
      to compare my deserialized tree and the given tree from makeFigure1Tree
      ()

  ### PHASE TWO TESTING:
      For phase two testing I used diff. I didn't do as good of a job 
      testing function by function because I found it very difficult to make 
      unit tests for each function because they depended so heavily on 
      eachother. Instead I first finished the encoding section then ran and 
      diffed it with each .txt file given to us. When there was a bug, I 
      started placing checkpoints throughout the function where I though the 
      bug was happening, and then was able to narrow it down to a few lines. 
      Once I discovered where the bug was comging from, I wrote down the 
      functions execution manually so I could see what was happening step by 
      step with the variables and operations. From that I was able to fix 
      any bugs I had. I did the same thing with my decoding function. 
      However, as mentioned above, I'm not quite certain where the istream 
      functions think the end of the file is, and one of my bugs came from 
      it not reaching the end of the file. I ended up getting help from a TA 
      for this. I still used checkpoints and wrote it down manually to 
      isolate the block of code where the bug was coming from. I made sure 
      to diff with nonexistent files too to make sure it worked. I tried to 
      diff decode with an empty file but I wasn't able to create the right 
      kind of empty file so the readFile() function from BinaryIO.h would 
      abort the program.


## I. Hours spent (weeks 1 and 2)  
    About 15.
