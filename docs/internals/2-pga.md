# Projective GA

VGA gives us the tools to represent vectors, planes, etc. that intersect the origin, and rotations/reflections around the origin. **PGA** (**Projective Geometric Algebra**) adds a new basis vector $w$, and uses it to represent points, lines, and planes _anywhere_ in Euclidean space[^projective], and any isometry of Euclidean space.

[^projective]: Actually, anywhere in _projective space_, which is a superset of Euclidean space!

!!! question "What is an isometry?"
    An **isometry** is a distance-preserving transformation. (The word "isometry" literally means "same measure.") In Euclidean space, this is some combination of translations, rotations, and reflections.

!!! failure "Is $w$ the :sparkles: _fourth dimension_ :sparkles:?"
    No, we're not talking about 4D space yet. $w$ is **not** an ordinary spatial dimension, it's just an algebraic tool. Everything discussed here can be generalized to any number of dimensions.

## Projective plane

In projective GA, we keep $x^2 = y^2 = z^2 = 1$. But our new basis vector $w$ is a **null vector**, which means it squares to zero: $w^2 = 0$. Let's stick to 2D for now, so that we only have three basis vectors: $x$, $y$, and $w$.

## Points, lines, and planes

PGA gives us the ability to represent a point as a vector. A point at coordinates $\langle a, b \rangle$ is represented with the vector $ax + by + w$. The origin $\langle 0, 0 \rangle$ is represented with the vector $w$. These vectors can be scaled arbitrarily and still represent the same point. For example, these blades all represent $\langle 2, -1 \rangle$:

- $2x-y+w$
- $-2x+y-w$
- $10x-5y+5w$

This gives the geometry of the **[projective plane]** (or in general, a **[projective space]**), where we have all the finite points $\langle x, y \rangle$ plus a **[line at infinity]** that can be reached by traveling infinitely far in any direction. (In 3D+, I like to think of it as half of a [skybox].) Points on the line at infinity are represented using vectors with zero $w$ component, such as $-3x+2y$.

[projective plane]: https://en.wikipedia.org/wiki/Projective_plane
[projective space]: https://en.wikipedia.org/wiki/Projective_space
[line at infinity]: https://en.wikipedia.org/wiki/Line_at_infinity
[skybox]: https://en.wikipedia.org/wiki/Skybox_(video_games)

!!! info "Projective geometry"
    The projective plane has some nice properties:

    - Every pair of distinct points has exactly one line passing through both.
    - Every pair of lines intersects at exactly one point, even if they are parallel.

Here's some examples of how this works:

- Given two points $p$ and $q$ represented using vectors, $p \wedge q$ is the line containing $p$ and $q$. Iff[^iff] $p$ and $q$ are the same point, then $p \wedge q = 0$.
- Given a line $l$ represented using a bivector and a point $p$ represented using a vector, $l \wedge p$ is the plane containing $l$ and $p$. Iff $p$ is already on $l$, then $l \wedge p = 0$.

[^iff]: "iff" is short for [if and only if](https://en.wikipedia.org/wiki/If_and_only_if).

In general, any blade $C_r$ intersects the projective plane $w=1$ at an $(r-1)$-dimensional subspace $S$, so we say that $C_r$ represents $S$. If $C_r$ has no $w$ component, we say it represents a subspace at infinity. (In 2D, that's the line at infinity or a point at infinity.)

## Outer product

The outer product works here too: The outer product $C_r \wedge D_s$ gives the representation of the unique $(r+s-1)$-dimensional subspace containing $C_r$ and $D_s$. If $C_r$ and $D_s$ are in the same $(r+s-2)$-dimensional subspace, then there isn't a unique $(r+s-1)$-dimensional subspace containing them, so $C_r \wedge D_s = 0$.

## Which side?

We can use this to check if a point $p$ is contained in a subspace $C_r$: just check whether $C_r \wedge p$ is zero. In $N$-dimensional space, we can check which side of a hyperplane $H$ (represented as an $N$-blade) contains $p$ using the sign of  we can check $C_n$ If the point is not contained in the subspace, then the sign of $C_r \wedge p$ is indicates which side of $C_r$

## Motors

Remember rotors, our friends from the even subalgebra of VGA? They've gotten an upgrade in PGA: they're now called **motors** and can represent arbitrary _translations_ in addition to rotations! And of course we have **flectors**, which use the odd subalgebra and represent any combination of translations, rotations, and an odd number of reflections. You can use them exactly the same way you use rotors.
