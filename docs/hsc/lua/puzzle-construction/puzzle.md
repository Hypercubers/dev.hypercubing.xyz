# Puzzle

When [defining a puzzle](../puzzle-library.md#puzzlesadd), the `build` function takes a single argument of type `puzzle`. The puzzle is initialized with a single infinite piece; this piece must be [carve()](#puzzlecarve)ed to make it finite in order to have a valid puzzle. It can then have [twist axes](axes.md#puzzleaxesadd) and [twists](twists.md#puzzletwistsadd) defined.

## Fields

Puzzles have the following fields:

- `.id` is the unique ID of the puzzle
- `.space` is the [space](../geometry/space.md) in which the puzzle is being constructed
- `.ndim` is the number of dimensions of the space in which the puzzle is being constructed
- `.colors` is the [color system](colors.md) of the puzzle
- `.axes` is the [axis system](axes.md) of the puzzle
- `.twists` is the [twist system](twists.md) of the puzzle

## Methods

### `puzzle:carve()`

`puzzle:carve()` cuts all pieces in the puzzle and discards pieces that are "outside" the cut. It takes two arguments: the [plane](../geometry/hyperplane.md) along which to cut, and an optional table containing additional arguments.

The orientation of the cutting plane determines which pieces are kept or discarded; the normal vector of the plane points toward pieces that will be discarded.

If the cutting plane is an [orbit](../geometry/orbit.md) of [planes](../geometry/hyperplane.md) instead of just a single one, then `puzzle:carve()` will perform a cut for each plane in the orbit. If the orbit has [names assigned](../geometry/orbit.md#orbitnamed), then the new colors created by the cut will be assigned those names.

The table may contain one optional key:

- `stickers` is an optional value which defaults to `true`

If `stickers` is `nil` or `true`, then `carve()` will add a new color for each cut and create stickers along the cut; if `stickers` is `false`, then `carve()` will leave the peices unstickered.

If `stickers` is [color](colors.md#color) or a string containing the name of a color, `carve()` will create stickers along the cut and assign that color to them.

If `stickers` is a table, then each key is a string containing the name of an element in the orbit and each value is the color to assign to the stickers along the cut. If the table is missing a key, then no stickers are created.

A color may be specified as a [color](colors.md#color) value or as a string containing the name of a color. If there is no color with the given name, then one will be created.

!!! warning "Unstickered pieces"

    If a piece is visible from the outside of the puzzle, it should be stickered, using an extra color if necessary.

    Some puzzles with real-world designs leave faces of pieces unstickered; this is not adivised in Hyperspeedcube. It is much better to sticker them using an extra color, and let the user hide them if they would like to.

```lua title="Examples using puzzle:carve()"
local sym = cd'bc3'

-- Carve at X=1, deleting everything with X>1
puzzle:carve(plane('x'))

-- Carve at X=1, deleting everything with X<1
puzzle:carve(-plane('x'))

-- Carve a cube
puzzle:carve(sym:orbit(sym.oox.unit))

-- Carve an unstickered octahedron
puzzle:carve(sym:orbit(sym.xoo.unit), {stickers=false})
```

### `puzzle:slice()`

`puzzle:slice()` cuts all pieces in the puzzle. It takes one argument: the [plane](../geometry/hyperplane.md) along which to cut.

The orientation of the cutting plane is ignored.

If the cutting plane is an [orbit](../geometry/orbit.md) of [planes](../geometry/hyperplane.md) instead of just a single one, then `puzzle:carve()` will perform a cut for each plane in the orbit.

```lua title="Examples using puzzle:slice()"
local sym = cd'bc3'

-- Slice at X=1, keeping all resulting pieces
puzzle:slice(plane('x'))

puzzle:slice(sym:orbit('x'))
```

## Operations

Puzzles support the following operations:

- `type(puzzle)` returns `'puzzle'`
