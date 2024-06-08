# Geometric primitives

!!! warning "This API is not stable and may change in future versions"

The Hyperspeedcube Lua API includes utilities for vector math and geometric algebra.

## Global functions

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

## Types
