# Hyperplane

A **hyperplane** is an oriented flat $(d-1)$-dimensional surface in $d$-dimensional space. The orientation of a hyperplane is determined by its normal vector; to flip a hyperplane's orientation, negate its normal vector.

Hyperplanes are also constructed automatically by functions that require them, so you can often omit the call to the constructor `plane()`.

Hyperplanes cannot be mutated once [constructed](#constructors). To modify a hyperplane, you must construct a new hyperplane and then replace the old one.

!!! info "The examples on this page assume 3D, but the same API works in other dimensions."

## Constructors

All these constructors return a hyperplane with the number of dimensions of the currently active space, and therefore can only be called in a context with a global number of dimensions (such as during the construction of a puzzle). They produce an error if it is called when there is not a global number of dimensions.

### `plane()`

`plane()` constructs a hyperplane and can be called in any of several ways:

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

## Fields

Hyperplanes have the following fields:

- `.ndim` is the number of dimensions of the space containing the blade
- `.flip` is the same hyperplane, facing the opposite direction
- `.normal` is the (normalized) normal vector of the hyperplane
- `.distance` is the minimum distance of the hyperplane from the origin
- `.blade` is the [blade](blade.md) representing the hyperplane
- `.region` is the [region](region.md) of space bounded on the side of the hyperplane facing away from the normal vector

## Methods

Hyperplanes have the following methods:

- [`:signed_distance()`](#hyperplanesigned_distance)

### `hyperplane:signed_distance()`

`hyperplane:signed_distance()` returns the signed distance of a point to a hyperplane. It takes one argument: a point. The distance returned is zero if the point is on the hyperplane, positive if the point is in the direction of the hyperplane's normal vector, or negative if the point is in the opposite direction.

## Operations

Hyperplanes support the following operations:

- `hyperplane == hyperplane` ([approximate floating-point equality])
- `hyperplane ~= hyperplane` ([approximate floating-point inequality])
- `type(hyperplane)` returns `'hyperplane'`
- `tostring(hyperplane)`

[approximate floating-point equality]: ../basics.md#approximate-equality
[approximate floating-point inequality]: ../basics.md#approximate-equality
