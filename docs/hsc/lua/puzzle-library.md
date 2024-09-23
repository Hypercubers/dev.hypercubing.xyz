# Puzzle library

The **puzzle library** is the database of all known puzzle definitions. The global variable `puzzles` contains the global puzzle library.

## Methods

### `puzzles:add()`

`puzzles:add()` adds a puzzle to the global puzzle library. It takes one argument: a table containing the following keys:

- `id` is a string containing a unique ID for the puzzle
- `name` is a string containing a user-facing name for the puzzle
- `version` is a string containing the [semantic version](../versioning.md) for the puzzle
- `ndim` is the number of dimensions for the puzzle
- `build` is a function used to construct the puzzle
- `aliases` is a sequential table of alternate string names for the puzzle
- `meta` is a table containing metadata about the puzzle and its file.
- `properties` is a table containing additional information about the puzzle's properties.
- `colors` is a string containing the ID of a color system in the [color system library](color-system-library.md)

The keys `ndim` and `build` are required. All other keys are optional. The keys `name` and `colors` are recommended.

The puzzle is not constructed immediately when `puzzles:add()` is called. (Otherwise, the program would slow to a crawl loading all the puzzles on startup.) Instead, the ID and table are stored, and the puzzle is constructed when the user first opens it.

To construct a puzzle, a [puzzle](puzzle-construction/puzzle.md) object is initialized with a new `ndim`-dimensional space, and an empty color system, axis system, and twist system; `build` is then called with that puzzle as its argument. `build` is responsible for cutting the pieces and defining colors, axes, and twists. See [Puzzle](puzzle-construction/puzzle.md), [Colors](puzzle-construction/colors.md), [Axes](puzzle-construction/axes.md), and [Twists](puzzle-construction/twists.md) for more.

## Operations

- `#puzzles` returns the number of puzzles that have been loaded
- `type(puzzles)` returns `'puzzledb'`
