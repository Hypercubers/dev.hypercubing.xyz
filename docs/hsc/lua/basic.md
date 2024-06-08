# Basics

!!! warning "This API is not stable and may change in future versions"

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

The following operators are unmodified from standard Lua:

- `+` - addition
- `-` - negation/subtraction
- `*` - multiplication
- `/` - division
- `^` - exponentiation
- `<`/`>` - less/greater than
- `<=`/`>=` - less/greater than or equal to
- `==` - equal to (exact)
- `~=` - not equal to (exact)

See the [Approximate equality](#approximate-equality) section for alternatives to `==` and `~=` that check for approximate equality.

## Global constants

The following global constants have been added:

- `_PUZZLE_ENGINE` is a string containing the program name and version; e.g., `"hyperspeedcube v2.0.0"`
- `FILENAME` is a string containing name of the file currently executing
- `AXES` is a table mapping axis names to numbers and vice versa

## Global functions

The following global functions have been added:

### `pstring(v)`

Converts a value `v` of any type to a string in a "pretty" way:

- Strings are escaped
- Tables have their contents printed (safe with recursive tables)
- All other types use `tostring()`

### `pprint(...)`

Same as `print()` function built into Lua, but uses `pstring()` instead of `tostring()`.

### `require()`

Loads another Lua file and returns a table containing all its global variables and functions. This cannot be called while a puzzle is being constructed. A filename is specified relative to the Lua directory, with an optional `.lua` suffix. If the file has already been loaded, it is not loaded again; the existing table is returned.

??? example "Example using `require()`"

    ```lua title="utils/my_utility_file.lua"
    function hello()
      return "Hello, world!"
    end

    favorite_number = 12

    local super_secret = "can't touch this"
    ```

    ```lua title="demos/some_other_file.lua"
    local my_utils = require('utils/my_utility_file')
    print(my_utils.hello()) -- Hello, world!
    print(my_utils.favorite_number) -- 12
    print(my_utils.super_secret) -- nil
    ```

The trailing patterns `*` (or equivalently, `*.lua`) are supported; in this case, a table is returned where each key is a filename (without the trailing Lua suffix) and each value is the table resulting from loading that file.

??? example "Example using `require()` with `*`"

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
    local all_friends = require('friends/*')
    print(all_friends.luna.name) -- Luna Harran
    print(all_friends.milo.name) -- Milo Jacquet
    print(all_friends.rowan.name) -- Rowan Fortier
    ```

## Math

The Lua math API is almost unmodified from Lua.

- The functions `math.random` and `math.randomseed` have been removed because puzzle definitions (with the exception of certain novelty puzzles) should be deterministic.
- The functions `math.deg` and `math.rad` have been removed because they have confusing names and semantics. They have been replaced by `math.degree`, a constant equal to the number of radians in one degree.
- Some new functions and constants have been added; they are described below.

Since mathematical functions are used very often in puzzle development, most of the contents of the math module have been placed in the global scope. For example, `sqrt(3)` is equivalent to `math.sqrt(3)`.

The following functions and constants are available from the math module:

### Trigonometry

- `math.sin`, `math.cos`, `math.tan`
- `math.asin`, `math.acos`, `math.atan`
- `math.pi`

The following additional trigonometric constants have been added:

- `math.tau` is equivalent to `math.pi * 2`
- `math.degree` is equivalent to `math.pi / 180`
- `math.phi` is equivalent to `(1 + math.sqrt(5)) / 2`

All trigonometric functions and constants are accessible as globals. For example `pi` and `math.pi` are equivalent.

### Mathematical functions

- `math.ceil`, `math.floor`
- `math.abs`
- `math.exp`
- `math.fmod`
- `math.log`
- `math.max`
- `math.min`
- `math.modf`
- `math.sqrt`

The following additional mathematical functions have been added:

- `math.round(x)` rounds a number to the nearest integer[^rounding]

[^rounding]: If `x` is half-way between two integers, `round(x)` rounds away from `0.0`. Internally, it uses [Rust's rounding semantics](https://doc.rust-lang.org/std/primitive.f64.html#method.round).

All these functions are accessible as globals. For example `sqrt(3)` and `math.sqrt(3)` are equivalent.

### Implementation utilities

- `math.huge`
- `math.maxinteger`
- `math.mininteger`
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

## UTF-8

The entire [Lua UTF-8 API](https://www.lua.org/manual/5.4/manual.html#6.5) is unmodified.

## Tables

The entire [Lua table API](https://www.lua.org/manual/5.4/manual.html#6.6) is unmodified.
