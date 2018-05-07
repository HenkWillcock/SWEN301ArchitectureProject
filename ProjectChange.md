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

The original change of simplifying the movement I wanted to do proved to be too difficult. The code is simply too coupled to itself and it was too difficult to extract the required information out of the poorly written 'Movement' class. The first problem I ran into is that Pieces don't have a field for their location. Instead to get this information you have to use a map stored only in the 'Movement' class. I first attempted to add a field for the piece's location, but to get this to work I had to make modifications to many other parts of the program until it ended up being counter productive, I had only succeeded in making the program even more complex. 

The second workaround I tried was to give the pieces a reference to the 'Movement' class, so they could use it's Map to figure out their location on the board. This also proved to be more difficult than it first appeared. It was difficult when initialising the program to give the pieces references to the 'Movement' object because of some hard to explain design choices. I finally did get this too work but soon discovered more issues, mostly with the `movesInDir()` method which also required fields only present in the 'Movement' class such as a reference to the board.

I soon realised that even if I got the change to work, it wouldn't reduce complexity in the way I had hoped. I then decided to start from scratch on a new change, the second one I proposed, adding a controller class and taking that functionality away from the 'GraphicsHandler' object.

## 4 Second Description 

## 5 Second Evaluation

This change was relatively easy to implement, and it mostly achieved its goal of taking functionality off the GraphicsHandler class so that all it did was handle the graphics. I wasn't able to move the actual mouse listener out of the GraphicsHandler class however, it had to be in this class because it is the JFrame which draws everything which also has the focus in this program. The clicked method in the GraphicsHandler now just calls the clicked event in the Controller.


