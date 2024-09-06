# First Puzzle

Now that we know all the puzzle jargon, let's build a **shallow-cut face-turning cube** (that's the standard 3x3x3 Rubik's cube) in Hyperspeedcube. First I'll show you the code and then we'll dissect each part.

```lua title="3x3x3.lua"
puzzles:add('3x3x3', {
  name = "3x3x3",
  ndim = 3,
  build = function(self)
    local sym = cd'bc3'

    -- Carve the base shape
    for _, v in sym:orbit(sym.oox.unit) do
      self:carve(plane(v))
    end

    -- Add twist axes and slice puzzle
    for _, v in sym:orbit(sym.oox.unit) do
      self.axes:add(v, {1/3, -1/3})
    end

    -- Add twists
    for _, axis in ipairs(self.axes) do
      self.twists:add(axis, rot{fix = axis.vector, angle = tau/4})
    end
  end,
})
```

## Boilerplate

I've tried my best to minimize the amount of boilerplate required to define a puzzle, but there's still some.

```lua title="Boilerplate"
puzzles:add('3x3x3', {
  name = "3x3x3",
  ndim = 3,
  build = function(self)
    ...
  end,
})
```

The first string is the **puzzle ID**. This should have only letters, numbers, and underscores.

The next string is the **puzzle name**. In this case, it's the same as the puzzle ID, but it isn't always. For a puzzle like [Eitan's Star], the name should be `"Eitan's Star"` but the ID would have to be `'eitans_star'` since it can't contain spaces or apostrophes.

[Eitan's Star]: https://twistypuzzles.com/app/museum/museum_showitem.php?pkey=5356

!!! question "Why the inconsistent quotation marks?"

    Lua doesn't actually care whether you use single quotes `'like this'` or double quotes `"like this"`, except that you can only put literal quotes inside a string with the opposite quotation type, `'like "this"'` or `"like 'this'"`. I like to use single quotes for non-user-facing strings, like the puzzle ID (since single quotes are slightly easier to type), and double quotes for user-facing strings (because they might contain apostrophes). Plus it gives a hint to anyone reading the code about the semantics of the string: is it user-facing, or just a string ID for use within code?

Then we have the number of dimensions, which should be pretty self-explanatory.

Lastly, we have the `build` function, which runs when our puzzle is constructed. It takes a single argument, the [`puzzle`](../lua/puzzle-construction/puzzle.md), which we call `p`.

## Symmetry

## Carving the base shape

At the beginning of the `build` function, our puzzle has a single piece that takes up all of 3D space.[^primordial-cube] Think of it like a really big block of marble that we can [`carve()`](../lua/puzzle-construction/puzzle.md#puzzlecarve) our shape from.

[^primordial-cube]: Ok technically it's just a very large **primordial cube** that takes up _a lot_ of 3D space, not all of it. The current iteration of the shape-cutting algorithm only knows how to handle finite shapes.

We could carve out each face individually ...

```lua title="Please don't do this"
self:carve(vec(1, 0, 0))
self:carve(vec(-1, 0, 0))
self:carve(vec(0, 1, 0))
self:carve(vec(0, -1, 0))
self:carve(vec(0, 0, 1))
self:carve(vec(0, 0, -1))
```

But since the puzzle is symmetric, we can use symmtery to simplify this. By starting with one face and iterating over its [**orbit**](https://en.wikipedia.org/wiki/Group_action#Orbits_and_stabilizers), we can generate the other five.

We start by generating the group $BC_3$ (which you may recognize from the [last section](geometry.md#coxeter-group-names) as the symmetry group of a cube) using the global [`cd()` constructor](../lua/geometry/symmetry.md).

```lua title="Carving a cube using a loop"
local sym = cd'bc3'

for _, v in sym:orbit
```

## Adding axes

## Adding twists
