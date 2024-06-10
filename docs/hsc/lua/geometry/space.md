# Space

A **space** is a finite-dimensional [Euclidean space] in which a puzzle can be constructed. The currently active space is stored in the global variable `SPACE`. A space is active during [puzzle construction](../puzzle-construction/puzzle.md).

[Euclidean space]: https://en.wikipedia.org/wiki/Euclidean_space

## Fields

Spaces have the following field:

- `.ndim` is the number of dimensions of the space.

## Methods

Spaces have no methods.

## Operations

Spaces support the following operations:

- `type(space)` returns `'space'`
- `tostring(space)`
