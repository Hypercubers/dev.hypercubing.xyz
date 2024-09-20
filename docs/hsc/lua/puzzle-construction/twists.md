# Twists

## Twist system

The **twist system** is a list of twists, sorted in alphabetical order by name. It can be accessed during [puzzle construction](puzzle.md) as `puzzle.twists`.

Twists can be added to the twist system using [`puzzle.twists:add()`](#puzzletwistsadd).

### Twist indexing

The twist system can be indexed by the name or index of a twist.

```lua title="Examples of twist indexing"
-- Gets the twist with the name "L"
puzzle.twists.L

-- Gets the twist with the name "L'"
puzzle.twists["L'"]

-- Gets the first twist alphabetically
puzzle.twists[1]
```

### Fields

The twist system has no fields.

### Methods

#### `puzzle.twists:add()`

`puzzle.twists:add()` adds a twist to the twist system, along with (optionally) its inverses and multipliers. It takes three arguments: an [axis](axes.md#axis) whose layers bound the pieces affected by the twist, a [transform](../geometry/transform.md) to apply to pieces, and an optional table with extra data about the twist.

The table has the following keys:

| Key                   | Type       | Default value | Description                                                       |
| --------------------- | ---------- | ------------- | ----------------------------------------------------------------- |
| `multipliers`         | `boolean`  | _See note 1_  | whether to generate double/triple/etc. twists[^twist-multipliers] |
| `inverse`             | `boolean`  | _See note 1_  | whether to generate inverse twist[^twist-inverse]                 |
| `prefix`              | `string`   | axis name     | prefix before twist name                                          |
| `name`                | `string`   | `nil`         | name for twist                                                    |
| `suffix`              | `string`   | `nil`         | suffix after twist name                                           |
| `inv_name`            | `string`   | `nil`         | name for inverse twist                                            |
| `inv_suffix`          | `string`   | _See note 2_  | suffix after inverse twist name                                   |
| `name_fn`             | `function` | `nil`         | function from integer to string                                   |
| `qtm`                 | `integer`  | `1`           | value of the twist in the quantum turn metric                     |
| `gizmo_pole_distance` | `number`   | `nil`         | distance of the gizmo pole facet                                  |

!!! note "Notes"

    1. `true` in 3D; `false` in 4D+
    2. If `inv_name` is `nil` or unspecified, then `inv_suffix` defaults to `"'"` (a string containing the `'` character); if `inv_name` is non-`nil`, then `inv_suffix` defaults to an empty string

[^twist-multipliers]: E.g., `R2` from `R`
[^twist-inverse]: E.g., `R'` from `R`

If `name_fn` is non-`nil`, then `name` and `inv_name` must both be `nil`.

`name_fn` takes a single argument: an integer corresponding to the twist multiplier (e.g., `1` for `R`, `-1` for `R'`, `2` for `R2`, `-2` for `R2'`, etc.). It returns a string, which is used in place of both `name` and `inv_name`.

Each twist is assigned a name by concatenating `<prefix> .. <name> .. <suffix> .. <multiplier>`, where:
- `<prefix>` is `prefix`
- `<name>` is either the output of `name_fn` (if `name_fn` is non-`nil`) or `name`/`inv_name` (selected using the sign of the multiplier).
- `<suffix>` is `suffix`/`inv_suffix` (selected using the sign of the multiplier)
- `<multiplier>` is `tostring(multiplier)`

#### `puzzle.twists:rename()`

`puzzle.twists:rename()` renames multiple twists at once. See [`:rename()`](../common.md#rename)

### Operations

- `#puzzle.twists` returns the number of twists that have been defined
- `type(puzzle.twists)` returns `'twistsystem'`
- `tostring(puzzle.twists)`
- `ipairs(puzzle.twists)`

## Twist

A **twist** is a twist that can be applied to the puzzle.

### Fields

- `.name` is the string name of the color
- `.axis` is the [axis](axes.md#axis) of the twist, which is typically fixed by its transform
- `.transform` is the [transform](../geometry/transform.md) applied to pieces during the twist

Some fields can be written to:

- Writing a new string to `.name` renames the twist

All other fields are read-only.

### Methods

Axes have no methods.

### Operations

- `twist == twist`
- `twist ~= twist`
- `twist * twist` composes two twists, if there is a twist corresponding to their composition
- `twist ^ number` returns the twist composed with itself some number of times (which may be negative), if there is such a twist
- `type(twist)` returns `'twist'`
- `tostring(twist)`

## Layer system

A **layer system** is an ordered list of the [layers](#layer) on an [axis](axes.md#axis). It can be accessed during [puzzle construction](puzzle.md) as `axis.layers`.

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

A **layer** is a region along a [twist axis](axes.md#axis) where pieces may be affected by a [twist](twists.md#twist). It is bounded below by a hyperplane, and optionally bounded above by another hyperplane.

[mapping]: ../common.md#mappings
