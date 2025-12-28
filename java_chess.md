## Project: 2 Player Chess

## Course: Software Engineering (Fall '24)

### The purpose of the program
  The purpose of this program is to implement a two-player game of chess (no graphics). The program takes in a file that states the intitial state of the chest board and a file that lists out the moves of each player and executes the game. The program focuses on using design patterns such as factory, iterator, listener, logger, and singleton patterns. 

### Provided files + description of each file
 - **-.class and -.java** files for each type of chess piece (excluding color) (ex. Bishop.class and Bishop.java)
   - Implements functionality for each chess piece object
 - **-Factory.class and -Factory.java** files for each type of chess piece (excluding color) (ex. BishopFactory.class and BishopFactory.java)
   - Implements functionality for creating new chess piece objects of a specified color
 - **Board.class and Board.java** 
   - Implements functionality for players to add to, remove from, and move pieces around a board object
 - **BoardInternalIterator.class and BoardInternalIterator.java**
   - Implements the iterator design pattern to help with updating the state of the chess board
 - **BoardListener.class and BoardListener.java**
   - Implements the listener design pattern to help with moving pieces around and capturing pieces
 - **BoardPrinter.class and BoardPrinter.java**
   - Allows the players to understand the state of the board since there are no graphics
 - **Chess.class and Chess.java**
   - Executes the game of chess provided by the input file
 - **Color.class and Color.java**
   - Sets piece color constants (BLACK and WHITE)
 - **Logger.class and Logger.java**
   - Updates players on the state of the board after a move or a capture
 - **Piece.class and Piece.java**
   - Registers and creates new chess pieces
 - **PieceFactory.class and PieceFactory.java**
   - Implements factory design pattern to help with creating new chess pieces
 - **Test.class and Test.java**
   - Testing files

 - **layout1**
   - example input file of an initial layout of the chessboard
 - **moves1**
   - example input file of the list of moves for the program to execute

### Bugs
  - Does not throw an exception if you try to add a piece in an occupied spot
  - Does not throw an exception if the file containing the initial layout of the board is empty
