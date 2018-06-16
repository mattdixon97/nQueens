# N-Queens Problem
The N-Queens problem is an extension of the eight queens puzzle developed in 1848 by Max Bezzel.
Each queen is placed on a NxN chess board so that no two queens share the same row, column, or
diagonal.

### Solution
The attached Python code takes in a single .txt file which contains a list of integer N values. It generates a
single solution to the N-Queens problem for each N value and outputs all solutions to a single .txt file.
For further specificity about the code function beyond what is outlined below, see comments and code
in nqueens.py.

### Initial Board Generation
Initially, the board is generated using a greedy algorithm. A list is created to represent the board where the list indices represent the column and the value stored at each index represents the row. Three lists are created to keep track of the row conflicts, right diagonal conflicts, and left diagonal conflicts. As a queen is placed in each column these lists are updated. For more information on how these lists are updated refer to Additional Implementation Strategies.
After the lists are intialized, an ordered set of row positions (i.e. numbers 1-N) is created. For each column, the number of conflicts for the first available row in the set are calculated; if are no conflicts, the queen is placed in that row. If there are conflicts, the row value is re-appended to the end of the row set and a new row value is tested. If both row values tested contain conflicts, a temporary value of None is placed in the column. After attempting to place a queen in each column, any columns containing a  None are filled with the remaining row values in order. This ensures the creation of an initial board with fewer conflicts than a randomly generated board in linear, O(n), time. 

### Minimum Conflicts Heuristic
The approach taken to repair the initially generated board is an iterative repair method which uses the minimum conflict heuristic by moving the queen with the largest number of conflicts to a position in the same column where there is the smallest number of conflicts.  
Once a NxN board has been generated, the queen which conflicts with the most other queens is found. For a description of this process see Finding the Maximum Conflicting Queen below. If the queen that is returned has conflicts (i.e. the problem has not yet been solved), the square in this queen’s column which has the least number of conflicts is found. For a description of this process see Finding the Minimum Conflicting Row below. A check is run to ensure the position with the least number of conflicts is not the queen’s current position; if it is not then the queen is moved to the new square. The row, right diagonal, and left diagonal conflict lists are then updated.   
These steps repeat until a solution is found or until N*0.6 maximum conflicting queens have been generated. If N*0.6 maximum conflicting queens have been generated then a new board will be generated and the program will reattempt to find a solution using the minimum conflict heuristic. The value of N*0.6 was chosen as this is the average number of steps required to solve the board using the min-conflicts heuristic. The time complexity for this algorithm is O(n), or linear.

### Finding the Maximum Conflicting Queen
The function findMaxConflictCol() uses the global variables which contain the number of row, right diagonal, and left diagonal conflicts to determine which column on the board contains the queen with the most conflicts. For every column on the board the number of conflicts of the corresponding queen is calculated. All the queens which have the maximum number of conflicts are added to a list and a random one is selected and returned. The iteration through each column and the selection of the conflicting queens is shown in the figure below. The tie-breaking method used to select the conflicting queen is implemented in a similar method to the figure shown below for finding the minimum conflicting row.

### Finding the Minimum Conflicting Row
The function minConflictPos() uses the global variables which contain the number of row, right diagonal, and left diagonal conflicts and a column number to determine which row in the specified column has the lowest number of conflicts. For every position (row) in the column specified, the number of possible conflicts is calculated. All the rows which minimize the number of conflicts are added to a list and a random position is selected. The random, tie-breaking selection, of a minimum conflicting row is shown in the figure below. The iterative method which finds the minConflictRows list is implemented in a similar method to the figure for finding the maximum conflicting queen shown above.

### Additional Implementation Strategies
#### Altering Conflict Values
The function changeConflicts() is used to add or subtract conflicts from a row or diagonal position when moving a queen. If the “val” parameter is +1, a conflict is added; if “val” is -1, a conflict is subtracted. The values stored in the conflict arrays (row_conflicts, diagr_conflicts, diagl_conflicts) represent the number of queens placed in each row or diagonal. If a count of 1 is found, it indicates that only one queen is in that position, and is therefore not in conflict. The sum of the three conflict lists should yield 3 for a specific queen when there are no conflicts as there will be only the selected queen present in the row, right diagonal, and left diagonal . Therefore, when the maximum conflicting column returns a total of 3 conflicts, the board has been solved.

#### Global Variables
The board, dimension, and conflict lists were stored as global variables. This improved the readability of the code. Upon testing it was found that using global variables also slightly improved the efficiency.

#### Special Cases
A special case was found in which an N value of 6 ran significantly slower than other values. In order to fix this issue, a hard-coded correct solution for a 6x6 board is generated if the dimension given is 6. 

#### Buffering the Output Writing
When writing the solution to the output file, we tested a variety of methods and discovered that the time difference was essentially negligible between different techniques. For small solutions, the output printed to file almost immediately. For the largest possible solution, it took approximately 4 seconds in the worst case. The fastest solution from the testing was to write the 1-based solution to the file with a buffer of 64. Note, that the solution returned is 0-based, and becomes 1-based when we write to file.

## Results
Small Value (1000): ~ 0.0 secs  
Medium Value (100,000): ~ 0.1 secs  
Large Value (10,000,000): ~ 60 secs  
