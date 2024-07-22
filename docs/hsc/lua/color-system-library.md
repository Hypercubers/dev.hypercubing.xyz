# Color system library

The **color system library** is the database of all known color systems that are shared by multiple puzzles. The global variable `color_systems` contains the global color system library.

A **color scheme** is a mapping from puzzle color (`R`, `U`, etc.) to default color (`Red`, `White`, etc.). A **color system** is a set of color names and at least one color scheme.

## Methods

### `color_systems:add()`

`color_systems:add()` adds a color system to the global color system library. It takes two arguments: the unique string ID for the color system (which may be the same as a puzzle ID), and a table containing the color system data with the following keys:

- `name` is a string containing the user-facing name of the puzzle
- [`colors`](#colors) is a sequential table of color names and display names in order
- `schemes` is a sequential table of [color scheme definitions](#color-scheme-definitions)
- `default` is either a [color scheme definition](#color-scheme-definitions) or a string the name of a color scheme defined in `schemes`

The key `colors` is required. All other keys are optional.

To use a color system in a puzzle, set the puzzle's `colors` key to the string ID of the color system. The colors of the puzzle must exactly match its color system.

#### `colors`

`colors` is a sequential table, where each element is a table with the following keys:

- `name` is a string containing a name for the color (e.g., `R`)
- `display` is a string containing a user-facing display name for the color (e.g., `Right`)
- `default` is a string containing a [default color](#default-colors).

The key `name` is required. All other keys are optional. `default` is only allowed when `schemes` and `default` are `nil`.

#### Color scheme definitions

A **color scheme definition** is a table mapping puzzle colors to default colors. Each key is a string containing a color name, and the corresponding value is a string representing a default color. Each puzzle color must be assigned a unique default color.

#### Default colors

A **default color** is a string corresponding to a color from the global color palette. To get the name of a default color, mouse over it in the **color scheme** panel in Hyperspeedcube.

Colors from color sets end with a number in square brackets. For example, `Red Traid [3]` is the third color from the "Red Triad" set.

Colors from gradients end with two numbers in square brackets separated by a slash, where the first number is the index within the gradient and the second number is the total number of colors assigned to the gradient. For  `Rainbow [3/5]` is the third color in the rainbow when five colors have been assigned to it.

## Operations

- `#puzzles` returns the number of puzzles that have been loaded
- `type(puzzles)` returns `'puzzledb'`

## Examples

```lua title="Standard Rubik's cube color system"
color_systems:add('cube', {
  name = "Cube",

  colors = {
    { name = 'R', display = "Right" },
    { name = 'L', display = "Left" },
    { name = 'U', display = "Up" },
    { name = 'D', display = "Down" },
    { name = 'F', display = "Front" },
    { name = 'B', display = "Back" },
  },

  schemes = {
    {"Western", {
      R = "Red",
      L = "Orange",
      U = "White",
      D = "Yellow",
      F = "Green",
      B = "Blue",
    }},
    {"Japanese", {
      R = "Red",
      L = "Orange",
      U = "White",
      D = "Blue",
      F = "Green",
      B = "Yellow",
    }},
  },

  default = "Western",
})
```

```lua title="Standard Rubik's hypercube color system"
color_systems:add('hypercube', {
  name = "Hypercube",

  colors = {
    { name = 'R', display = "Right", default = "Red" },
    { name = 'L', display = "Left",  default = "Orange" },
    { name = 'U', display = "Up",    default = "White" },
    { name = 'D', display = "Down",  default = "Yellow" },
    { name = 'F', display = "Front", default = "Green" },
    { name = 'B', display = "Back",  default = "Blue" },
    { name = 'O', display = "Out",   default = "Pink" },
    { name = 'I', display = "In",    default = "Purple" },
  },
})
```
