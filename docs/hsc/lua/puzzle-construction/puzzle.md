# Puzzle

When [defining a puzzle](../puzzle-library.md#puzzlesadd), the `build` function takes a single argument of type `puzzle`. The puzzle is initialized with a single infinite piece; this piece must be [carve()](#puzzlecarve)ed to make it finite in order to have a valid puzzle. It can then have [twist axes](#puzzleadd_axes) and [twists](axes.md#axesadd_twist) defined.

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

If the cutting plane is an [orbit](../geometry/orbit.md) of [planes](../geometry/hyperplane.md) instead of just a single one, then `puzzle:carve()` will perform a cut for each plane in the orbit. If the orbit has [names assigned](../geometry/orbit.md#orbitwith), then the new colors created by the cut will be assigned those names.

The table may contain one optional key:

- `stickers` is an optional boolean which defaults to `true`

If `stickers` is `true`, then `carve()` will add a new color for each cut and create stickers along the cut; if `stickers` is `false`, then `carve()` will leave the peices unstickered.

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

### `puzzle:add_axes()`

`puzzle:add_axes()` adds a symmetric set of [twist axes](axes.md), optionally adding layers to those axes and slicing the puzzle. It takes two arguments: an [orbit](../geometry/orbit.md) of vectors to add as new twist axes, and an optional table containing additional arguments.

The table may contain up to two optional keys:

- `layers` is an optional sequential table of distances from the origin at which to add layer boundaries
- `slice` is an optional boolean which defaults to `true`

If the table contains a sequence, then that sequence is used in place of `layers`. In this case the `slice` key is still used as normal.

If `layers` is specified, then `add_axes()` will automatically add [layers](layers.md) to each axis it creates.

If `slice` is `true`, then `add_axes()` will automatically [slice](#puzzleslice) all pieces at the layer boundaries specified by `layers`.

```lua title="Examples using puzzle:add_axes()"
local sym = cd'bc3'

-- Add cubic axes with no layers
puzzle:add_axes(sym:orbit(sym.oox.unit))

-- Add cubic axes with a single cut through the origin
puzzle:add_axes(sym:orbit(sym.oox.unit), {0})

-- Add cubic axes with 3x3x3-like cuts
puzzle:add_axes(sym:orbit(sym.oox.unit), {1/3})
```

## Operations

Puzzles support the following operations:

- `type(puzzle)` returns `'puzzle'`
