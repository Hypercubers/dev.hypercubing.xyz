# First Puzzle

Now that we know all the puzzle jargon, let's build a **shallow-cut face-turning cube** (that's the standard 3x3x3 Rubik's cube) in Hyperspeedcube. First I'll show you the code and then we'll disect each part.

```lua
puzzles:add('3x3x3', {
  name = "3x3x3",
  ndim = 3,
  symmetry = cd{4, 3},
  build = function(p)
    -- Build the shape
    p.shape:carve('x')
    p.shape.colors:add('x')

    -- Axes
    p.twists.axes:add('x')

    -- Twists
    local R = p.twists.axes[1]
    local U = p.twists.axes[2]
    local F = p.twists.axes[5]
    p.twists:add{axis = U, transform = rot{fix = U, from = R, to = F}}

    -- Slicing and layers
    p:slice(vec('z') / 3)
    U.layers:add(vec('z') / 3)
  end,
})
```

TODO: disect each part. but with how much detail?
