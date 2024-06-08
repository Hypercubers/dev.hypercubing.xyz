# Transforms

A **transform** is a combination of some sequence of translations, rotations, and reflections in space. Hyperspeedcube uses motors from the [Projective Geometric Algebra](https://en.wikipedia.org/wiki/Plane-based_geometric_algebra#Projective_Geometric_Algebra), but knowledge of geometric algebra is not required in order to use the API effectively.

!!! info "The examples on this page assume 3D, but the same API works in other dimensions."

[rotation]: https://en.wikipedia.org/wiki/Rotation_(mathematics)
[reflection]: https://en.wikipedia.org/wiki/Reflection_(mathematics)
[point reflection]: https://en.wikipedia.org/wiki/Point_reflection

[blade]: blades.md
[point]: blades.md#points
[vector]: blades.md#vectors
[hyperplane]: blades.md#hyperplanes

## Why not matrices?

Although transforms are commonly represented using matrices, motors provide a few advantages:

- Fewer numbers to store (below 6D)
- Good for interpolation
- Can distinguish between 180-degree rotations in opposite directions, which is good for displaying correct animations

## Constructors

All these constructors return a transform with the number of dimensions of the currently active space, and therefore can only be called in a context with a global number of dimensions (such as during the construction of a puzzle). They produce an error if it is called when there is not a global number of dimensions.

### `ident()`

`ident()` constructs the identity transformation. It takes no arguments.

### `refl()`

`refl()` constructs a transform representing a [reflection] or [point reflection] and can be called in any of several ways:

- **No arguments.** Calling `refl()` with no arguments constructs a point reflection across the origin.
- **Point.** Calling `refl()` with a [point] constructs a point reflection across that point. For example, `refl(point(0, -2))` constructs a point reflection across the origin.
- **Vector.** Calling `refl()` with a [vector] constructs a reflection through that vector. The magnitude of the vector is ignored. For example, `refl(point(0, -2))` constructs a reflection across the plane $y=0$.
- **Hyperplane.** Calling `refl()` with a [hyperplane] constructs a reflection across that hyperplane. The orientation of the plane is ignored. For example, `refl(plane('z', 1/2))` constructs a reflection across the plane $z = 0.5$.

### `rot()`

`rot()` constructs a transform representing a [rotation] fixing the origin and takes several named arguments as values in a table:

- **`from`** is a vector. The magnitude of the vector is ignored.
- **`to`** is the destination vector for `from` once the rotation has been applied. The magnitude of the vector is ignored.
- **`fix`** is a [blade] to keep fixed during the rotation.
- **`angle`** is the angle of the rotation in radians.

Any combination of these may be specified, subject to the following constraints:

<div class="annotate" markdown>

- `from` and `to` are mutually dependent; i.e., one cannot be specified without the other.
- If `from` and `to` are not specified, then `fix` and `angle` are both required and `fix` must be dual to a 2D plane. (1)
- `from` and `to` must not be opposite each other. (2)

If `from` and `to` are specified, then the rotation is constructed using these steps:

1. If `fix` is specified, then `from` and `to` are [orthogonally rejected] from `fix`. (3)
2. If `angle` is specified, then `to` is minimally adjusted to have the angle `angle` with respect to `from`.
3. `from` and `to` are normalized.
4. A rotation is constructed that takes `from` to `to` along the shortest path.

If `from` and `to` are not specified, then a rotation of `angle` around `fix` is constructed. This method is not recommended because the direction of the rotation is unspecified and may change depending on the sign of `fix`.

</div>

1. This is possible with [antigrade] 3 (if its [bulk] is zero) or [antigrade] 2 (if its [bulk] is nonzero). For example, in 3D, `fix` must be one of the following:
    - A vector (zero [bulk], [grade] 1, [antigrade] 3)
    - A line (nonzero [bulk], [grade] 2, [antigrade] 2)
2. There are many 180-degree rotations that take any given vector to its opposite, so this case is disallowed due to ambiguity.
3. This results in the component of each vector that is perpendicular to `fix`.

[grade]: https://rigidgeometricalgebra.org/wiki/index.php?title=Grade_and_antigrade
[antigrade]: https://rigidgeometricalgebra.org/wiki/index.php?title=Grade_and_antigrade
[bulk]: https://rigidgeometricalgebra.org/wiki/index.php?title=Bulk_and_weight

[orthogonally rejected]: https://en.wikipedia.org/wiki/Vector_projection

```lua title="Examples of rotation construction"
-- 45-degree rotation in the XY plane
rot{from = 'x', to = vec(1, 1, 0)}

-- 180-degree rotation around the Z axis
rot{fix = 'z', angle = pi}

-- Jumbling rotation of the Curvy Copter puzzle
rot{fix = vec(1, 1, 0), angle = acos(1/3)}

-- 90-degree rotation around an edge of a cube
rot{fix = vec(1, 1, 0), from = 'x', to = 'z'}
```

## Fields

Transforms have the following fields:

- `.ndim` is the number of dimensions of the space containing the transform
- `.is_ident` is `true` if the transform is equivalent to the identity transform, or `false` otherwise
- `.is_refl` is `true` if the transform is equivalent to an odd number of reflections, or `false` otherwise[^even-refl]
- `.rev` is the reverse transform

[^even-refl]: Note that in an even number of dimensions (2D, 4D, etc.) a point reflection contains an _even_ number of reflections, so `refl().is_refl` is `false` in those dimensions.

## Methods

Transforms have the following method:

- `:transform(object)` returns the object transformed according to the transform

Below is a list of all types that can be transformed:

- [Blades][blade], including [vectors][vector] and [points][point]
- Other transforms
- [Axes](puzzles.md#axes)
- [Colors](puzzles.md#colors)
- [Twists](puzzles.md#twists)

## Operations

Transforms support the following operations:

- `transform * transform` (composition of transforms)
- `transform == transform` (uses [approximate] floating-point comparison)
- `transform ~= transform` (uses [approximate] floating-point comparison)
- `type(transform)` (returns `'transform'`)
- `tostring(transform)`

[approximate]: basic.md#approximate-equality
