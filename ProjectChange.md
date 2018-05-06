## 1 Change Proposals

### Simplify Movement Class

The purpose of my change is to simplify the cluttered 'Movement' class by moving functionality out of it into other classes, mostly the concrete 'Piece' classes (Knight, Rook, Bishop, etc.). This change would be valueable to the program because it would substantially reduce the complexity of its biggest classes. 

### Build Full Model-View-Controller Architecture

Right now the graphics handler handles the control of the program, making it quite a large and complex class. Adding a controller class to the program and simplifying the graphics handler to just handle the graphics and nothing else would subtantially reduce the complexity of this class. It would also mean the program uses a well-known architecture making it easier for other developers to develop ChessMaster.

## 2 Description

The methods which return lists of potential moves for example `getKnightMoves(Knight knight)`, should be moved to their corresponding class. The 'Piece' class will then be given an abstract method `getMoves()`. This method will then be implemented differently in each class which extends 'Piece'. The useful `movesInDir()` method could then be moved to the Piece class to be used by all the pieces. The `recomputeMoves()` and `getAllMoves()` methods then become unnecessary with this change.

The `canCastle()` and `getCastlingMove()` methods will then be incorporated into the King's `getMoves()` method as a special type of move.

## 3 Evaluation
