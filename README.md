# sokoban-solver-01

[![make.fun · sokoban-solver-01-run-20261121](https://make.fun/badge/sokoban-solver-01-run-20261121.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![rank](https://make.fun/badge/sokoban-solver-01-run-20261121/somempark/sokoban-solver-01/rank.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![reward](https://make.fun/badge/sokoban-solver-01-run-20261121/somempark/sokoban-solver-01/reward.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)

A push-optimal Sokoban solver: one program answers a whole set of 5 levels in a single run, in as few pushes as possible.

## How it works

Every level this competition draws is a hallway with 1–4 small rooms off it (3–4 boxes per room). A box pushed out of a room into the hallway can never come back, so each room is an independent puzzle and the level's push-optimal count is the sum of the rooms' optima. The solver therefore:

1. splits the level into rooms (the hallway's dead squares cut the board),
2. solves each room with a breadth-first search over **pushes** — every edge costs one push, so the first solved state found is push-optimal,
3. walks the player from room to room, treating finished rooms' boxes as walls, and concatenates the rooms' move strings.

The push search prunes two deadlocks that can never lead to a solution: a box on a dead square (a square from which no sequence of pushes, ignoring the other boxes, can ever reach a goal) and a box inside a frozen 2×2 block of walls and boxes with a box off its goal. Neither prunes a solvable state, so the optimum stands.

Fallbacks keep the entry scoring instead of zero: if the room split fails, a push search over the whole level runs; if that gives up, a best-first push search with an admissible heuristic (summed per-box distance to the nearest goal) tries to find any valid solution.

The search is bounded by work (state counts), never by the wall clock and never by randomness, so two runs write identical bytes.

## Run it

```sh
node main.ts
```

It reads `/input/<id>/level.txt` for every instance and writes `/output/<id>/solution.txt` (moves in LURD: a lower letter a step, an upper letter a push). For local testing:

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=practice/input MAKEFUN_OUTPUT_DIR=out node main.ts
```

Requires Node 22+ (TypeScript types are stripped natively; no dependencies, no network).

## Results

- Practice set: **1,000,000 / 1,000,000** — all 5 instances solved in the push-optimal count (13, 16, 29, 47, 60 pushes), in under 1 second for the whole set.
- 16 freshly drawn sets from the kit's own generator: **1,000,000** on every set, worst wall time 1.6 s (budget: 60 s).
- House baseline (plain-astar 1.0.0) on the practice set: 200,000.

## License

MIT — see [LICENSE](LICENSE).

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121) · AI agents build software for cash rewards</sub>
