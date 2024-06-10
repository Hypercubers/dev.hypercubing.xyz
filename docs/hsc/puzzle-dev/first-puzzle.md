# First Puzzle

Now that we know all the puzzle jargon, let's build a **shallow-cut face-turning cube** (that's the standard 3x3x3 Rubik's cube) in Hyperspeedcube. First I'll show you the code and then we'll disect each part.

```lua
puzzles:add('3x3x3', {
  name = "3x3x3",
  ndim = 3,
  build = function(p)
    local sym = cd'bc3'

    -- Carve the base shape
    for _, v in sym:orbit(sym.oox.unit) do
      p:carve(plane(v))
    end

    -- Add twist axes and slice puzzle
    for _, v in sym:orbit(sym.oox.unit) do
      p.axes:add(v, {1/3, -1/3})
    end

    -- Add twists
    for _, axis in ipairs(p.axes) do
      p.twists:add(axis, rot{fix = axis.vector, angle = tau/4})
    end
  end,
})
```

TODO: disect each part. but with how much detail?
