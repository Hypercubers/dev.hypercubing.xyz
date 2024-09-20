# Region

A **region** is a subset of a [space](space.md) bounded by [hyperplanes](hyperplane.md). They are most commonly used to identify pieces.

## Constants

The following global constants contain regions:

- `EVERYWHERE` is the region containing all of space
- `NOWHERE` is the region containing none of space

## Constructors

### `axis()`

Calling an axis as a function returns a region. It can be called in any of several ways:

- **Single layer.** Calling an axis with a single layer index constructs a region bounding that layer.
- **Two layers.** Calling an axis with two layer indices constructs a region bounding that layer range.
- **Layer list.** Calling an axis with a sequential table of layer indices constructs a region bounding the union of all of those layers.
- **Layer mask string.** Calling an axis with a layer mask string constructs a region bounding the union of the layers specified by the string. If the string is a single asterisk `*` then all layers are included.

```lua title="Examples using axis()"
-- Add 2 axes, each with 5 layers.
self.axes:add(vec('x'), {1/2, 1/4, 0, -1/2, -1/4})
self.axes:add(vec('y'), {1/2, 1/4, 0, -1/2, -1/4})
self.axes:autoname()

local region1 = self.axes.A(1) -- first layer of axis A
local region2 = self.axes.B('*') -- all 3 layers of axis B
local region3 = self.axes.B(1, 3) -- layers 1 to 3 of axis B
local region4 = self.axes.A({1, 3, 5}) -- layers 1, 3, and 5 of axis A
local region5 = self.axes.A('1..-2') -- layers 1 to 4 of axis A
```

### `hyperplane.region`

`hyperplane.region` returns the region bounded by a hyperplane, on the side its normal vector is pointing _away_ from.

```lua title="Examples of hyperplane.region"
local region1 = plane('x').region -- X <= 1
local region2 = plane('x').flip.region -- X >= 1
local region3 = ~plane('x').region -- X >= 1
```

### `orbit:intersection()`

`orbit:intersection()` returns the intersection of all regions in an orbit. Each element of the orbit must be a single region. Names are ignored.

```lua title="Examples of orbit:intersection()"
-- region inside a unit cube
local region1 = cd'bc3':orbit(plane('x').region):intersection()
```

### `orbit:union()`

`orbit:union()` returns the union of all regions in an orbit. Each element of the orbit must be a single region. Names are ignored.

```lua title="Examples of orbit:union()"
-- region outside a unit cube
local region1 = cd'bc3':orbit(plane('x').flip.region):union()
```

## Fields

Regions have no fields.

## Methods

Regions have the following methods:

- `:contains(point)` returns whether the region contains a point

## Operations

Regions support the following operations:

- `region & region` (intersection)
- `region | region` (union)
- `region ~ region` (symmetric difference)
- `~region` (complement)
- `type(region)` returns `'region'`
- `tostring(region)`
