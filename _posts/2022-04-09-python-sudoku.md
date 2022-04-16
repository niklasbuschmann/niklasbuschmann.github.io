---
title: "Python: Simple Sudoku Solver"
date: 2022-04-09
description: A simple backtracking algorithm for solving Sudokus with Python
---

Solving Sudokus with Python is surprisingly easy: One can simply start with an empty square and guess a number that doesn't appear in the corresponding row, column and subgrid and then recursively do this again for the next empty square until either there is no square left or one hits a square with no possible candidates. In the latter case one of the previous guesses must have been wrong and the recursion should go back one step and try the next valid number for that square.

So a simple Python implementation could look like this:

```python
def solve(board):
    pos_x, pos_y = next_empty_square(board)
    if (pos_x, pos_y) == (None, None):
        return board
    candidates = get_candidates(board, pos_x, pos_y)
    for candidate in candidates:
        board[pos_x][pos_y] = candidate
        if solve(board):
            return board
    board[pos_x][pos_y] = 0
```

To implement `next_empty_square()` one can use the generator expression `empty_squares` to iterate over all empty squares, and use Python's `next()` function to take the first entry.

```python
def solve(board):
    empty_squares = ((x, y) for x in range(9) for y in range(9) if not board[x][y])
    pos_x, pos_y = next(empty_squares, (None, None))
    if (pos_x, pos_y) == (None, None):
        return board
    candidates = get_candidates(board, pos_x, pos_y)
    for candidate in candidates:
        board[pos_x][pos_y] = candidate
        if solve(board):
            return board
    board[pos_x][pos_y] = 0
```

 `get_candidates()` can be implemented by collecting the digits that already occur in the adjacent squares of the current row, column and subgrid:

```python
def solve(board):
    empty_squares = ((x, y) for x in range(9) for y in range(9) if not board[x][y])
    pos_x, pos_y = next(empty_squares, (None, None))
    if (pos_x, pos_y) == (None, None):
        return board
    adjacent_values = [board[pos_x][y] for y in range(9)] + [board[x][pos_y] for x in range(9)] + [board[x][y] for x in range((pos_x//3)*3, (pos_x//3)*3+3) for y in range((pos_y//3) * 3, (pos_y//3)*3+3)]
    for candidate in set(range(1, 10)) - set(adjacent_values):
        board[pos_x][pos_y] = candidate
        if solve(board):
            return board
    board[pos_x][pos_y] = 0
```

Using this to solve the [world's hardest sudoku](https://www.telegraph.co.uk/news/science/science-news/9359579/Worlds-hardest-sudoku-can-you-crack-it.html) takes around 0.25s on a 2020 MacBook and 40000 wrong guesses.
