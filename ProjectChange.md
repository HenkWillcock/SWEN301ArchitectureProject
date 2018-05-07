## 1 Change Proposals

### 1.1 Simplify Movement Class

The purpose of this change is to simplify the cluttered 'Movement' class by moving functionality out of it into other classes, mostly the concrete 'Piece' classes (Knight, Rook, Bishop, etc.). This change would be valueable to the program because it would substantially reduce the complexity of its biggest classes. 

### 1.2 Build Full Model-View-Controller Architecture

Right now the graphics handler handles the control of the program, making it quite a large and complex class. Adding a controller class to the program and simplifying the graphics handler to just handle the graphics and nothing else would subtantially reduce the complexity of this class. It would also mean the program uses a well-known architecture making it easier for other developers to develop ChessMaster.

### 1.3 Chosen Change

Out of these options, the best in my opionion is the first option of simplifying the 'Movement' class. There are pros and cons of the others but I think the 'Movement' class is so complex that reducing its complexity will be very valueable to the program. I also don't think this change will be too difficult to implement.

## 2 Description

The methods which return lists of potential moves for example `getKnightMoves(Knight knight)`, should be moved to their corresponding class. The 'Piece' class will then be given an abstract method `getMoves()`. This method will then be implemented differently in each class which extends 'Piece'. The `movesInDir()` method which is passed a piece and a vector and returns all the moves available in that particular direction, will be moved to the Piece class to be used by all the pieces. The middle management method `recomputeMoves()` then become unnecessary with this change, further reducing complexity.

If the above change can be completed, then I also want to move the methods which perform special moves out of the 'Movement' class. These methods are `canCastle()`, `getCastlingMove()` and `promotePawn()`. The `canCastle()` and `getCastlingMove()` methods will be incorporated into the King's `getMoves()` method as a special type of move. And `promotePawn()` will be incorporated into the Pawn's `getMoves()` method.

If these changes can both be completed I'd also like to look into changing how the game checks for checkmate. At first glance it seems like the game handles this very poorly with lots of spaghetti code and bugs. I won't spend time looking into this until I've completed one of the other changes first.

## 3 Evaluation
