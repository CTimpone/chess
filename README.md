# chess

## Introduction
This is a command line interface version of chess built entirely on Ruby.  It features every aspect of traditional chess save the en passant.  In the command line, chess supports both human and AI players.  Any person can play either a computer or another person, and the computer can also simulate a match against itself.  

## The Code

### Piece Super-Class
Every type of piece inherits from the Piece super-class, in an effort to DRY up the code.  While every piece has its own unique movement logic, there is significant overlap in all other functionality

### Piece Sub-Classes
Due to the unique logic of the piece types, (nearly) every piece has it's own seperate sub-class.  This is where the logic for the actual available moves, castling, and pawn promotion.  Available movement is stored as arrays of position deltas for every piece.  Special considerations are made for pieces that step (knight, king) and pieces that slide (bishop, rook, queen).  The sliding pieces are such that other than the move deltas, their functionality is identical (save the Rooks castling mechanic).  The same is true for stepping pieces, save for special considerations for the king being in check and castling.  Every piece also holds it's own unique unicode character to facilitate superior representation.

### Board
The board holds all of the pieces, of course, and also is used to conduct the actual movement of one piece to another.  It also contains the logic of whether said moves are valid in cases of check, checkmate, and square occupation.  In addition, all aesthetic concerns are taken care of in the board class as well, through an overwrite of the inspect method with the use of the Colorize gem and the unicode characters from the piece sub-classes.
<br>
There are also a number of factory methods that can still be found in this class that were constructed specifically to facilitate the testing of some of the more complex piece interactions.  Finally, although the project lacks a traditional save/load functionality, we also constructed a method which can take a saved board text file and convert it into a actual board, complete with proper piece placement.

### Game
This class handles the actual running of the actual chess match.  It is entirely reliant on every other class to create a functional game.  It is what ends the game using the logic from the board class regarding check and stale mates.  It supports both human and computer players.

### Computer Player
The computer player has a number AI types that it supports.  The first AI is completely random.  The player, in this scenario randomly selects a piece, and then randomly samples that piece's valid moveset.  The second AI is the first in which board evaluation is employed.  At this setting, the AI cycles through all of it's valid moves and applies a tweaked Negamax (<a href="https://chessprogramming.wikispaces.com/Evaluation">read more</a>) evaluation to determine whatever the best move available is.  This AI will not look ahead, which renders the Negamax evaluation incomplete, as it never knows what it will lose after completing its best move.
<br>
The final AI is an iteration of the Negamax evaluation AI.  Once again, the AI cycles through all valid moves, but then, it simulates every possible opposing move against that valid move.  In an effor to DRY up the code, that simulation of the opposing moves actually uses the AI from the simpler Negamax method.  In the opposing move cycle, it determines what the best available move is, and then assigns the Negamax evaluated score based on the entire exchange.  Whichever exchange yields the best result for the initial AI is the move taken.  The branching nature of chess leads to extended computation time even just looking one move ahead, which is why we did not look another move ahead.

### Human Player
Comparatively, the human player simply enters two chess-styled coordinates (which are translated with help of the board) to complete their move.  The first coordinate is the starting piece, and the second is where it is moved.

## Running the Program
To actually run the program, simply clone or download the project, cd into the directory and run ruby game.rb in the command line.  It will prompt for what type of player is desired for each color.  For choosing an AI, smart is the computer without foresight, while grandmaster looks a move ahead.
