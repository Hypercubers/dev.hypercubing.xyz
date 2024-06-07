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

Symmetries can be constructed using [`cd(...)`](#cdindexes). Symmetries are also constructed automatically by functions that require them, so you can often omit the call to `cd()`.

A symmetry defined using `cd()` has a **mirror basis** of unit-length vectors, where each vector is parallel to all mirror planes except one.

#### Symmetry methods

Symmetries have the following methods:

- `:ndim()` - Returns the minimum number of dimensions required by the symmetry.
- `:vec(...)` - Constructs a vector by passing the arguments to `vec(...)`, then transforms that vector from the mirror basis.
