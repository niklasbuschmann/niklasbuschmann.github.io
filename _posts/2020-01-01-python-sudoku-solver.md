---
title: "Python: Simple Sudoku solver"
date: 2020-01-01
description: A simple backtracking algorithm for solving Sudokus with Python
layout: post
---

Solving Sudokus with Python is surprisingly easy: One can simply start with an empty square and guess a number that doesn't appear in the corresponding row, column and square and then recursively do this again for the next empty square until either there is no square left or one hits a square with no possible candidates. In the latter case one of the previous guesses must have been wrong and the recursion should go back one step and try the next valid number for that square.


So a simple Python implementation could look like this:

```python
def solve(board):
    if position := next(((x, y) for x in range(9) for y in range(9) if not board[x][y]), None):
        x, y = position
        found = [board[i][j] for i in range(9) for j in range(9) if i == x or j == y or (x//3)*3 <= i <= (x//3)*3+2 and (y//3)*3 <= j <= (y//3)*3+2]
        for candidate in set(range(1, 10)) - set(found):
            board[x][y] = candidate
            yield from solve(board)
        board[x][y] = 0
    else:
        yield board
```

Using this to solve the [world's hardest sudoku](https://www.telegraph.co.uk/news/science/science-news/9359579/Worlds-hardest-sudoku-can-you-crack-it.html) takes around 40000 wrong guesses before finding the correct solution.
