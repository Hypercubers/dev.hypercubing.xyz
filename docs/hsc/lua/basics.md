# Basics

See the [Lua 5.4 reference manual](https://www.lua.org/manual/5.4/manual.html) for general Lua functionality.

Hyperspeedcube user code is run in a [sandbox](http://lua-users.org/wiki/SandBoxes), with some Lua functionality restricted. The following globals are available from the Lua standard library:

- **Constants:** `_VERSION`
- **Functions:** `ipairs`, `next`, `pairs`, `pcall`, `select`, `tonumber`, `tostring`, `unpack`
- **Modules:** `math`, `string`, `table`, `utf8`

The `math` module has been slightly modified; see the [Math](#math) section for more.

The following functions have been slightly modified from their default behavior to capture their output or account for new types, but should otherwise behave approximately the same as they do in the Lua standard library:

- **Modified functions:** `assert`, `error`, `print`, `type`, `warn`

The function [`setmetatable()`](https://www.lua.org/manual/5.4/manual.html#pdf-setmetatable) is accessible, however it has been modified to return a shallow copy of its input table instead of modifying an existing table. `getmetatable()` is not accessible, for safety reasons.

## Operators

The following unary (one-input) prefix operators are unmodified from standard Lua:

- `-` (negation)
- `~` (bitwise NOT)

The following binary (two-input) operators are unmodified from standard Lua:

- `+`, `-`, `*`, and `/` (addition, subtraction, multiplication, and division)
- `//` (floor division)
- `%` (modulo)
- `^` (exponentiation)
- `<<` and `>>` (bitshift operators)
- `&`, `|`, and `~` (bitwise and/or/xor)
- `..` (string concatenation)
- `<` and `>` (less/greater than)
- `<=` and `>=` (less/greater than or equal to)
- `==` (equality)
- `~=` (inequality)
- `and`, `or`, and `not` (logical operators)

See the [Approximate equality](#approximate-equality) section for alternatives to `==` and `~=` that check for approximate equality.

## Global constants

The following global constants have been added:

- `_PUZZLE_ENGINE` is a string containing the program name and version; e.g., `"hyperspeedcube v2.0.0"`
- `AXES` is a table mapping axis names to numbers and vice versa
- `FILENAME` is a string containing name of the file currently executing
- `SPACE` is currently active [space](geometry/space.md)
- `NDIM` is the number of dimensions of the currently active [space](geometry/space.md)
- `lib` provides access to other Lua files; see below
- `color_systems` is the [global color systems library](color-system-library.md)
- `puzzle_generators` is the [global puzzle generators library](puzzle-generator-library.md)
- `puzzles` is the [global puzzle library](puzzle-library.md)

### `lib`

`lib` is a table that provides access to global variables and functions defined in other Lua files. For example, `lib.utils` returns a table of global variables and functions defined in `utils.lua`, and `lib.symmetries.cubic` returns a similar table for `symmetries/cubic.lua`.

??? example "Example using `lib`"

    ```lua title="utils/my_utility_file.lua"
    function hello()
      return "Hello, world!"
    end

    favorite_number = 12

    local super_secret = "can't touch this"
    ```

    ```lua title="demos/some_other_file.lua"
    local my_utils = lib.utils.my_utility_file
    print(my_utils.hello()) -- Hello, world!
    print(my_utils.favorite_number) -- 12
    print(my_utils.super_secret) -- nil
    ```

??? example "Example using `lib` with subdirectories"

    ```lua title="friends/luna.lua"
    name = "Luna Harran"
    ```

    ```lua title="friends/milo.lua"
    name = "Milo Jacquet"
    ```

    ```lua title="friends/rowan.lua"
    name = "Rowan Fortier"
    ```

    ```lua title="demos/friends_demo.lua"
    print(lib.friends.luna.name) -- Luna Harran
    print(lib.friends.milo.name) -- Milo Jacquet
    print(lib.friends.rowan.name) -- Rowan Fortier
    ```

Note that `pairs()` does not work with `lib` and other tables, since the files are loaded dynamically as needed.

Circular dependencies between files are not allowed and will result in errors.

## Global functions

The following global functions have been added:

### `pstring(v)`

Converts a value `v` of any type to a string in a "pretty" way:

- Strings are escaped
- Tables have their contents printed (safe with recursive tables)
- All other types use `tostring()`

### `pprint(...)`

Same as `print()` function built into Lua, but uses `pstring()` instead of `tostring()`.

## Math

The Lua math API is almost unmodified from Lua.

- The functions `math.random` and `math.randomseed` have been removed because puzzle definitions (with the exception of certain novelty puzzles) should be deterministic.
- The functions `math.deg` and `math.rad` have been removed because they have confusing names and semantics. They have been replaced by `math.degree`, a constant equal to the number of radians in one degree.
- Some new functions and constants have been added; they are described below.

Since mathematical functions are used very often in puzzle development, most of the contents of the math module have been placed in the global scope. For example, `sqrt(3)` is equivalent to `math.sqrt(3)`.

The following functions and constants are available from the math module.

### Trigonometry

- `math.sin()`, `math.cos()`, `math.tan()`
- `math.asin()`, `math.acos()`, `math.atan()`
- `math.pi`

The following additional trigonometric constants have been added:

- `math.tau` is equivalent to `math.pi * 2`
- `math.degree` is equivalent to `math.pi / 180`
- `math.phi` is equivalent to `(1 + math.sqrt(5)) / 2`

All trigonometric functions and constants are accessible as globals. For example `pi` and `math.pi` are equivalent.

### Mathematical functions

- `math.ceil()`, `math.floor()`
- `math.max()`, `math.min()`
- `math.abs()`
- `math.exp()`
- `math.fmod()`
- `math.log()`
- `math.modf()`
- `math.sqrt()`

The following additional mathematical functions have been added:

- `math.round(x)` rounds a number to the nearest integer[^rounding]

[^rounding]: If `x` is half-way between two integers, `math.round(x)` rounds away from `0.0`. Internally, it uses [Rust's rounding semantics](https://doc.rust-lang.org/std/primitive.f64.html#method.round).

All these functions are accessible as globals. For example `sqrt(3)` and `math.sqrt(3)` are equivalent.

### Implementation utilities

- `math.maxinteger` and `math.mininteger`
- `math.huge`
- `math.ult`
- `math.tointeger`
- `math.type`

These functions and constants are _not_ accessible as globals.

### Approximate equality

Calculations done with [floating-point numbers][how floating point works] are often inexact, so it can often happen that numbers you expect to be equivalent [might not be][0.3]. The best way to remedy this is to never compare floating-point numbers at all, and for most puzzle definitions, you probably won't need to. But for the rare cases where you do, the following functions are provided that check for _approximate_ equality:

- `math.eq(a, b)` returns whether `abs(a - b) <= EPSILON`
- `math.neq(a, b)` returns whether `abs(a - b) > EPSILON`

`EPSILON` is a built-in constant containing a very small number. At the time of writing it is `0.000001`, but this is subject to change in the future.

!!! experiment

    Currently, `math.eq` and `math.neq` only accept numbers. In the future, they may accept other types such as `blade`s and `transform`s.

[how floating point works]: https://www.youtube.com/watch?v=dQhj5RGtag0
[0.3]: https://0.30000000000000004.com/

`math.eq()` and `math.neq()` are _not_ accessible as globals.

## Strings

The entire [Lua string API](https://www.lua.org/manual/5.4/manual.html#6.4) is unmodified.

The following additional string functions have been added:

- `string.fmt2(s1, s2, ...)` is equivalent a `string.format(s1, ...), string.format(s2, ...)`

## UTF-8

The entire [Lua UTF-8 API](https://www.lua.org/manual/5.4/manual.html#6.5) is unmodified.

## Tables

The entire [Lua table API](https://www.lua.org/manual/5.4/manual.html#6.6) is unmodified.
