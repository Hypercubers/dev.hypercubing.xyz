# Orbit

An **orbit** is an iterator over all the unique locations of a set of [transformable] objects under some [symmetry].

[transformable]: transform.md#methods
[symmetry]: symmetry.md

## Constructors

### `symmetry:orbit()`

Calling the `:orbit()` method on a [symmetry] with one or more [transformable] objects constructs an orbit of those objects. For example, `cd'bc3':orbit(vec('x'), vec('y'))` constructs an orbit containing 12 elements, since there are twelve unique symmetric locations for the (ordered) pair of vectors $(\langle 1,0,0 \rangle, \langle 0,1,0 \rangle)$ on a cube.

## Fields

- `.symmetry` is the symmetry used to construct the orbit
- `.init` is a sequential table containing the elements used to construct the orbit
- `.names` is a sequential table containing names assigned to elements using [`:named()`](#orbitnamed)

## Methods

### `orbit:get()`

`orbit:get()` returns noe step of the iteration. It takes a single argument: an index.

The first value returned is always the transform for the orbit element. The remaining values are the transformed objects.

```lua title="Example using orbit:get()"
local sym = cd'bc3'
local axes_orbit = sym:orbit(sym.oox.unit)
local tranform1, axis1 = axes_orbit:get(1)
local tranform2, axis2 = axes_orbit:get(2)
local tranform3, axis3 = axes_orbit:get(3)
```

### `orbit:intersection()`

See [`orbit:intersection()`](region.md#orbitintersection).

### `orbit:iter()`

`orbit:iter()` returns a new identical orbit that can be used for iteration. Once an orbit has been iterated over in a `for` loop, it cannot be used again. See [Iteration](#iteration).

### `orbit:named()`

`orbit:named()` returns a new orbit with names assigned to the elements. It takes as its only argument a table with a specific format.

The keys of the table are the names to assign, and the values are a sequence of mirror reflections, optionally ending with the name of an initial element.

An example will make this clearer:

```lua title="Example using orbit:named()"
local sym = cd'bc3'
local named_orbit = sym:orbit(sym.oox.unit):named({
  F = {}, -- oox
  U = {3, 'F'},
  R = {2, 'U'},
  L = {1, 'R'},
  D = {2, 'L'},
  B = {3, 'D'},
})
```

In this example, the initial element of the orbit (`sym.oox.unit`) is assigned the name `F`, since its mirror sequence is the empty table `{}`. Starting with `F`, applying the third mirror brings us to a new element, which is assigned the name `U` (since `U` corresponds to the sequence `{3, 'F'}`; i.e., starting with `F`, reflect across mirror 3). `R` is generated from `U`, etc.

This example also specifies an order for the elements: `R` is the first element, `L'` is the second, etc.

This could also be written like this, using longer mirror sequences instead of referring to other elements:

```lua title="Example using orbit:named() with no named references"
local sym = cd'bc3'
local named_orbit = sym:orbit(sym.oox.unit):named({
  F = {}, -- oox
  U = {3},
  R = {2, 3},
  L = {1, 2, 3},
  D = {2, 1, 2, 3},
  B = {3, 2, 1, 2, 3},
})
```

You should rarely write one of these tables by hand. There are many of them bundled with the default Lua files in Hyperspeedcube, and you can generate new ones using the **Developer Tools** window in Hyperspeedcube.

### `orbit:prefixed()`

`orbit:prefixed()` returns a new orbit with a prefix prepended to each name in the orbit. It takes as its only argument an optional string to prepend. If no argument is supplied, then the orbit is returned unmodified.

```lua title="Example using orbit:named()"
local sym = cd'bc3'
local named_orbit = sym:orbit(sym.oox.unit):named({
  F = {}, -- oox
  U = {3, 'F'},
  R = {2, 'U'},
  L = {1, 'R'},
  D = {2, 'L'},
  B = {3, 'D'},
})
local orbit1 = named_orbit -- F, U, R, ...
local orbit2 = named_orbit:prefixed(nil) -- F, U, R, ...
local orbit3 = named_orbit:prefixed('Cubic') -- CubicF, CubicU, CubicR, ...
```

### `orbit:union()`

See [`orbit:union()`](region.md#orbitunion).

## Operations

Orbits support the following operations:

- `#orbit` returns the length of the orbit; i.e., the number of iterations
- `orbit[index]` returns the first transformed object at the given index in the orbit
- `type(orbit)` returns `'orbit'`

## Iteration

Orbit objects can be used in `for` loops. When iterated over, they yield a transform from the symmetry group, the sequence of initial objects transformed by that symmetry (as separate values, not as a table), and lastly a name (or `nil` if no name has been assigned for the element).

There may be multiple transforms that produce the same element of the orbit; it is unspecified which one will used when iterating.

```lua title="Examples iterating over orbits"
local symmetries = require('symmetries')
local sym = cd'bc3'

for t, v1, name in sym:orbit(sym.oox):named(symmetries.cubic.FACE_NAMES_LONG) do
  assert(v1 == t:transform(sym.oox))
  print(v1) -- +X, -X, +Y, -Y, +Z, -Z
  print(name) -- Right, Left, Up, Down, Front, Back
end

for t, face_vector, edge_vector in sym:orbit(sym.oox, sym.oxo) do
  assert(face_vector == t:transform(sym.oox))
  assert(edge_vector == t:transform(sym.oxo))
  -- no names, because we didn't call `:named()`
end
```

It is good practice to call [`:iter()`](#iteration) on a symmetry when using it in a `for` loop, unless the symmetry is constructed inline in a `for` loop.

```lua title="Examples of when to use orbit:iter()"
local sym = cd'bc3'

-- This is ok, because the symmetry is constructed inline
for _, v in sym:orbit(sym.oox) do
  print(v)
end

-- This is ok, because we call `:iter()`
local orbit1 = sym:orbit(sym.oox)
for _, v in orbit1:iter() do
  print(v)
end

-- This is questionable but will work, because we are only using the orbit once
local orbit2 = sym:orbit(sym.oox)
for _, v in orbit2 do
  print(v)
end

-- This is bad! We are using the same orbit object twice
for _, v in orbit2 do
  -- The contents of this loop will not be executed
  -- because the iterator has been used up
  print(v)
end

-- This is ok! Calling `:iter()` always gives a fresh iterator
for _, v in orbit2:iter() do
  print(v)
end

-- This is also ok! Functions that take an orbit as a parameter implicitly call `:iter()`
puzzle:carve(orbit2)
```
