---
title: "Python: Simple Sudoku Solver"
date: 2022-04-09
description: A simple backtracking algorithm for solving Sudokus with Python
---

Solving Sudokus with Python is surprisingly easy: One can simply start with an empty square and guess a number that doesn't appear in the corresponding row, column and subgrid and then recursively do this again for the next empty square until either there is no square left or one hits a square with no possible candidates. In the latter case one of the previous guesses must have been wrong and the recursion should go back one step and try the next valid number for that square.

So a simple Python implementation could look like this:

```python
def solve(board):
    if not get_empty_square(board):
        print('Done')
        quit()
    pos_x, pos_y = get_empty_square(board)
    for candidate in range(1, 10):
        if not conflicts(board, pos_x, pos_y, candidate):
            board[pos_x][pos_y] = candidate
            solve(board)
    board[pos_x][pos_y] = 0
```

To implement `get_empty_square` one can use the generator expression `((i, j) for i in range(9) for j in range(9) if not board[i][j])` that allows one to iterate over all empty squares. Python's `next()` function allows one to take the first empty square found or return a default argument if there is no empty square. The code could now look like this:

```python
def solve(board):
    empty_squares = ((i, j) for i in range(9) for j in range(9) if not board[i][j])
    pos_x, pos_y = next(empty_squares, (None, None))
    if (pos_x, pos_y) == (None, None):
        print('Done')
        quit()
    for candidate in range(1, 10):
        if not conflicts(board, pos_x, pos_y, candidate):
            board[pos_x][pos_y] = candidate
            solve(board)
    board[pos_x][pos_y] = 0
```

Now to check whether a number can be placed on an empty square without conflicts one has to check the values of 8 + 8 + 4 = 20 adjacent squares of corresponding the row, column and subgrid. Since the adjacent positions don't change one can precompute them for every square using the following 9x9 array:

```python
adjacent_squares = [[
        [(pos_x, y) for y in range(9) if y != pos_y] +
        [(x, pos_y) for x in range(9) if x != pos_x] +
        [(x, y) for x in range((pos_x // 3) * 3, (pos_x // 3) * 3 + 3) for y in range((pos_y // 3) * 3, (pos_y // 3) * 3 + 3) if x != pos_x and y != pos_y]
    for pos_y in range(9)] for pos_x in range(9)]
```

Again using the generator expression `conflicts = ((x, y) for x, y in affected_squares[pos_x][pos_y] if board[x][y] == candidate)` one can generate a list of conflicting squares for each candidate and use `next()` to check whether there exists one. The final code looks like this:

```python
adjacent_squares = [[
        [(pos_x, y) for y in range(9) if y != pos_y] +
        [(x, pos_y) for x in range(9) if x != pos_x] +
        [(x, y) for x in range((pos_x // 3) * 3, (pos_x // 3) * 3 + 3) for y in range((pos_y // 3) * 3, (pos_y // 3) * 3 + 3) if x != pos_x and y != pos_y]
    for pos_y in range(9)] for pos_x in range(9)]

def solve(board):
    empty_squares = ((i, j) for i in range(9) for j in range(9) if not board[i][j])
    pos_x, pos_y = next(empty_squares, (None, None))
    if (pos_x, pos_y) == (None, None):
        print(*board, sep='\n')
        quit()
    for candidate in range(1, 10):
        conflicts = ((x, y) for x, y in adjacent_squares[pos_x][pos_y] if board[x][y] == candidate)
        if not next(conflicts, False):
            board[pos_x][pos_y] = candidate
            solve(board)
    board[pos_x][pos_y] = 0
```

Using this to solve the [world's hardest sudoku](https://www.telegraph.co.uk/news/science/science-news/9359579/Worlds-hardest-sudoku-can-you-crack-it.html) takes around 0.3s on a 2020 MacBook and 40000 wrong guesses.
