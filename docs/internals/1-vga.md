# Vectorspace GA

Complex numbers are a system where each number has two components: real and imaginary. Geometric Algebra is like complex numbers, but adds _way_ more components.[^complex] For example, **3D VGA** (**Vectorspace Geometric Algebra** or **Vanilla Geometric Algebra**) adds _seven_ extra components a total of eight.

[^complex]: In fact, complex numbers _are_ a geometric algebra! You can think of them either as a 1D GA with a single extra basis vector $i^2=-1$, or as the even subalgebra of 2D VGA -- but I'm getting ahead of myself.

## Multivectors

All geometric algebra systems have **scalars**, the ordinary positive and negative numbers you know and love. In 3D VGA, we also have **vector** components $x$, $y$, and $z$, and we can build vectors out of them. For example, $x-7z$ represents the vector $\langle 1, 0, -7 \rangle$. Just like a complex number has both real and imaginary components (e.g., $3+2i$), a **multivector** represented in VGA can have both scalar and vector components (e.g., $5+3x+y-2z$). Each of these components is a **term**

You might recognize this as a **[vector space]**, which just means that addition, subtraction, and scalar multiplication all work how you expect. But 3D VGA isn't actually a 3D vector space, because we have $1$, the unit scalar, as a fourth **basis vector**, orthogonal to $x$, $y$, and $z$. Don't worry, you can still visualize it using just three dimensions by treating the scalar component as separate.

[vector space]: https://en.wikipedia.org/wiki/Vector_space

!!! warning "Confusing terminology"
    A "multivector" is any element of the vector space, while a "vector" in GA consists of only vector components, like $2x-y$ (but not $4+2x-y$). But the term "basis vector" applies in the vector space as a whole, so $1$, $x$, $y$, and $z$ are all basis vectors. Blame the mathematicians, not me.

So we have four basis vectors: $1$, $x$, $y$, and $z$. But earlier I said that 3D VGA actually has _eight_ basis vectors. How do we get all those extra basis vectors?

## Geometric product

!!! info "Conventions"
    - $a$, $b$, $c$, $d$ are arbitrary **vectors**
    - $A$, $B$, $C$, $D$ are arbitrary **multivectors**

The geometric product is how we generalize multiplication to work on multivectors. We write it using ordinary multiplication, so $AB$ is the geometric product of $A$ and $B$. In code, the geometric product of `a` and `b` is written `a * b`. For scalars, it does what you expect. The geometric product has some nice properties:

- :white_check_mark: Associativity[^assoc]: $A(BC) = (AB)C$
- :white_check_mark: Distributivity: $A(B + C) = AB + AC$ and $(B + C)A = BA + CA$
- :x: No commutativity: $AB = BA$ isn't always true

