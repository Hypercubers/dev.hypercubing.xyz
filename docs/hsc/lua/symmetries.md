# Symmetries

A **symmetry** is a [group] of transformations in space. Each element of the [symmetry group] is represented as a [transform](transforms.md).

[group]: https://en.wikipedia.org/wiki/Group_(mathematics)
[symmetry group]: https://en.wikipedia.org/wiki/Symmetry_group

!!! info "Coxeter groups"

    The symmetry groups of most puzzle shapes are [Coxeter groups][Coxeter group]. It's highly recommended that you learn about Coxeter groups and their construction using mirror planes before reading the rest of this page. The section titled **Symmetries of the cube** in [this article](https://syntopia.github.io/Polytopia/polytopes.html) is a good primer, and the interactive demos with three planes are very helpful references.

## Constructors

### `cd()`

`cd()` constructs a [Coxeter group] and can be called in either of two ways:

- **Table of branch labels.** Calling `cd()` with a table containing a sequence of positive integers constructs group corresponding to the linear [Coxeter-Dynkin diagram] with those branch labels.
- **Coxeter group name.** Calling `cd()` with the name of a Coxeter group constructs that Coxeter group. The name is case-insensitive and must consist of one or two letters followed by a number.

[Coxeter group]: https://en.wikipedia.org/wiki/Coxeter_group
[Coxeter-Dynkin diagram]: https://en.wikipedia.org/wiki/Coxeter%E2%80%93Dynkin_diagram

The table of branch labels is similar to a [Schläfli symbol]. For example, `cd{4, 3}` is the symmetry group of a cube, and is equivalent to `cd'b3'`, `cd'c3'`, and `cd'bc3'`.[^omit-braces]

[Schäfli symbol]: https://en.wikipedia.org/wiki/Schl%C3%A4fli_symbol

See this diagram for a list of named Coxeter groups:

![Diagrams representing finite Coxeter groups](https://en.wikipedia.org/wiki/Coxeter%E2%80%93Dynkin_diagram#/media/File:Finite_coxeter.svg)

Here is a list of the names for the finite Coxeter groups:

- `a2`, `a3`, `a4`, ... (simplical symmetries)
- `bc2`, `bc3`, `bc4`, ... (hypercubic symmetries)
    - equivalently: `b2`, `b3`, `b4`, ...
    - equivalently: `c2`, `c3`, `c4`, ...
- `d4`, `d5`, `d6`, ...
- `e6`, `e7`, `e8`
- `f4`
- `h2`, `h3`, `h4` (pentagonal, dodecahedral/icosahedral, and 120-cell/600-cell symmetries)
- `g6` (hexagonal symmetry)
- `i2`, `i3`, `i4`, ... (polygonal symmetries)

## Fields

Symmetries have the following fields:

- `.ndim` is the minimum number of dimensions required to contain the symmetry
- `.mirror_vectors` is a table of vectors, each corresponding to one of the mirror transformations that together generate the group
- `.chiral` is the canonical chiral subgroup of the symmetry; i.e., the symmetry which is the same as this one but excluding reflections

```lua title="Example using mirror vectors of a symmetry"
local mirrors = cd'bc3'.mirror_vectors
assert(#mirrors == 3)
assert(mirrors[1] == vec(1))
assert(mirrors[2] == vec(-1, 1) / sqrt(2))
assert(mirrors[3] == vec(0, -1, 1) / sqrt(2))
```

Additionally, symmetries have a field corresponding to each possible vector written in [Dynkin notation] that uses only the characters `o` and `x`. For example `cd'bc3'.xoo` is a vector pointing toward the vertex of a cube or the face of an octahedron.

## Methods

Symmetries have the following methods:

- [`:orbit()`](#symmetryorbit)
- [`:vec()`](#symmetryvec)
- [`:thru()`](#symmetrythru)

### `symmetry:orbit()`

`:orbit()` returns the [orbit](orbits.md) of its arguments under the symmetry.

TOOD: document `:orbit()`

### `symmetry:vec()`

`:vec()` returns constructs a vector written using [Dynkin notation] or converts a vector into the basis defined by `.mirror_vectors`

TOOD: document `:vec()`

### `symmetry:thru()`

`:thru()` constructs a transformation by composing reflections across the corresponding mirror planes

TOOD: document `:thru()`

## Operations

Symmetries support the following operations:

- `type(symmetry)` (returns `'symmetry'`)
- `tostring(symmetry)`

## Dynkin notation

[Dynkin notation]: #dynkin-notation

To describe points relative to a symmetric mirror construction, we use [Dynkin notation](https://web.archive.org/web/20230410033043/https://bendwavy.org/klitzing//explain/dynkin-notation.htm). A point specified in Dynkin notation consists of a string of characters, each specifying the distance from its corresponding mirror planes. Here is a list of all the symbols supported by Hyperspeedcube:

| Symbol |            Distance            |
| :----: | :----------------------------: |
|  `o`   |              $0$               |
|  `x`   |              $1$               |
|  `u`   |              $2$               |
|  `q`   |           $\sqrt 2$            |
|  `f`   | $\phi = \frac{1 + \sqrt 5}{2}$ |

For example, the string `oqx` describes a point touching the first mirror plane, $\sqrt 2$ units from the second mirror plane, and $\phi$ units from the third mirror plane.

```lua title="Examples using Dynkin notation"
local sym = cd'h3' -- symmetry of a dodecahedron or icosahedron
sym:orbit(sym.oox.unit) -- face poles of a dodecahedron, or vertices of an icosahedron
sym:orbit(sym.oxo.unit) -- edge poles of a dodecahedron or icosahedron
sym:orbit(sym.xoo.unit) -- vertices of a dodecahedron or face poles of an icosahedron

local sym = cd'bc4' -- symmetry of a hypercube or 16-cell
sym:orbit(sym.ooox.unit) -- cell poles of a hypercube, or vertices of a 16-cell
sym:orbit(sym.ooxo.unit) -- face poles of a hypercube, or edge poles of a 16-cell
sym:orbit(sym.oxoo.unit) -- edge poles of a hypercube, or face poles of a 16-cell
sym:orbit(sym.xooo.unit) -- vertices of a hypercube, or cell poles of a 16-cell
```

<!-- Footnotes -->

[^omit-braces]: Lua syntax allows omitting the parentheses when calling a function with a table literal or string literal as its only argument. For example, you can write `cd{4, 3}` instead of `cd({4, 3})` or `cd'bc3'` instead of `cd('bc3')`.
