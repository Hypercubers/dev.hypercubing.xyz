# Axes

## Axis system

The **axis system** is an ordered list of twist axes. It can be accessed during [puzzle construction](puzzle.md) as `puzzle.axes`.

Axes can be added to the axis system using [`puzzle.axes:add()`](#puzzleaxesadd).

### Axis indexing

The axis system can be indexed by the name or index of a axis, or by its vector.

```lua title="Examples of axis indexing"
-- Gets the axis with the name "L"
puzzle.axes.L

-- Gets the first axis in the list
puzzle.axes[1]

-- Gets the axis with the vector (1,0,0)
puzzle.axes[vec('x')]
```

### Fields

The axis system has no fields.

### Methods

#### `puzzle.axes:add()`

`puzzle.axes:add()` adds a [twist axis](axes.md) to the axis system, optionally adding [layers](twists.md#layer-system) to it and slicing the puzzle on each layer boundary. It takes two arguments: a [vector](../geometry/blade.md#vectors) for the new twist axis, and an optional table containing additional arguments. The new axis is returned.

If the vector is an [orbit](../geometry/orbit.md) of [vectors](../geometry/blade.md#vectors) instead of just a single one, then `puzzle.axes:add()` will add an axis for each vector, all with the same layers. If the orbit has [names assigned](../geometry/orbit.md#orbitnamed), then the new axes created by the cut will be assigned those names. An orbit of the new axes is returned.

The table may contain two optional keys:

- `layers` is an optional sequential table of distances from the origin at which to add layer boundaries
- `slice` is an optional boolean which defaults to `true`

If the table contains a sequence, then that sequence is used in place of `layers`. In this case the `slice` key is still used as normal.

If `layers` is specified, then `add_axes()` will automatically add [layers](twists.md#layer-system) to each axis it creates.

If `slice` is `true`, then `add_axes()` will automatically [slice](puzzle.md#puzzleslice) all pieces at the layer boundaries specified by `layers`.

```lua title="Examples using puzzle.axes:add()"
local sym = cd'bc3'

-- Add cubic axes with no layers
puzzle.axes:add(sym:orbit(sym.oox.unit))

-- Add cubic axes with a single cut through the origin
puzzle.axes:add(sym:orbit(sym.oox.unit), {0})

-- Add cubic axes with 3x3x3-like cuts
puzzle.axes:add(sym:orbit(sym.oox.unit), {1/3})
```

#### `puzzle.axes:autoname()`

`puzzle.axes:autoname()` assigns alphabetic names to all twist axes. It takes no arguments.

#### `puzzle.axes:rename()`

`puzzle.axes:rename()` renames multiple axes at once. See [`:rename()`](../common.md#rename)

#### `puzzle.axes:swap()`

`puzzle.axes:swap()` swaps two axes in the ordering. See [`:swap()`](../common.md#swap)

#### `puzzle.axes:reorder()`

`puzzle.axes:reorder()` changes the ordering of all axes. See [`:reorder()`](../common.md#reorder)

### Operations

- `#puzzle.axes` returns the number of axes that have been defined
- `type(puzzle.axes)` returns `'axissystem'`
- `tostring(puzzle.axes)`
- `ipairs(puzzle.axes)`

## Axis

An **axis** is a twist axis around which twists can be defined.

### Fields

- `.name` is the string name of the color
- `.index` is the integer index of the color in the color system
- `.vector` is the vector of the axis, which typically stays fixed during any of its twists
- `.layers` is the [layer system](#layer-system) of the axis
- `.opposite` is the axis with the opposite vector, or `nil` if there is none

Some fields can be written to:

- Writing a new string to `.name` renames the axis
- Writing to `.index` reorders the axis to that index, shifting the axes in between

All other fields are read-only.

### Methods

Axes have no methods.

### Operations

- `type(axis)` returns `'axis'`
- `tostring(axis)`

## Layer system

A **layer system** is an ordered list of the [layers](#layer) on an [axis](#axis). It can be accessed during [puzzle construction](puzzle.md) as `axis.layers`.

### Layer system indexing

Indexing a layer system by a layer index returns a table with the following keys:

- `bottom` is the hyperplane bounding the bottom of the layer
- `top` is the hyperplane bounding the top of the layer, or `nil` if there is none

### Fields

A layer system has no fields.

### Methods

#### `axis.layers:add()`

`axis.layers:add()` adds a new layer to the layer system. It takes two arguments: a hyperplane bounding the bottom of the layer, and an optional hyperplane bounding the top of the layer. If the second hyperplane is omitted, then the layer is unbounded.

### Operations

- `#axis.layers` returns the number of layers in the layer system
- `type(axis.layers)` returns `'layersystem'`
- `ipairs(axis.layers)`

## Layer

A **layer** is a region along a [twist axis](#axis) where pieces may be affected by a [twist](twists.md#twist). It is bounded below by a hyperplane, and optionally bounded above by another hyperplane.

[mapping]: ../common.md#mappings