[^assoc]: If you ever see a non-associative algebra (like the [octonions](https://en.wikipedia.org/wiki/Octonion)), run far, far away and never look back. They are utter hell.

The geometric product of anything involving a scalar is just scalar multiplication, which does commute: $x3 = 3x$. But what if we multiply two _vector_ components? If the vectors are the same, like $xx$, then they cancel and the result is $1$. If the vectors are different, like $xy$, then we get something new: a **bivector**.

There are three unique bivectors in VGA: $xy$, $xz$, and $yz$. You can swap the letters of a bivector if you swap the sign, like $xy = -yx$, so we don't need a separate $yx$ component.

Let's make a multiplication table! Check for yourself that each entry in here makes sense. (left times top)

| $1$ |  $x$  |  $y$  | $z$  |
| --- | :---: | :---: | :--: |
| $x$ |  $1$  | $xy$  | $xz$ |
| $y$ | $-xy$ |  $1$  | $yz$ |
| $z$ | $-xz$ | $-yz$ | $1$  |

What happens when you multiply a bivector by a vector? Well when we multiply $xy$ by $x$, we get $xyx$. We can simplify $yx$ to $-xy$, and then $xx$ simplifies to $1$, so:

$$
\begin{align}
xyx &= x(-xy) \\
&= -xxy \\
&= -y
\end{align}
$$

But if we have three different letters, then it doesn't simplify: multiplying $xy$ by $z$ results in the **trivector** $xyz$, our eighth and final basis vector. In general, a geometric algebra with $n$ vector components will have $2^n$ basis vectors.

Let's make an even bigger multiplication table!

| $1$   |  $x$  |  $y$   |  $z$  | $xy$  |  $xz$  | $yz$  | $xyz$ |
| ----- | :---: | :----: | :---: | :---: | :----: | :---: | :---: |
| $x$   |  $1$  |  $xy$  | $xz$  |  $y$  |  $z$   | $xyz$ | $yz$  |
| $y$   | $-xy$ |  $1$   | $yz$  | $-x$  | $-xyz$ |  $z$  | $-xz$ |
| $z$   | $-xz$ | $-yz$  |  $1$  | $xyz$ |  $-x$  | $-y$  | $xy$  |
| $xy$  | $-y$  |  $x$   | $xyz$ | $-1$  | $-yz$  | $xz$  | $-z$  |
| $xz$  | $-z$  | $-xyz$ |  $x$  | $yz$  |  $-1$  | $-xy$ |  $y$  |
| $yz$  | $xyz$ |  $-z$  |  $y$  | $-xz$ |  $xy$  | $-1$  | $-x$  |
| $xyz$ | $yz$  | $-xz$  | $xy$  | $-z$  |  $y$   | $-x$  | $-1$  |

But what do all these components actually represent? Well just like the vector $x$ represents one unit of _distance_ in the positive direction along the X axis, the bivector $xy$ represents one unit of _positively-oriented area_ in the XY plane. And a trivector $xyz$ represents one unit of _positively-oriented volume_ in 3D space. Orientation is just a thing that changes when you flip the sign, so $xy$ has positive orientation and $-xy$ has negative orientation. Don't worry if that doesn't make much sense.

A bivector with multiple components represents non-axis-aligned area. Consider the vector $x+2y$, which represents some length in the XY plane. Its projection onto the X axis is one unit long, and its projection onto the Y axis is two units long. Similarly, $2xy+xz-3yz$ represents an oriented area in the XYZ 3D space. Its projection (2D shadow) onto the XY plane has two units of positively-oriented area, its projection onto the XZ plane has 1 unit of positively-oriented area, and its projection onto the YZ plane has 3 units of negatively-oriented area. This is hard to visualize if you haven't seen it before; check out [Marc ten Bosch's explanation of rotors][marctenbosch-rotors] if you're confused.

## Blades and grades

It's time for some new terminology:

- The **grade** of a multivector is the number of letters each component has. A scalar has grade 0, a vector has grade 1, a bivector has grade 2, a trivector has grade 3, etc.
- A **blade** is a multivector whose components all have the same grade.
- An **$r$-blade** is a blade with grade $r$ (where $r$ is a nonnegative integer).
- The **pseudoscalar** $I$ (or `pss` in code) is the unit-length multivector with maximum grade. In 3D space, $I = xyz$.

To get a blade from a multivector, you can **grade-project** it, which extracts all the components of a particular grade. The projection of $A$ into grade $r$ is written $\langle A \rangle_r$.

Most of the time, all the multivectors we see will be blades. The only exception to this rule is rotors, which we'll get to later.

!!! info "Conventions"
    - $C_r$ is an arbitrary **blade** with **grade** $r$
    - $D_s$ is an arbitrary **blade** with **grade** $s$

## Outer product (wedge product)

The **wedge product** or **outer product** of $C_r$ and $D_s$ is written $C_r \wedge D_s$ ("$C_r$ wedge $D_s$") and is defined as $\langle C_r D_s \rangle_{r+s}$. In code, the wedge product of `a` and `b` is written `a ^ b`.

To give an intuitive understanding: When computing the geometric product of two multivectors, you get a lot of different components of different grades. The outer product selects only the components with the maximum possible grade. Here's some examples of how that works:

- The outer product of two vectors is always a bivector, with no scalar component (or zero, if the vectors are parallel).
- The outer product of a bivector and a vector is always a trivector (or zero, if the vector is in the same plane as the bivector).
- The outer product of a trivector and a vector is always a quadvector or zero. In 3D VGA, we have no quadvectors, so it is always zero.
- The outer product with a scalar is the same as ordinary multiplication by that scalar.

!!! example inline end "Outer product visualization"
    ![Geometric interpretation of grade n elements in a real exterior algebra for n from 0 to 3](https://upload.wikimedia.org/wikipedia/commons/2/27/N_vector_positive.svg)
    _[CC0 from Wikipedia](https://commons.wikimedia.org/wiki/File:N_vector_positive.svg)_

    Click for full size

The outer product has a really nice geometric interpretation. The outer product of two vectors $a \wedge b$ is a bivector representing the oriented area swept out by moving $b$ along $a$. Adding a third vector gives $a \wedge b \wedge c$, a _trivector_ that represents the oriented _volume_ generated by sweeping $c$ over the whole surface of $a \wedge b$.

This area and volume are _oriented_ because they face in a particular direction. In the image to the right, this is represented by the circular orange arrows. If you swap any two adjacent vectors in the outer product, then the sign of the result flips. In general, $a \wedge b = -b \wedge a$. This property is called **anticommutativity**, and it will be a common theme as we learn more about geometric algebra. Note the outer product is anticommutative for vectors, but not generally for multivectors. Later we'll learn about how to compute these operations and how to know when the sign changes.

!!! success "This looks suspiciously similar to the determinant of a matrix ..."
    This is no coincidence! The determinant of a matrix is exactly equal to the pseudoscalar component of the outer product of its column vectors. Think about properties of the determinant, like how behaves when you swap adjacent columns or its representation of signed area/volume.

## Inner product

There's a few different ways to generalize inner products to work on multivectors.

### Scalar dot product

The **scalar dot product** (or simply **dot product**) of two multivectors $A$ and $B$ is written $A \cdot B$ ("$A$ dot $B$") and is defined as $\langle AB \rangle_{0}$, i.e., the scalar component of the geometric product. This is one of several **inner products**. The scalar dot product is commutative for vectors (so $a \cdot b = b \cdot a$) but not generally for multivectors.

!!! tip "Exercise"
    Think about why the dot product of two blades with different grades is always zero.

!!! warning "Common misconception: $AB = A \wedge B + A \cdot B$"
    There's a misconception in geometric algebra that the geometric product is the sum of the outer and inner products. This is true when multiplying vectors (so $ab = a \wedge b + a \cdot b$ is true) but not generally for multivectors (so $AB = A \wedge B + A \cdot B$ does _not_ always hold).

Using the dot product, we can also define the **magnitude** of a multivector $\|A\| = \sqrt{A \cdot A}$. Note that the magnitude of a multivector is not always real; for example, the $xy \cdot xy = -1$, so its magnitude would be $\sqrt{-1}$, which is imaginary.

### Contraction

The **left contraction** of $C_r$ and $D_s$ is written $C_r \rfloor D_s$ ("$C_r$ left-contract $D_s$" or "$C_r$ contracted from $D_s$) and is defined as $\langle C_r D_s \rangle_{s-r}$. In code, the left contraction of `a` and `b` is written `a << b`.

!!! tip "Exercise"
    1. When is left contraction equivalent to the scalar dot product?
    2. **:sparkles: Bonus :sparkles:** When is left contraction equivalent to the geometric product? (The answer is revealed later, in case you're stuck.)

I have to admit that I don't have a great geometric understanding of left contraction. My understanding is that it's sort of a way to "remove" some multivector from a product of vectors, but that intuition may be wrong.

For completeness: the **right contraction** of $C_r$ and $D_s$ is written $C_r \lfloor D_s$ and is defined as $\langle C_r D_s \rangle_{r-s}$, but we'll never use it.

## Reverse

The **reverse** of a multivector $A$ is written $A^\dagger$. It's computed by reversing the order of all the basis vectors in each component. For example, the reverse of $2x + 3xz + 4xyz$ is $2x + 3zx + 4zyx$. Using the rule that $xy = -yx$, we can simplify this to $2x - 3xz - 4xyz$.

In general, a multivector can be reversed by negating each of its components with grade $1$ or $2$ mod 4. Components with grade $0$ or $3$ mod 4 remain unchanged.

## Inverse

The multiplicative **inverse** of a multivector is written $A^{-1}$. It's computed using the formula $A^{-1} = \frac{A^\dagger}{A A^\dagger}$, which I [stole from Mathematics Stack Exchange](https://math.stackexchange.com/a/443680/1115019). This means we can technically do division with multivectors? Wacky!

Not all multivectors have a multiplicative inverse, especially once we get to projective and conformal geometric algebra.

!!! tip "Exercise"
    A particularly important inverse is that of the pseudoscalar $I$.

    1. Write the pseudoscalar and its inverse for dimensions 1 to 8.
    2. Identify a pattern in the relationship between the pseudoscalar and its inverse depending on the number of dimensions.

## Dual

The **dual** of a multivector $A$ is written $\tilde A$ ("$A$ tilde"). In code, the dual of `a` is written `!a`, `a << pss` where `pss` is the pseudoscalar.

This hints at how to compute the dual of a multivector: just multiply it by the pseudoscalar. We'll usually avoid the notation $\tilde A$ in favor of explicitly multiplying by the pseudoscalar, since sometimes we'll have to multiply by the inverse of the pseudoscalar, which flips the sign in certain dimensions.

!!! tip "Exercise"
    Think about why `a << pss` and `a * pss` are always equivalent, even though in general `a << b` isn't equivalent to `a * b`.

## Rotors

Now that you understand multivectors and the geometric, outer, and inner products, read and/or watch [Marc ten Bosch's explanation of rotors][marctenbosch-rotors]. It also contains excellent interactive demos of bivectors.

[marctenbosch-rotors]: https://marctenbosch.com/quaternions/#h_12

Important things to note:

- In general, rotors use the **even subalgebra** of geometric algebra -- that is they are multivectors where each component has even grade. That means they have scalar, bivector, quadvector, etc. components.
- The rotor $ab$ represents a rotation in the plane spanned by $a$ and $b$ by twice the angle between $a$ and $b$.
- The **reverse** of a rotor represents the reverse transformation.
- The geometric product of two rotors is their composition.
- To apply a rotor $R$ to an object $A$, compute the negative **sandwich product** $R A \tilde R$.

Rotors can be generalized to **rotoreflectors**, which use the **odd subalgebra** of geometric algebra. These work exactly like rotors except that they represent an odd number of reflections.
