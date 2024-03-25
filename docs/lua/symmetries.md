# Symmetry groups

!!! warning "This API is not stable and may change in future versions"

The Hyperspeedcube Lua API includes utilities for working with symmetry groups of polytopes.

## Global functions

### `cd(indexes)`

Constructs a [Coxeter group] from a linear [Coxeter-Dynkin diagram] specified as a table `indexes`. For example, `cd({4, 3})` is the symmetry group of a cube.

Lua syntax allows omitting the parentheses when calling this function with a table literal. For example, you can write `cd{4, 3}` instead of `cd({4, 3})`.

[Coxeter group]: https://en.wikipedia.org/wiki/Coxeter_group
[Coxeter-Dynkin diagram]: https://en.wikipedia.org/wiki/Coxeter%E2%80%93Dynkin_diagram

#### Future plans

This function may eventually support nonlinear Coxeter-Dynkin groups.

## Types

### Symmetry group
