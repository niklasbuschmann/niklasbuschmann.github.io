---
title: "Python: Simple Sudoku solver"
date: 2020-01-01
description: A simple backtracking algorithm for solving Sudokus with Python
layout: post
---

Solving Sudokus with Python is surprisingly easy: One can simply start with an empty square and guess a number that doesn't appear in the corresponding row, column and square and then recursively do this again for the next empty square until either there is no square left or one hits a square with no possible candidates. In the latter case one of the previous guesses must have been wrong and the recursion should go back one step and try the next valid number for that square.


So a simple Python implementation could look like this:

```python
def next_empty_square(board):
    empty_squares = ((x, y) for x in range(9) for y in range(9) if not board[x][y])
    return next(empty_squares, (None, None))

def get_candidates(board, pos_x, pos_y):
    found = [board[x][y] for x in range(9) for y in range(9) if x == pos_x or y == pos_y or (pos_x//3)*3 <= x <= (pos_x//3)*3+2 and (pos_y//3)*3 <= y <= (pos_y//3)*3+2]
    return set(range(1, 10)) - set(found)

def solve(board):
    pos_x, pos_y = next_empty_square(board)
    if (pos_x, pos_y) == (None, None):
        return board
    for candidate in get_candidates(board, pos_x, pos_y):
        board[pos_x][pos_y] = candidate
        if solve(board):
            return board
    board[pos_x][pos_y] = 0
```

Using this to solve the [world's hardest sudoku](https://www.telegraph.co.uk/news/science/science-news/9359579/Worlds-hardest-sudoku-can-you-crack-it.html) takes around 40000 wrong guesses before finding the correct solution.
