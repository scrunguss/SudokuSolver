# **Sudoku Solver - Approach**

I did not reference any solvers online, I structured my approach off the eight-queens problem described on moodle. 

I decided on a backtracking with constraint satisfaction algorithm, and I began to approach the problem by first creating a good model to represent the state of the board.

## **Board Model**

#### Board Checker
Initially, since I did not fully understand how to solve this problem, I started off writing a function (isSolved) to detect whether the board was in a solved state. 

#### Implementation
This was done by checking for 0's, and then checking whether each row/column/square(3x3 section) did not have any duplicate elements.

#### Tracking Squares
I then needed to implement a function to find the 3x3 sections (hereafter referred to as 'squares') of the board, as this was necessary for the isSolved function. 

#### Implementation
This was done by iterating over the board at an increment of 3, and then populating an array with tuples of both the square array, and an array of the 4 indexes that defined the bound of the respective square (this would become useful later). Later on, I discovered that I would need an accompanying function to this called locateSquareOfPos, to find the square for a given cell in the board.

#### Tracking Unassigned Variables and Possible Values
The final component I needed to complete my board model (or board state representation) was a way of tracking which variables (cells) in the board were unassigned, and which values were possible in these. 

#### Implementation
In function findRemaining, I decided to keep an array 'r' which contained 3 subarrays for squares, columns and rows. These subarrays were lists of arrays again, with each of them containing the remaining valid numbers for their respective square/col/row. The structure of 'r' is like so : 
[[9],[9],[9]], where [9] is either square/col/row. SO an example of a subarray [9] = [[1,2],[6,8],[3],[3,7,1],..(5 more arrays)].
Each of the lowest level arrays is populated with a lambda function to filter an array containing numbers 0 to 9, where the values that were not in that row/col/square were returned.

Once I had my model of the board, I could begin to write an algorithm to solve it.

## **Solving Algorithm**

#### Backtracking 
Before writing my functions to pick new positions on the board and populate them, I started off with backtracking. In retrospect, it would've made more sense to implement the former functions first, since the easier sudokus can be solved without backtracking, however I did not know about this initially. 

#### Implementation
 My initial naiive approach was to keep stack of board states, where every change to the board made by the picker/chooser functions was appended to this list. Once a local maximum had been reached (i.e. the board was in an impossible state), I would backtrack to the previous board state. 

 I quickly discovered that this did not work, since I had no way of tracking whether a new state was unique or not, and whether it had been tried before. The solver would quickly get into infinite loops on the harder puzzles. 

 To remedy this, I instead implemented it with 3 arrays; an array of states, a visited array (held boolean values), and a set. 
 The same index is used for both the visited and state arrays. When a new state is returned by the picker/chooser function, the state is first hashed and added to the set. If the set addition succeeds, it is then appended to the states array, and 'visited' is appended with a 'False' value. 

 When we need to backtrack, the element at the current index in the visited array is set to True. the new index is set by finding the last occurring 'False' value in the 'visited' array, and setting the current board to the corresponding state at the same index of the 'states' array. 

 #### Finding the Next Position
I decided at the time that the best way to populate the board, was to have one function handling finding new positions, and another function to choose the value to enter there. These are what I have been referring to as the 'picker/chooser' functions respectively. 
There were significant limitations to this design choice, which I will discuss later. 




