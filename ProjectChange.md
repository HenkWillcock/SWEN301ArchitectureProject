## 1 Change Proposals

### 1.1 Simplify Movement Class

The purpose of this change is to simplify the cluttered `Movement` class by moving functionality out of it into other classes, mostly the concrete `Piece` classes (Knight, Rook, Bishop, etc.). This change would be valueable to the program because it would substantially reduce the complexity of its biggest classes. 

### 1.2 Build Full Model-View-Controller Architecture

Right now the `GraphicsHandler` object handles the control of the program, making it quite large, and unnecessarily complex. Adding a `Controller` class to the program and reducing the `GraphicsHandler` to just handle the graphics would subtantially reduce the complexity of this class. It would also mean the `GraphicsHandler` name is actually a good description for the functionality of the object. On top of this, it would mean the program uses a well-known architecture (MVC), making ChessMaster easier to develop in the future.

### 1.3 Insert 3rd Proposal



## 2 Simplifying 'Movement' Class

Out of the proposed changes to ChessMaster, the best in my opionion is the first option of simplifying the `Movement` class. There are pros and cons of the others but I think the `Movement` class is so complex that reducing its complexity will be very valueable to the developers of ChessMaster. I also don't think this change will be too difficult to implement.

### 2.1 Planning

To move functionality out of the `Movement` class and into the `Piece` classes seems relatively simple, however it seems likely I'll run into complications because of spaghetti code. The methods which return lists of potential moves for example `getKnightMoves(Knight knight)`, will be moved to their corresponding class. The `Piece` class will then be given an abstract method `getMoves()`. This method will then be implemented differently in each class which extends `Piece`. The `movesInDir()` method which is passed a piece and a vector and returns all the moves available in that particular direction, will be moved to the Piece class to be used by all the pieces. The middle management method `recomputeMoves()` then become unnecessary with this change, further reducing complexity.

Using agile software development techniques, I'll attempt to do this change to the program without a highly detailed plan. If it's discovered that there are issues with my high-level plan, revisions will be made, and workarounds will be used as necessary.

### 2.2 Extra Goals

If the above change can be completed, then I also want to move the methods which perform special moves out of the `Movement` class. These methods are `canCastle()`, `getCastlingMove()` and `promotePawn()`. The `canCastle()` and `getCastlingMove()` methods will be incorporated into the King's `getMoves()` method as a special type of move. And `promotePawn()` will be incorporated into the Pawn's `getMoves()` method.

If these changes can both be completed I'd also like to look into changing how the game checks for checkmate. At first glance it seems like the game handles this very poorly with lots of spaghetti code and bugs. I won't spend time looking into this until I've completed one of the other changes first.

### 2.3 After Completion

The original change of simplifying the `Movement` class I wanted to do proved to be too difficult. The code is simply too coupled to itself and it was too difficult to extract the required information out of the poorly written 'Movement' class. The first problem I ran into is that Pieces don't have a field for their location. Instead to get this information you have to use a map stored only in the `Movement` class. I first attempted to add a field for the piece's location, but to get this to work I had to make modifications to many other parts of the program until it ended up being counter productive, I had only succeeded in making the program even more complex. 

The second workaround I tried was to give the pieces a reference to the `Movement` class, so they could use it's map to figure out their location on the board. This also proved to be more difficult than it first appeared. It was difficult when initialising the program to give the pieces references to the `Movement` object because the pieces are created by the `Board` object, not the `Main` object. This seems like it might not be a problem because the `Board` has a reference to a `Movement` class, however it's not the same `Movement` object as the one stored by `Main`, the one with the map of pieces on it. I finally did get this to work, but soon discovered more issues. One such issue was with the `movesInDir()` method, which also required fields only present in the `Movement` class such as a reference to the `Board`.

I soon realised that even if I got the change to work, it wouldn't reduce complexity in the way I had hoped. I then decided to start from scratch on a new change, the second one I proposed, adding a controller class and taking that functionality away from the `GraphicsHandler` object.

