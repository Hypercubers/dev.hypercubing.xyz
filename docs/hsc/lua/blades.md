# Blades

A **blade** is a geometric algebra primitive that can be used to represent a vector, a point, a line, a hyperplane, or numerous other geometric objects. Hyperspeedcube uses blades from the [Projective Geometric Algebra](https://en.wikipedia.org/wiki/Plane-based_geometric_algebra#Projective_Geometric_Algebra), but knowledge of geometric algebra is not required in order to use the API effectively.

Blades cannot be mutated once [constructed](#constructors). To modify a blade, you must construct a new blade and then replace the old one.

!!! info "The examples on this page assume 3D, but the same API works in other dimensions."


## Constructors

All these constructors return a blade with the number of dimensions of the currently active space, and therefore can only be called in a context with a global number of dimensions (such as during the construction of a puzzle). They produce an error if it is called when there is not a global number of dimensions.

### `vec()`

`vec()` constructs a [vector](#vectors) and can be called in any of several ways:

- **No arguments.** Calling `vec()` with no arguments returns the zero vector. For example, `vec()` constructs the blade $0$, which represents the vector $\langle 0, 0, 0 \rangle$.
- **Components.** Calling `vec()` with multiple numbers constructs a vector with those components. For example, `vec(10, 20, 30)` constructs the blade $10x+20y+30z$, which represents the vector $\langle 10, 20, 30 \rangle$.
- **Axis name.** Calling `vec()` with a single-character axis string constructs a unit vector along that axis. For example, `vec('y')` constructs the blade $y$, which represents the vector $\langle 0, 1, 0 \rangle$.
- **Table.** Calling `vec()` with a table as its argument assigns each key-value pair to a component of the vector. The key may be a number or a string, using the same mapping as [vector component access](#vector-component-access). For example, `vec{10, z = 30}`[^omit-braces] constructs the blade $10x+30y$, which represents the vector $\langle 10, 0, 30 \rangle$.
- **Vector.** Calling `vec()` with an existing vector will return the vector unmodified.
- **Point.** Calling `vec()` with an existing point will unitize the point then return its coordinates as a vector.

Extra components beyond the number of dimensions of the space are ignored.

### `point()`

`point()` constructs a [point](#points), using the same argument structure as [`vec()`](#vec). The returned point is [unitized][unitization].

### `blade()`

`blade()` constructs a [blade](#general-blades) and can be called in either of two ways:

- **Scalar.** Calling `blade()` with a scalar value returns a scalar blade. For example, `blade(6.5)` returns the blade $6.5$.
- **Blade.** Calling `blade()` with an existing point, vector, or blade returns the value unmodified.

### `plane()`

`plane()` constructs a [blade](#general-blades) representing a hyperplane and can be called in any of several ways:

- **Table.** There are several ways to construct a plane using a table:
    - Calling `plane{normal: vector, distance: number}`[^omit-braces] constructs the plane with normal vector `normal` and distance from the origin `distance`.
    - Calling `plane{normal: vector, point: point}` constructs the plane with normal vector `normal` that passes through the point `point`.
    - Calling `plane{pole: vector|point}` constructs the plane with `pole` that passes through the point `pole`. `plane{pole = p}` is equivalent to `plane{normal = p, point = p}` or `plane{normal = p, distance = p.mag}`.
- **Normal vector and distance.** Calling `plane()` with a vector and a number constructs the plane with the given normal vector and distance from the origin. `plane(n, d)` is equivalent to `plane{normal = n, distance = d}`.
- **Pole vector.** Calling `plane()` with a vector or point constructs the plane with that vector as a normal vector that passes through that point. `plane(p)` is equivalent to `plane{pole = p}`.
- **Plane.** Calling `plane()` with a blade representing a hyperplane returns the blade unchanged.

```lua title="Examples of plane construction"
-- plane through point (2, -3, 6) with normal vector (2/7, -3/7, 6/7)
plane(vec(2, -3, 6))
plane(point(2, -3, 6))

-- plane through point (1, 0, 0) perpendicular to the X axis
plane('x')
plane{pole = vec(1)} -- note the curly braces!
plane{normal = vec(3), distance = 1}

-- plane through point (1, 0, 0) perpendicular to the X axis, but facing the other way
-plane('x')
plane(-vec('x'), -1)

-- plane through the origin with normal vector (0, 0, 1)
plane('z', 0)
plane{normal = 'z', point = vec()}
plane{normal = 'z', distance = 0}
```

## Vectors

A vector is a direction in space and an associated magnitude. Vectors can be constructed using [`vec()`](#vec). Vectors are also constructed automatically by functions that require them, so you can often omit the call to `vec()`.

Vectors are represented as 1-blades with no e~0~ component.

### Vector component access

Vectors can be indexed by positive integers or single-character strings. The following single-letter and single-digit strings are recognized, uppercase or lowercase:

- `my_vector.X` = `my_vector[1]` = X axis
- `my_vector.Y` = `my_vector[2]` = Y axis
- `my_vector.Z` = `my_vector[3]` = Z axis
- `my_vector.W` = `my_vector[4]` = W axis (4th dimension)
- `my_vector.V` = `my_vector[5]` = V axis (5th dimension)
- `my_vector.U` = `my_vector[6]` = U axis (6th dimension)
- `my_vector[7]` = 7th dimension

```lua title="Examples of vector indexing"
local my_vector = vec(10, 20, 30)
assert(my_vector.x == 10)
assert(my_vector.Y == 20)
assert(my_vector[3] == 30)
```

### Vector fields

Vectors have the following fields:

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
- `vector == vector` (uses [approximate] floating-point comparison)
- `vector ~= vector` (uses [approximate] floating-point comparison)
- `type(vector)` (returns `'blade'`)
- `tostring(vector)`
- `pairs(vector)`

[approximate]: basic.md#approximate-equality

For `+` and `-`, either of the two vectors may be substituted for a value of any other type and it will be converted to a vector using [`vec(...)`](#vec).

## Points

A point is a point in space. Points can be constructed using [`point()`](#point). Points are also constructed automatically by functions that require them, so you can often omit the call to `point()`.

Points are represented as 1-blades, with an e~0~ component.

See [General blades](#general-blades) for fields, methods, and operations on points.

!!! warning "Operations on points"

    Be very cautious when doing operations on points. You can add two points to compute a position between them, but ensure that they are unitized first.

### Point component access

!!! warning "Point component access"

    Accessing components of points (such as `p.x`) does **not** return a coordinate of the point unless the point is unitized: use either `p.unit.x` or `p.x / p.e0`.

Once a point has been unitized using `.unit` (see [Blade fields](#blade-fields)), its components can be accessed the same as a vector. The e~0~ component can be accessed with `.e0`.

## Hyperplanes

A hyperplane is an oriented flat $d-1$-dimensional surface in $d$-dimensional space. Hyperplanes can be constructed using [`plane()`](#plane). Hyperplanes are also constructed automatically by functions that require them, so you can often omit the call to `plane()`.

Hyperplanes are represented as $d$-blades.

See [General blades](#general-blades) for fields, methods, and operations on hyperplanes.

## General blades

### Blade indexing

Indexing a blade using a string of [vector index](#vector-component-access) characters, with the addition of `0` for the e~0~ component. Additionally, the characters `s`, `e`, `n`, `_`, and ` ` (space) are ignored. By combining these, any component of the blade is accessible.

```lua title="Examples of blade component access"
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
- `.antigrade` is the [antigrade] of the blade (2)
- `.dual` is the [metric dual] of the blade
- `.antidual` is the [metric antidual] of the blade
- `.complement` is the [right complement] of the blade
- `.unit` returns the [unitization] of a blade if it has nonzero [weight], or its [bulk normalization][bulk norm] if it has zero [weight] (3)
- `.mag2` is the [geometric norm] squared (4)
- `.mag` is the [geometric norm] (5)
- `.bulk` is the [bulk] of the blade (6)
- `.weight` is the [weight] of the blade (7)

</div>

1. A vector or point always has grade `1`, a bivector has grade `2`, etc.
2. A hyperplane always has antigrade `1`.
3. For a vector, this is a unit vector in the same direction. For a point, this is a unitized point whose components are the point's coordinates.
4. For a vector, this is the squared length of the vector.
5. For a vector, this is the length of the vector.
6. The bulk is a blade comprised of only components with an e~0~ factor.
7. The bulk is a blade comprised of only components _without_ an e~0~ factor.

!!! example "Useful constructions"

    - `.bulk.mag` is the [bulk norm] of the blade
    - `.weight.mag` is the [weight norm] of the blade
    - `b / b.weight.mag` is the [unitization] of a blade `b`

[grade]: https://rigidgeometricalgebra.org/wiki/index.php?title=Grade_and_antigrade
[antigrade]: https://rigidgeometricalgebra.org/wiki/index.php?title=Grade_and_antigrade
[metric dual]: https://rigidgeometricalgebra.org/wiki/index.php?title=Duals#Dual
[metric antidual]: https://rigidgeometricalgebra.org/wiki/index.php?title=Duals#Antidual
[right complement]: https://rigidgeometricalgebra.org/wiki/index.php?title=Complements
[geometric norm]: https://rigidgeometricalgebra.org/wiki/index.php?title=Geometric_norm#Geometric_Norm
[bulk]: https://rigidgeometricalgebra.org/wiki/index.php?title=Bulk_and_weight
[weight]: https://rigidgeometricalgebra.org/wiki/index.php?title=Bulk_and_weight
[bulk norm]: https://rigidgeometricalgebra.org/wiki/index.php?title=Geometric_norm#Bulk_Norm
[weight norm]: https://rigidgeometricalgebra.org/wiki/index.php?title=Geometric_norm#Weight_Norm
[unitization]: https://rigidgeometricalgebra.org/wiki/index.php?title=Unitization

### Blade methods

Blades have the following methods:

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

Blades support the following operations:

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

<!-- Footnotes -->

[^omit-braces]: Lua syntax allows omitting the parentheses when calling a function with a table literal or string literal as its only argument. For example, you can write `vec{10, z = 30}` instead of `cd({10, z = 30})`.
