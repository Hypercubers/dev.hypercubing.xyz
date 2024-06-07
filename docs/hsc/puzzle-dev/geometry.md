# Puzzle Geometry

In order to build a puzzle in Hyperspeedcube, first you must understand the geometry of the puzzle. Fundamentally, we build puzzles by starting with a base shape, slicing it, and then defining how to transform pieces during twists. We have shorthand words for common ways to do this; for example:

- The 3^3^ puzzle is a **shallow-cut face-turning cube**.
- The 2^3^ puzzle is a **half-cut face-turning cube**.
- The skewb is a **half-cut vertex-turning cube**.
- The Halpern-Maier tetrahedron is either a **deep-cut vertex-turning tetrahedron** or a **shallow-cut face-turning tetrahedron**.
- The megaminx is a **shallow-cut face-turning dodecahedron**.
- The square-1 is a **cube shapemod** of **bandaged 3-layer 12-gonal prism**.
- The Pentultimate is a **half-cut face-turning dodecahedron**.
- The face-turning octahedron is — you guessed it! — a **face-turning octahedron**.

What do these terms mean? Let's start with the "X-turning" terms:

- **face-turning** means that the cuts are parallel to the face planes, and the twists rotate pieces in the plane of the face.
- **vertex-turning** means that cuts correspond to vertices of the shape, and the twists rotate pieces around vertices.

You may also see **edge-turning**, **facet-turning**, and **ridge-turning**. (We prefer the terms "facet" and "ridge" in 4D, since "face" is used ambiguously in hypercubing.)

- **shallow-cut** means that the cuts are close to the outside of the polytope, and making them closer to the outside doesn't change the puzzle.
- **half-cut** means that the cuts pass through the center of the puzzle.
- **cut-to-adjacent** means that the cuts pass through the adjacent axes.
- **deeper-cut-than-adjacent** means that the cuts are deeper than the adjacent turning axes.
- **deep-cut** is somewhat vague and has a few different definitions depending on who you ask; in this guide, we'll use it to mean "deeper than shallow-cut, and not better described by one of the other terms."

## Shapes and Schläfli symbols

Shapes can be described with names, like "cube" or "dodecahedron" or "12-gonal prism." The most important property of a shape is its **symmetry group**, which is all the ways it can be rotated and/or reflected and still look the same. The symmetries of most shapes we care about are a special kind of group called a **[Coxeter group]**, which are written using **[Coxeter-Dynkin diagrams]**. Don't worry if you don't know group theory or haven't encountered these before; most of the Coxeter diagrams we want to write are linear (i.e., the diagram has no branching), so we can write them using **[Schläfli symbols]**, which are easier to understand without a strong background in group theory.

!!! abstract "Learn more"
    If you want to learn more about Coxeter groups and computer algorithms for generating their elements, check out the excellent interactive article [Building 4D Polytopes] by Mikael Hvidtfeldt Christensen.

[Coxeter group]: https://en.wikipedia.org/wiki/Coxeter_group
[Coxeter-Dynkin diagrams]: https://en.wikipedia.org/wiki/Coxeter%E2%80%93Dynkin_diagram
[Schläfli symbols]: https://en.wikipedia.org/wiki/Schl%C3%A4fli_symbol

[Building 4D Polytopes]: https://syntopia.github.io/Polytopia/polytopes.html

Schläfli symbols use numbers and curly braces to describe the symmetries of many shapes. For example, here are the Schläfli symbols for the five platonic solids in 3D:

- $\{ 3, 3 \}$ = Tetrahedron
- $\{ 3, 4 \}$ = Octahedron
- $\{ 3, 5 \}$ = Icosahedron
- $\{ 4, 3 \}$ = Cube
- $\{ 5, 3 \}$ = Dodecahedron

!!! tip "Exercise"
    If you have any D&D dice lying around, go get them right now and use them to follow along in this next section! 🐉

The first number in each pair tells you what the base polygon is. For a tetrahedron it's $\{ 3 \}$, a triangle. For a cube it's $\{ 4 \}$, a square. For a dodecahedron it's $\{ 5 \}$, a pentagon. (All of these are regular polygons of course, since we want as much symmetry as possible!)

The second number tells you how many of those polygons are situated around a vertex. For the cube, tetrahedron, and dodecahedron, there are three faces around a vertex. For the octahedron, there's four faces around a vertex. For the icosahedron, there's five. You could also think of this as describing the **[vertex figure]** of the shape.

[vertex figure]: https://en.wikipedia.org/wiki/Vertex_figure

Notice that the octahedron has the same Schläfli symbol as the cube, but reversed. This isn't a coincidence — the cube and dodecahedron have the same symmetry! To see this for yourself, hold an octahedron with one vertex facing toward you and a vertex facing up. Now put a vertex in the center of each triangular face and connect them to form a cube.

- The 8 triangular faces of the octahedron become the 8 vertices of the cube.
- The 12 edges of the octahedron become the 12 edges of the cube.
- The 6 vertices of the octahedron become the 6 square faces of the cube.

We say that the cube and octahedron are **dual** shapes.

!!! tip "Exercise"
    The dodecahedron and icosahedron are also dual. What properties correspond between them?

!!! tip "Exercise"
    What's the dual of the tetrahedron?

### Case study: the rhombic dodecahedron

Let's look at a fun shape: the rhombic dodecahedron.

- It has 12 faces, all rhombi.
- It is **face-transitive**, meaning the polyhedron can be reoriented to put any face in the place of any other.
- It is _not_ **vertex-transitive**. 6 vertices have 3 edges and 8 vertices have 4 edges.

Some of those numbers look familiar ... 6, 8, and 12 showed up in the cube and octahedron. Indeed, the rhombic dodecahedron has **cubic symmetry**, which is equivalent to **octahedral symmetry**. The faces of the rhombic dodecahedron correspond to the edges of the cube. We'll come back to this later when we start generating shapes.
