# Puzzle

When [defining a puzzle](../puzzle-library.md#puzzlesadd), the `build` function takes a single argument of type `puzzle`. The puzzle is initialized with a single infinite piece; this piece must be [`carve()`](#puzzlecarve)ed to make it finite in order to have a valid puzzle. It can then have [twist axes](axes.md#puzzleaxesadd) and [twists](twists.md#puzzletwistsadd) defined.

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

    Some puzzles with real-world designs leave faces of pieces unstickered; this is not advised in Hyperspeedcube. It is much better to sticker them using an extra color, and let the user hide them if they would like to.

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

### `puzzle:add_piece_type()`

`puzzle:add_piece_type()` adds a piece type if it does not already exist, and sets it display name. It takes one argument: a table containing the following keys:

- `name` is a string containing a name for the piece type (e.g., `center/t_2`)
- `display` is a string containing a user-facing display name for the piece type (e.g., `T-center (2)`)

The key `name` is required. `display` is optional.

`name` consists of segments separated by `/`, which describes its position in the hierarchy of piece types. Parent piece types are automatically created; for example, `puzzle:add_piece_type{ name = 'a/b/c' }` will create the piece types `a`, `a/b`, and `a/b/c` if they do not already exist. Only the leaf piece (`a/b/c` in the example) will have its display name set.

### `puzzle:mark_piece()`

`puzzle:mark_piece()` marks the piece type of a piece. It takes one argument: a table containing the following keys:

- `region` is a [region](../geometry/region.md) containing a piece
- `name` is a string containing a name for the piece type (e.g., `center/t_2`)
- `display` is a string containing a user-facing display name for the piece type (e.g., `T-center (2)`)

The keys `region` and `name` are required. `display` is optional.

`puzzle:mark_piece()` automatically calls `puzzle:add_piece_type()` to ensure that the piece type exists and to assign a display name if one is provided.

If `region` matches more or fewer than 1 piece, then a warning is emitted.

Note that a piece cannot be marked with a parent type. For example, if the piece type `a/b/c` exists, then no piece may be marked with `a/b` or `a`.

### `puzzle:unify_piece_types()`

`puzzle:unify_piece_types()` marks the piece type for many pieces using symmetry. It takes one argument: the [symmetry](../geometry/symmetry.md) by which to expand.

```lua title="Example defining piece types"
-- Define piece types for a 3x3x3 Rubik's cube
puzzle:mark_piece{
  region = R(2) & U(2) & F(1),
  name = 'center',
  display = "Center",
}
puzzle:add_piece_type{
  name = 'moving',
  display = "Moving pieces",
}
puzzle:mark_piece{
  region = R(2) & U(1) & F(1),
  name = 'moving/edge',
  display = "Edge",
}
puzzle:mark_piece{
  region = R(1) & U(1) & F(1),
  name = 'moving/corner',
  name = "Corner",
}
puzzle:unify_piece_types(sym'bc3')
```

## Operations

Puzzles support the following operations:

- `type(puzzle)` returns `'puzzle'`
