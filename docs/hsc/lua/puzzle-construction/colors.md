# Colors

## Color system

The **color system** is an ordered list of [sticker colors](#color). It can be accessed during [puzzle construction](puzzle.md) as `puzzle.colors`.

Colors can be added to the color system explicitly using [`puzzle.colors:add()`](#puzzlecolorsadd) or implicitly using [`puzzle:carve()`](puzzle.md#puzzlecarve).

### Color indexing

The color system can be indexed by the name or index of a color, or by any of its hyperplanes.

```lua title="Examples of color indexing"
-- Gets the color with the name "L"
puzzle.colors.L

-- Gets the first color in the list
puzzle.colors[1]

-- Gets the color with the plane X=1 (oriented outward)
puzzle.colors[plane('x')]
```

### Fields

The color system has no fields.

### Methods

#### `puzzle.colors:add()`

`puzzle.colors:add()` adds a color to the color system. It takes a single argument: a string (which will be the name of the color), or a table with data about the color.

The table has the following keys:

- `name` is an optional string, and will be the name of the color
- `display` is an optional string, and will be the display name of the color
- `default` is an optional string, and will be the default for the color

Names should be short (typically 1-3 uppercase letters), but display names can be longer.

#### `puzzle.colors:set_defaults()`

`puzzle.colors:set_defaults()` sets multiple default colors at once. It takes a [mapping](../common.md#mappings) from [colors](#color) to strings (the new default colors).

#### `puzzle.colors:rename()`

`puzzle.colors:rename()` renames multiple colors at once. See [`:rename()`](../common.md#rename)

#### `puzzle.colors:swap()`

`puzzle.colors:swap()` swaps two colors in the ordering. See [`:swap()`](../common.md#swap)

#### `puzzle.colors:reorder()`

`puzzle.colors:reorder()` changes the ordering of all colors. See [`:reorder()`](../common.md#reorder)

### Operations

- `#puzzle.colors` returns the number of colors that have been defined
- `type(puzzle.colors)` returns `'colorsystem'`
- `tostring(puzzle.colors)`
- `ipairs(puzzle.colors)`

## Color

A **color** is a color that can be assigned to a sticker. Each color has a list of "surfaces," which are hyperplanes where stickers typically have that color (although they are not required to).

When required as input to a function, a color may be specified as a color object, a color index (integer), or a color name (string).

Each color has a **default color**, which is a string used to look up the user's preferences for a color. For example, the default color may be `'red'`, which would default to the user's prefered color named `red`.

### Fields

Colors have the following fields:

- `.name` is the string name of the color
- `.display` is the string name of the color
- `.index` is the integer index of the color in the color system
- `.default` is the string used to look up the default value for the color

Some fields can be written to:

- Writing a new string to `.name` renames the color
- Writing a new string to `.display` changes the display name
- Writing to `.index` reorders the color to that index, shifting the colors in between
- Writing a new string to `.default` changes the default color

All other fields are read-only.

### Methods

Colors have no methods.

### Operations

- `type(color)` returns `'color'`
- `tostring(color)`

[mapping]: ../common.md#mappings