## 3 Building Model View Controller

After the first attempt at a change failed a second option was completed instead. This second changed was to add a `Controller` class to the program and complete a full Model-View-Controller architecture.

### 3.1 Planning

The main goal here is to move the `MouseHandler` out of the `GraphicsHandler` and into a new class, `Controller`. After this all the methods associated with the `MouseHandler` will also be moved to this new class. The first of these methods is `clicked()`, which handles all the click events produced by the `MouseHandler`. This method in turn calls the methods `check()`, `checkMate()`, and `isUnderCheck`, so these will also be moved to the new `Controller` class.

It is also logical to move the `AI` field out of the `GraphicsHandler` into the `Controller`, ideally this would be handled by another new class, but time is limited. The `AI` class in ChessMaster actually clicks the board like a human player would to move pieces. This seems like a poor way of handling things, but to change it would be very time consuming. One possible solution here would be for the `AI` class to simply return the best move, and the actual execution of the move is handled by the `Main` class which keeps track of turns. After moving the `AI` field into the `Controller`, the methods associated with it will also be moved. These methods are `setGameMode()` and `setAI()`.

### 3.2 After Completion

This change was relatively easy to implement, and it achieved its goal of taking functionality away from the `GraphicsHandler` class. Now all the `GraphicsHandler` does is handle the graphics. To give some numbers on the simplification of `GraphicsHandler`, it was previously 280 lines of code with 6 methods. Now it's 150 lines of code with just 1 method.

The way I implemented the `Controller` class was to give it a reference to `GraphicsHandler`. This allowed it to add a `MouseListener` to `GraphicsHandler`, meaning it returned mouse coordinates on the board, rather than some other component. The reference to `GraphicsHandler` is also used to update the graphics. This does mean the architecture is not a full model view controller. In a proper MVC it should be the model which updates the view, not the controller. However this fact didn't seem to be very important as the change still reduced complexity significantly. All the JUnit tests still pass with the change, and the program seems to work as normal. While this doesn't definitely mean I haven't created any bugs, it's obviously a good sign.

### 3.3 Evaluation



## 4 Splitting Up paintComponent() Method

With the `GraphicsHandler` class simplified, it opened the opportunity to split up the large `paintComponent()` method. This method is very large at almost 100 lines of code, and it seems relatively straight forward to extract some methods out of it and have them as private methods inside the `GraphicsHandler`.

### 4.1 Description

First, methods were created for drawing the axis labels of the chess board. Previously these were meshed in with the drawing of the board's cells making the code difficult to understand. Second, I went through and removed useless comments which did nothing but repeat the code underneath them. Next, I created the methods `drawWhiteCell()`, `drawGreyCell()`, `highlightCellIfNecessary()`, and `drawPiece()`. Compared with the original method, these are very concise, easy to understand methods with descriptive titles. These methods were used to build the `drawCompleteCell()` method, and only this method was used in the `paintComponent()` method.

### 4.2 Evaluation

The `paintComponent()` method is now only 8 lines long and is very readable, its complexity being abstracted away by these new methods. It's also worth noting that adding these methods didn't add a subtantial amount of code. In fact, the total lines of code is slightly less than before because of the comments I removed.

Splitting up the `paintComponent()` method will make future changes to the `GraphicsHandler` class much easier. This is because developers will be able to find the specific method they want to change and change it. Whereas before they would be navigating through spaghetti code while worrying about unintentionally breaking the whole thing.The `paintComponent()` method is now only 8 lines long and is very readable, its complexity being abstracted away by these new methods. It's also worth noting that adding these methods didn't add a subtantial amount of code. In fact, the total lines of code is slightly less than before because of the comments I removed.

Splitting up the `paintComponent()` method will make future changes to the `GraphicsHandler` class much easier. This is because developers will be able to find the specific method they want to change and change it. Whereas before they would be navigating through spaghetti code while worrying about unintentionally breaking the whole thing.

