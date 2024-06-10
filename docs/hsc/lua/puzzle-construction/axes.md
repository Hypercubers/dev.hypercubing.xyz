# Axes

## Axis system

The **axis system** is an ordered list of twist axes. It can be accessed during [puzzle construction](puzzle.md) as `puzzle.axes`.

Axes can be added to the axis system using [`puzzle:add_axes()`](puzzle.md#puzzleadd_axes).

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
