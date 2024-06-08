# Blades

A blade is a geometric algebra primitive that can be used to represent a vector, a point, a line, a hyperplane, or numerous other geometric objects. Hyperspeedcube uses blades from the [Projective Geometric Algebra](https://en.wikipedia.org/wiki/Plane-based_geometric_algebra#Projective_Geometric_Algebra), but knowledge of geometric algebra is not required in order to use the API effectively.

Blades cannot be mutated once constructed. To modify a blade, you must construct a new blade and then replace the old one.

## Vectors

Vectors can be constructed using [`vec()`](#vec). Vectors are also constructed automatically by functions that require them, so you can often omit the call to `vec()`.

### Vector component access

Vectors can be indexed by positive integers or single-character strings. The following single-letter and single-digit strings are recognized, uppercase or lowercase:

- `my_vector.X` = `my_vector[1]` = X axis
- `my_vector.Y` = `my_vector[2]` = Y axis
- `my_vector.Z` = `my_vector[3]` = Z axis
- `my_vector.W` = `my_vector[4]` = W axis (4th dimension)
- `my_vector.V` = `my_vector[5]` = V axis (5th dimension)
- `my_vector.U` = `my_vector[6]` = U axis (6th dimension)
- `my_vector[7]` = 7th dimension

??? example "Example using vector indexing"

    ```lua
    local my_vector = vec(10, 20, 30)
    assert(my_vector.x == 10)
    assert(my_vector.Y == 20)
    assert(my_vector[3] == 30)
    ```

### Vector fields

- `.unit` is the normal vector in the same direction as the original vector
- `.mag2` is the squared magnitude of the vector
- `.mag` is the magnitude of the vector

See [Blade fields](#blade-fields) for a full list.

### Vector methods

Vectors support all the same methods as general [blades](#blade-methods). Additionally, they have the following method:

- `:cross(other)` returns the 3D cross product between the vector and another vector `other`, ignoring components other than XYZ

### Vector operations

Vectors support the following operations:

- `vector + vector`
- `vector - vector`
- `vector * float`
- `float * vector`
- `vector / float`
- `-vector`
- `vector == vector` (uses approximate floating-point comparison)
- `vector ~= vector` (uses approximate floating-point comparison)
- `#vector` (same as `vector:ndim()`)
- `type(vector)` (returns `'blade'`)
- `tostring(vector)`
- `pairs(vector)`

For `+` and `-`, either of the two vectors may be substituted for a value of any other type and it will be converted to a vector using [`vec(...)`](#vec).

## Points

### Point component access

!!! warning "Point component access"

    Accessing components of points (such as `p.x`) does **not** return a coordinate of the point unless the point is unitized: use either `p.unit.x` or `p.x / p.e0`.

Points are represented as 1-blades, with an e~0~ component. Once a point has been unitized using `.unit` (see [Blade fields](#blade-fields)), its components can be accessed the same as a vector. The e~0~ component can be accessed with `.e0`

## General blades

### Blade indexing

Indexing a blade using a string of [vector index](#vector-component-access) characters, with the addition of `0` for the e~0~ component. Additionally, the characters `s`, `e`, `n`, `_`, and ` ` (space) are ignored. By combining these, any component of the blade is accessible.

??? example "Example using blade component access"

    ```lua
    print(my_scalar_blade.s) -- scalar component

    print(my_bivector_blade.xy) -- xy bivector component
    assert(my_bivector_blade.xy == -my_bivector_blade.yx)

    print(my_trivector_blade.e0_xy) -- e₀xy trivector component
    ```

### Blade fields

Blades have the following fields:

<div class="annotate" markdown>

- `.ndim` is the number of dimensions of the space containing the blade
- `.grade` is the [grade] of the blade (1)
- `.antigrade` is the [antigrade][grade] of the blade (2)
- `.dual` is the [metric dual] of the blade
- `.antidual` is the [metric antidual] of the blade
- `.complement` is the [right complement] of the blade
- `.unit` returns the [unitization] of a blade if it has nonzero [weight][bulk and weight], or its [bulk normalization][bulk norm] otherwise (3)
- `.mag2` is the [geometric norm] squared (4)
- `.mag` is the [geometric norm] (5)
- `.bulk` is the [bulk][bulk and weight] of the blade (6)
- `.weight` is the [weight][bulk and weight] of the blade (7)

</div>

1. A vector or point always has grade `1`, a bivector has grade `2`, etc.
2. A hyperplane always has antigrade `1`.
3. For vectors, this is a unit vector in the same direction. For points, this is a unitized point whose components are the point's coordinates.
4. For vectors, this is the squared length of the vector.
5. For vectors, this is the length of the vector.
6. The bulk is a blade comprised of only components with an e~0~ factor.
7. The bulk is a blade comprised of only components _without_ an e~0~ factor.

??? example "Useful constructions"

    - `.bulk.mag` is the [bulk norm] of the blade
    - `.weight.mag` is the [weight norm] of the blade
    - `b / b.weight.mag` is the [unitization] of a blade `b`

[grade]: https://rigidgeometricalgebra.org/wiki/index.php?title=Grade_and_antigrade
[metric dual]: https://rigidgeometricalgebra.org/wiki/index.php?title=Duals#Dual
[metric antidual]: https://rigidgeometricalgebra.org/wiki/index.php?title=Duals#Antidual
[right complement]: https://rigidgeometricalgebra.org/wiki/index.php?title=Complements
[geometric norm]: https://rigidgeometricalgebra.org/wiki/index.php?title=Geometric_norm#Geometric_Norm
[bulk and weight]: https://rigidgeometricalgebra.org/wiki/index.php?title=Bulk_and_weight
[bulk norm]: https://rigidgeometricalgebra.org/wiki/index.php?title=Geometric_norm#Bulk_Norm
[weight norm]: https://rigidgeometricalgebra.org/wiki/index.php?title=Geometric_norm#Weight_Norm
[unitization]: https://rigidgeometricalgebra.org/wiki/index.php?title=Unitization

### Blade methods

- `:projected_to(other)` returns the component of the blade that is parallel to another blade `other` (i.e., its [projection] onto `other`)
- `:rejected_from(other)` returns the component of the blade that is perpendicular to another blade `other`
- `:wedge(other)` returns the [wedge product] between two blades
- `:antiwedge(other)` returns the [antiwedge product] between two blades
- `:dot(other)` returns the [dot product] between two blades
- `:antidot(other)` returns the [antidot product] between two blades

[projection]: https://rigidgeometricalgebra.org/wiki/index.php?title=Projections#Projection
[wedge product]: https://rigidgeometricalgebra.org/wiki/index.php?title=Exterior_products#Exterior_Product
[antiwedge product]: https://rigidgeometricalgebra.org/wiki/index.php?title=Exterior_products#Exterior_Antiproduct
[dot product]: https://rigidgeometricalgebra.org/wiki/index.php?title=Dot_products#Dot_Product
[antidot product]: https://rigidgeometricalgebra.org/wiki/index.php?title=Dot_products#Antidot_Product

### Blade operations

- `blade + blade`
- `blade - blade`
- `blade * float`
- `float * blade`
- `blade / float`
- `-blade`
- `blade ^ blade` ([wedge product])
- `blade & blade` ([antiwedge product])
- `~blade` ([right complement])
- `blade == blade` ([approximate floating-point equality])
- `blade ~= blade` ([approximate floating-point inequality])
- `blade[index]` (see [Blade indexing](#blade-indexing))
- `type(blade)` returns `'blade'`
- `tostring(blade)`
- `pairs(blade)` iterates over the key/value pairs for a vector or point

[approximate floating-point equality]: basic.md#approximate-equality
[approximate floating-point inequality]: basic.md#approximate-equality

??? example "Example using `pairs(blade)`"

    ```lua
    local v = vec(10, 20, 30)
    for i, x in pairs(v) do
      print(i) -- 1, 2, 3
      print(x) -- 10, 20, 30
    end
    ```
