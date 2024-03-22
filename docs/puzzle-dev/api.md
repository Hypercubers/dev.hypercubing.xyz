# Lua API Reference

!!! warning "This API is not stable and may change in future versions"

## General utilities

See the [Lua 5.4 reference manual](https://www.lua.org/manual/5.4/manual.html) for general Lua functionality.

Hyperspeedcube user code is run in a sandbox with the following global constants and functions available, unmodified from Lua:

- `_VERSION`
- `ipairs`
- `next`
- `pairs`
- `select`
- `tonumber`
- `tostring`

The following functions mimic their original behavior, but have been modified to capture their output or account for new types:

- `assert`
- `error`
- `warn`
- `type`

The following global constants have been added:

- `_PUZZLE_ENGINE` - string containing the program name and version; e.g., `"hyperspeedcube v2.0.0"`
- `FILENAME` - name of the file currently executing
- `AXES` - table mapping axis names to numbers and vice versa

The following global functions have been added:

### `pstring(v)`

Converts a value `v` of any type to a string in a "pretty" way:

- Strings are escaped
- Tables have their contents printed (safe with recursive tables)
- All other types use `tostring()`

### `pprint(...)`

Same as `print()`, but uses `pstring()` instead of `tostring()`.

### `vec(...)`

Constructs a vector.

- If no arguments are given, then the zero vector is returned.
- If the only argument is a vector, then it is returned unmodified.
- If the only argument is a multivector, then it is grade-projected to a vector and returned.
- If the only argument is a string specifying an axis, then a unit vector pointing along that axis is returned.
- If the only argument is a table, then the values in the table are used. See [Vector indexing](#vector-indexing) for valid keys.
- Otherwise, each `i`th argument is assigned to the `i`th index in the vector.

Values must be numbers.

### `mvec([v])`

Constructs a multivector from a value `v`.

- If `v` is `nil` or omitted, then the zero vector is returned.
- If `v` is a multivector, then it is returned unmodified.
- If `v` is a vector, then it is converted to a 1-blade (vector blade) and returned.
- If `v` is a number, then it is converted to a 0-blade (scalar blade) and returned.
- If `v` is a string specifying a multivector component, then a unit multivector for that components is returned.
- If `v` is a table, then the values in the table are used. See [Multivector indexing](#multivector-indexing) for valid keys.

### `plane(...)`

Constructs a multivector representing a plane. TODO: document more

### `sphere(...)`

Constructs a multivector representing a sphere. TODO: document more

### `cd(indexes)`

Constructs a [Coxeter group](https://en.wikipedia.org/wiki/Coxeter_group) from a [Coxeter-Dynkin diagram](https://en.wikipedia.org/wiki/Coxeter%E2%80%93Dynkin_diagram) specified as a table `indexes`. For example, `cd({4, 3})` is the symmetry group of a cube.

Lua syntax allows omitting the parentheses when calling this function with a table literal. For example, you can write `cd{4, 3}` instead of `cd({4, 3})`.

### `svec(...)`

TODO: document

### `puzzledef{...}`

TODO: document

## Math API

The math API is almost unmodified from Lua. The functions `math.random` and `math.randomseed` have been removed and the following have been added:

- `math.tau` - equivalent to `math.pi * 2`
- `math.phi` - equivalent to `(1 + math.sqrt(5)) / 2`

Other functions remain unmodified:

### Trigonometry

- `math.acos`
- `math.asin`
- `math.atan`
- `math.cos`
- `math.deg`
- `math.pi`
- `math.rad`
- `math.sin`
- `math.tan`

### Mathematical functions

- `math.abs`
- `math.ceil`
- `math.exp`
- `math.floor`
- `math.fmod`
- `math.log`
- `math.max`
- `math.min`
- `math.modf`
- `math.sqrt`

### Implementation utilities

- `math.huge`
- `math.maxinteger`
- `math.mininteger`
- `math.ult`
- `math.tointeger`
- `math.type`

## String API

All functions from the Lua string API remain unmodified:

- `string.byte`
- `string.char`
- `string.dump`
- `string.find`
- `string.format`
- `string.gmatch`
- `string.gsub`
- `string.len`
- `string.lower`
- `string.match`
- `string.pack`
- `string.packsize`
- `string.rep`
- `string.reverse`
- `string.sub`
- `string.unpack`
- `string.upper`

The following have been added:

### `string.join(connector, t)`

Given a string `connector` and a list `t`, returns a string containing each element from `t` converted to a string using `tostring()` concatenated together, separated by `connector`.

## Table API

The entire `table` API is unmodified from Lua.

## UTF-8 API

The entire `utf8` API is unmodified from Lua.

## Types

The Hyperspeedcube API adds several new types.

### Vector

Vectors can be constructed using [`vec()`](#vec). Vectors are also constructed automatically by functions that require them, so you can often omit the call to `vec()`.

Vectors cannot be mutated once constructed. To modify a vector, you must construct a new vector and then replace the old one.

#### Vector indexing

Vectors can be indexed by positive integers or single-character strings. The following strings are recognized, uppercase or lowercase:

- `X` = 1
- `Y` = 2
- `Z` = 3
- `W` = 4
- `V` = 5
- `U` = 6
- `T` = 7
- `S` = 8
- `R` = 9
- `Q` = 10

#### Vector methods

Vectors have the following methods:

- `:ndim()` - Returns the number of components in the vector, including zeros.
- `:mag2()` - Returns the squared magnitude of the vector.
- `:mag()` - Returns the magnitude of the vector.
- `:at_ndim(new_ndim)` - Truncates or extends the vector with zeros to the desired number of components.
- `:normalized([new_mag])` - Scales the vector so that its magnitude is `new_mag`, or `1` if `new_mag` is `nil`.
- `:dot(other)` - Returns the dot product between the vector and another vector `other`.
- `:cross(other)` - Returns the 3D cross product between the vector and another vector `other`, ignoring components other than XYZ.
- `:projected_to(other)` - Returns the component of the vector that is parallel to another vector `other`.
- `:rejected_from(other)` - Returns the component of the vector that is perpendicular to another vector `other`.

#### Vector operations

Vectors support the following operations:

- `vector + vector`
- `vector - vector`
- `vector * float`
- `float * vector`
- `vector / float`
- `-vector`
- `vector == vector` (uses approximate floating-point comparison)
- `vector != vector` (uses approximate floating-point comparison)
- `#vector` (same as `vector:ndim()`)
- `tostring(vector)`
- `pairs(vector)`

For `+` and `-`, either of the two vectors may be substituted for a value of any other type and it will be converted to a vector using [`vec(...)`](#vec).

### Multivector

Multivectors can be constructed using [`mvec()`](#mvecv). Multivectors are also constructed automatically by functions that require them, so you can often omit the call to `mvec()`.

Multivectors cannot be mutated once constructed. To modify a multivector, you must construct a new multivector and then replace the old one.

#### Multivector indexing

Multivectors can be indexed by positive integers or strings. Integers are converted into strings and then processed. Strings are broken down character-by-character to specify a component of a multivector. The characters `1`, `x`, and `X` all represent the $x$ basis vector; `2`, `y`, and `Y` all represent the $y$ basis vector; etc. up to 6 dimensions. After 6 dimensions, axis names are not allowed. See [Vector indexing](#vector-indexing) for all axis letters. Other special characters:

- `s`, `e`, `n`, `_`, and space are ignored.
- `m` or `-` represents the basis vector $e_-$.
- `p` or `+` represents the basis vector $e_+$.
- `o` represents the basis vector $n_o = \frac{1}{2}(e_- - e_+)$.
- `i` represents the basis vector $n_\infty = e_- + e_+$.
- `E` represents the basis plane $n_o \wedge n_i = e_- e_+$.

These characters can be concatenated to access components of any grade. For example:

- `multivector.s` returns the scalar component of `multivector`.
- `multivector.Exyz` returns the coefficient for the $e_- e_+ x y z$ component of `multivector`.
- `multivector.no` returns the projection of the vector components of `multivector` onto $n_o$.
- `multivector.em_xy` or `multivector['e- x y']` returns the coefficient for the $e_- x y$ component of `multivector`.

When concatenating components, order matters: for example, `multivector.xy` equals `-multivector.yx`.

#### Multivector methods

Multivectors have the following methods:

- `:ndim()` - Returns the minumum number of dimensions necessary to represent the multivector.
- `:grade()` - Returns the grade of the multivector if it is a blade, or `nil` if the multivector is not a blade.
- `:mag2()` - Returns the squared magnitude of the multivector.
- `:mag()` - Returns the magnitude of the multivector.
- `:reverse()` - Returns the reverse of the multivector.
- `:inverse()` - Returns the inverse of the multivector.

#### Multivector operations

Multivectors support the following operations:

- `multivector + multivector`
- `multivector - multivector`
- `multivector * multivector`
- `multivector / multivector`
- `-multivector`
- `multivector ^ multivector` (wedge product $\wedge$)
- `multivector << multivector` (left contraction $\rfloor$)
- `~multivector` (reverse $^\dagger$)
- `multivector == multivector` (uses approximate floating-point comparison)
- `multivector != multivector` (uses approximate floating-point comparison)
- `tostring(multivector)`
- `pairs(multivector)`

For binary operations, either of the two multivectors may be substituted for a value of any other type and it will be converted to a multivector using [`mvec(v)`](#mvecv).

### Symmetry

Symmetries can be constructed using [`cd(...)`](#cdindexes). Symmetries are also constructed automatically by functions that require them, so you can often omit the call to `cd()`.

A symmetry defined using `cd()` has a **mirror basis** of unit-length vectors, where each vector is parallel to all mirror planes except one.

#### Symmetry methods

Symmetries have the following methods:

- `:ndim()` - Returns the minimum number of dimensions required by the symmetry.
- `:vec(...)` - Constructs a vector by passing the arguments to `vec(...)`, then transforms that vector from the mirror basis. See [`svec(...)`](#svec).
