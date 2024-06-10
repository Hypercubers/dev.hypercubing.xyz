# Common patterns

Several types have methods that behave similarly and are implemented using the same generic code. To keep the documentation consistent and concise, they are all documented here.

On this page:

- An **element** is a [color], [axis], or [twist], or the index or name of one
- A **value** is some other value (often a string or number, but not always)

[color]: puzzle-construction/colors.md#color
[axis]: puzzle-construction/axes.md#axis
[twist]: puzzle-construction/twists.md#twist

## Mappings

A **mapping** associates a value to each element. A mapping may be either of the following:

- a function from elements to values
- a table whose keys are elements and whose values are the resulting values

See [`:rename()`](#rename) for an example of each of these.

## Common methods

### `:rename()`

`:rename()` renames multiple elements at once. It takes a single argument: a [mapping](#mappings) from elements to strings (the new names).

```lua title="Examples of puzzle.colors:rename()"
-- Assign existing colors new names, in their current ordering
puzzle.colors:rename{'Right', 'Left', 'Up', 'Down'}

-- Rename colors based on their old names
puzzle.colors:rename(function(color)
  return "Old " .. color.name
end)
```

### `:swap()`

`:swap()` swaps two elements in the ordering. It takes two arguments: the elements to swap.

### `:reorder()`

`:reorder()` changes the ordering of all elements. It takes a single argument: a sequential table of elements in the new order, or a function mapping elements to new indices.

The keys of the table or the outputs of the function do not actually need to be sequential integers; they may be any numbers, and will be sorted by their values.

The sort is [stable](https://en.wikipedia.org/wiki/Sorting_algorithm#Stability), and any unspecified elements are left at the end of the list in their original order.
