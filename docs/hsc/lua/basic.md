# Basic functionality

!!! warning "This API is not stable and may change in future versions"

See the [Lua 5.4 reference manual](https://www.lua.org/manual/5.4/manual.html) for general Lua functionality.

Hyperspeedcube user code is run in a sandbox, with some Lua functionality restricted. These globals are available from the Lua standard library:

- **Constants:** `_VERSION`
- **Functions:** `ipairs`, `next`, `pairs`, `pcall`, `select`, `tonumber`, `tostring`, `unpack`
- **Modules:** `math`, `string`, `table`, `utf8`

These functions have been slightly modified from their default behavior to capture their output or account for new types, but should otherwise behave approximately the same:

- **Modified functions:** `assert`, `error`, `print`, `type`, `warn`

## Global constants

The following global constants have been added:

- `_PUZZLE_ENGINE` - string containing the program name and version; e.g., `"hyperspeedcube v2.0.0"`
- `FILENAME` - name of the file currently executing
- `AXES` - table mapping axis names to numbers and vice versa

## Global functions

The following global functions have been added:

### `pstring(v)`

Converts a value `v` of any type to a string in a "pretty" way:

- Strings are escaped
- Tables have their contents printed (safe with recursive tables)
- All other types use `tostring()`

### `pprint(...)`

Same as `print()`, but uses `pstring()` instead of `tostring()`.

## Math

The math API is almost unmodified from Lua. The functions `math.random` and `math.randomseed` have been removed and the following have been added:

- `math.tau` - equivalent to `math.pi * 2`
- `math.phi` - equivalent to `(1 + math.sqrt(5)) / 2`

Other functions remain unmodified:

### Trigonometry

- `math.acos`
- `math.asin`
- `math.atan`
- `math.cos`
- `math.deg`
- `math.pi`
- `math.rad`
- `math.sin`
- `math.tan`

### Mathematical functions

- `math.abs`
- `math.ceil`
- `math.exp`
- `math.floor`
- `math.fmod`
- `math.log`
- `math.max`
- `math.min`
- `math.modf`
- `math.sqrt`

### Implementation utilities

- `math.huge`
- `math.maxinteger`
- `math.mininteger`
- `math.ult`
- `math.tointeger`
- `math.type`

## Strings

All functions from the Lua string API remain unmodified:

- `string.byte`
- `string.char`
- `string.dump`
- `string.find`
- `string.format`
- `string.gmatch`
- `string.gsub`
- `string.len`
- `string.lower`
- `string.match`
- `string.pack`
- `string.packsize`
- `string.rep`
- `string.reverse`
- `string.sub`
- `string.unpack`
- `string.upper`

The following have functions been added:

### `string.join(connector, t)`

Given a string `connector` and a list `t`, returns a string containing each element from `t` converted to a string using `tostring()` concatenated together, separated by `connector`.

## Tables

The entire `table` API is unmodified from Lua.

## UTF-8

The entire `utf8` API is unmodified from Lua.
