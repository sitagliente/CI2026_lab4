# CI2026_lab4 –  Klotski

Solve the six instances of `klotski_problems.txt` with path search: find the sequence of moves that brings the
target piece to the exit. Everything is in `lab4.ipynb`.

## Formulation

- **State**: the position (top-left cell) of every piece. Pieces with the same shape are interchangeable, so
  their names are not part of the state: two boards that differ only by swapping two equal pieces are the
  same state.
- **Move**: one piece slides through empty cells (by any number of cells, also around a corner) while the
  others stay still. Each move costs 1. The rule "one cell = one move" is also implemented, for comparison.
- **Goal**: the target covers the exit cells on the bottom edge.

## Algorithms

One generic graph search (priority queue with lazy deletion, path stored as parent links, goal tested on
extraction), the strategies only change the priority:

| Strategy | Priority |
|---|---|
| breadth-first | order of discovery |
| depth-first | reverse order of discovery |
| uniform cost | g (cost so far) |
| greedy best-first | h |
| A* | g + h |

Admissible and consistent heuristics, obtained by relaxing the problem:
- `distance`: the target needs at least 1 move if it is not on the exit (Manhattan distance with one-cell moves);
- `distance + blockers`: plus 1 for every other piece lying on the cells the target must reach.

## Results (one slide = one move)

Each cell shows "moves / expanded states".

| Instance | Shortest solution | Breadth-first | Uniform cost | A* (distance) | A* (distance + blockers) | Greedy best-first | Depth-first |
|---|---|---|---|---|---|---|---|
| p01 | 6 | 6 / 113 | 6 / 113 | 6 / 104 | 6 / 71 | 6 / 12 | 58 / 61 |
| p02 | 11 | 11 / 239 | 11 / 239 | 11 / 223 | 11 / 143 | 14 / 78 | 38 / 63 |
| p03 | 30 | 30 / 2,142 | 30 / 2,142 | 30 / 2,115 | 30 / 2,002 | 45 / 1,506 | 626 / 894 |
| p04 | 81 | 81 / 23,822 | 81 / 23,822 | 81 / 23,792 | 81 / 23,442 | 94 / 8,753 | 3632 / 11,070 |
| p05 | 138 | 138 / 42,473 | 138 / 42,473 | 138 / 42,017 | 138 / 40,387 | 206 / 23,807 | 5500 / 18,234 |
| p06 | no solution | none / 833 | none / 833 | none / 833 | none / 833 | none / 833 | none / 833 |

- Breadth-first, uniform cost and A* always find the shortest solution; depth-first finds very long ones
  (3632 moves for p04); greedy best-first is fast but not optimal.
- With one-cell moves the shortest solutions are 8, 18, 45, 116 and 179 moves. The classic Klotski (p04)
  gives the two known values: 81 moves (slides) and 116 (cells).
- p06 has no solution: the search visits all 833 reachable states and the target never reaches the exit.
- A* saves little over breadth-first: the heuristics only count moves of the target and of the pieces on its goal cells,
  while most moves of a solution move the other pieces.


## Credits

- The instances are from [GiuseppeEsposito98/Lab4-exercises](https://github.com/GiuseppeEsposito98/Lab4-exercises).
- AI assistance: All ideas, design decisions, and core implementations are my own. AI was used to refine the code, improve readability, and enhance the clarity and structure of this README.
