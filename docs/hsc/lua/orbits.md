# Orbits

An **orbit** is an iterator over all the unique locations of a set of [transformable] objects under some [symmetry].

[transformable]: transforms.md#methods
[symmetry]: symmetries.md

## Constructors

### `symmetry:orbit()`

Calling the `:orbit()` method on a [symmetry] with one or more [transformable] objects constructs an orbit of those objects. For example, `cd'bc3':orbit(vec('x'), vec('y'))` constructs an orbit containing 12 elements, since there are twelve unique symmetric locations for the (ordered) pair of vectors $(\langle 1,0,0 \rangle, \langle 0,1,0 \rangle)$ on a cube.

## Fields

- `symmetry` is the symmetry used to construct the orbit
- `init` is a sequential table containing the elements used to construct the orbit
- `names` is a sequential table containing the names of elements (assigned using [`:with()`](#orbitwith)), or `nil` if [`:with()`](#orbitwith) has not been called

## Methods

### `orbit:iter()`

`:iter()` returns a new identical orbit that can be used for iteration. Once an orbit has been iterated over in a `for` loop, it cannot be used again.

It is good practice to call `:iter()` on a symmetry when using it in a `for` loop, unless the symmetry is constructed inline in a `for` loop.

```lua title="Examples iterating over orbits"
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
  print(v) -- This will not be called because the iterator has been used up
end

-- This is ok! Calling `:iter()` always gives a fresh iterator
for _, v in orbit2:iter() do
  print(v)
end

-- This is also ok! Functions that take an orbit as a parameter implicitly call `:iter()`
puzzle:carve(orbit2)
```

### `orbit:with()`

`:with()` returns a new orbit, reordered and with names assigned. It takes as its only argument a table with a specific format.

The table must have a single named key `symmetry` containing the symmetry it is defined with respect to. The rest of the table is a sequence of pairs, where the first value is the name of the element and the second value is a sequence of mirror reflections, optionally ending with the name of an initial element.

An example will make this clearer:

```lua title="Example using symmetry:with()"
local sym = cd'bc3'
local named_orbit = sym:orbit(sym.oox.unit):with({
  symmetry = sym,
  {'R', {2, 'U'}},
  {'L', {1, 'R'}},
  {'U', {3, 'F'}},
  {'D', {2, 'L'}},
  {'F', {}}, -- oox
  {'B', {3, 'D'}},
})
```

In this example, the initial element of the orbit (`sym.oox.unit`) is assigned the name `'F'`, since its mirror sequence is the empty table `{}`. Starting with `'F'`, applying the third mirror brings us to a new element, which is assigned the name `'U'` (since `'U'` corresponds to the sequence `{3, 'F'}`; i.e., starting with `'F'`, reflect across mirror 3). `'R'` is generated from `'U'`, etc.

This example also specifies an order for the elements: `'R'` is the first element, `'L''` is the second, etc.

This could also be written like this, using longer mirror sequences instead of referring to other elements:

```lua title="Example using symmetry:with() with no named references"
local sym = cd'bc3'
local named_orbit = sym:orbit(sym.oox.unit):with({
  symmetry = sym,
  {'R', {2, 3}},
  {'L', {1, 2, 3}},
  {'U', {3}},
  {'D', {2, 1, 2, 3}},
  {'F', {}}, -- oox
  {'B', {3, 2, 1, 2, 3}},
})
```

You should almost never write one of these tables by hand. There are many of them bundled with the default Lua files in Hyperspeedcube, and there will soon be a user interface for generating them.

## Operations

- `#orbit` returns the length of the orbit; i.e., the number of iterations
- `type(orbit)` returns `'orbit'`

## Iteration

TODO: document iteration over orbits
