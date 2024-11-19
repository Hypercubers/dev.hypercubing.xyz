# Tags

Hyperspeedcube uses a tag-based system to organize puzzles (as opposed to the hierarchical organization used by many other puzzle programs). **Tags** are organized in a hierarchy, where one tag may have several **subtags**. For example, `shape/3d/platonic/cube` is a subtag of `shape/3d/platonic`, which is a subtag of `shape/3d`, which is a subtag of `shape`. For each tagged object (including puzzles and puzzle generators), a tag may have an associated **value** of one of the following types:

- Boolean (`true` or `false`)
- Integer
- String
- String list
- Puzzle name

Most tags have the boolean value, so the tables on this page only indicate a tag's type if it is not boolean. A tag is **present** if it is assigned any value other than `false`, or if any of its subtags are present. The value `false` is valid for all tags, regardless of type.

Some tags (such as `colors/system`) are automatically added when generating the puzzle and so do not need to be specified in the puzzle definition. Most tags are not automatically added, so the tables on this page only indicate if they are.

## List of tags

[`hyperpuzzle/src/tags.kdl`][tags.kdl] is the most up-to-date list of tags. If you're doing puzzle development, it's a good idea to skim that list. Note that leading underscores are removed, so `_120cell` becomes `120cell`.

[tags.kdl]: https://github.com/HactarCE/Hyperspeedcube/blob/main/hyperpuzzle/src/tags.kdl

### Type

The `type` tag indicates whether an object is a shape, puzzle, or generator.

| Tag                     | Description                      |
| ----------------------- | -------------------------------- |
| `type/shape`            | Shape only (cannot be scrambled) |
| `type/puzzle`           | Puzzle (can be scrambled)        |
| `type/generator` (auto) | Puzzle/shape generator           |

`type/shape` should be included on generators that only generate shapes. `type/puzzle` is not necessary on puzzles that only generate puzzles.

### Shapes

The `shape` tags indicate the physical shape of the puzzle. A puzzle will typically only have one `shape` tag (plus its parent tags). If a puzzle has a unique shape that is not listed here, it should have a dimension tag (such as `shape/3d`).

Shape tags are always boolean (`true` or `false`).

Here are a few of the most common shape tags:

| Tag                                    | Description              |
| -------------------------------------- | ------------------------ |
| `shape/3d/platonic/cube`               | Cube                     |
| `shape/3d/platonic/tetrahedron`        | Tetrahedron              |
| `shape/3d/platonic/dodecahedron`       | Dodecahedron             |
| `shape/3d/platonic/octahedron`         | Octahedron               |
| `shape/3d/platonic/icosahedron`        | Icosahedron              |
| `shape/3d/prism`                       | Polygonal prism          |
| `shape/3d/compound`                    | Polyhedral compound      |
| `shape/4d/platonic/hypercube`          | Hypercube (4D)           |
| `shape/4d/platonic/simplex`            | 4-simplex                |
| `shape/4d/platonic/120cell`            | 120-cell                 |
| `shape/4d/platonic/prism/dodecahedron` | Dodecahedral prism       |
| `shape/4d/platonic/duoprism`           | Polygon-polygon duoprism |
| `shape/5d/platonic/simplex`            | 5-simplex                |
| `shape/5d/platonic/hypercube`          | Hypercube (5D)           |

### Colors

The `colors` tags relate to the color system of the puzzle.

| Tag                      | Type   | Description                                      |
| ------------------------ | ------ | ------------------------------------------------ |
| `colors/system` (auto)   | string | Color system name                                |
| `colors/multi_per_facet` |        | Present if there is a facet with multiple colors |
| `colors/multi_facet_per` |        | Preset if multiple facets have the same color    |

### Shapeshifting

Shapeshifting is a unique emergent property from the shape and the axis system.

| Tag             | Description                                        |
| --------------- | -------------------------------------------------- |
| `shapeshifting` | Present if any puzzle states have different shapes |

### Axes

The `axes` tags indicate the axis system(s) of the puzzle. A puzzle may have multiple independent axis systems, although most will only have one. If a puzzle has a unique axis system that is not listed here, it should have a dimension tag (such as `axes/3d`).

Here are a few of the most common axis system tags:

| Tag                                     | Description                   |
| --------------------------------------- | ----------------------------- |
| `axes/3d/elementary/cubic`              | Cubic axes                    |
| `axes/3d/elementary/tetrahedral`        | Tetrahedral axes              |
| `axes/3d/elementary/dodecahedral`       | Dodecahedral axes             |
| `axes/3d/elementary/octahedral`         | Octahedral axes               |
| `axes/3d/elementary/icosahedral`        | Icosahedral axes              |
| `axes/3d/prism`                         | Polygonal prism axes          |
| `axes/3d/compound`                      | Polyhedral compound axes      |
| `axes/4d/elementary/hypercube`          | Hypercubic axes (4D)         |
| `axes/4d/elementary/simplex`            | 4-simplex axes                |
| `axes/4d/elementary/120cell`            | 120-cell axes                 |
| `axes/4d/elementary/prism/dodecahedron` | Dodecahedral prism axes       |
| `axes/4d/elementary/duoprism`           | Polygon-polygon duoprism axes |
| `axes/5d/elementary/simplex`            | 5-simplex axes                |
| `axes/5d/elementary/hypercube`          | Hypercubic axes (5D)          |

### Turning element

The `turns_by` tags indicate which element(s) of the shape correspond to the twisting axes. Since these can be synonyms in low dimensions (for instance, in 3 dimensions, face and facet are the same), most puzzles will have more than one of these tags.

| Tag               | Description                                 |
| ----------------- | ------------------------------------------- |
| `turns_by/facet`  | Facet-turning                               |
| `turns_by/ridge`  | Ridge-turning                               |
| `turns_by/peak`   | Peak-turning                                |
| `turns_by/cell`   | Cell-turning                                |
| `turns_by/face`   | Face-turning                                |
| `turns_by/edge`   | Edge-turning                                |
| `turns_by/vertex` | Vertex-turning                              |
| `turns_by/other`  | Turns by some other element of the polytope |

### Completeness

| Tag                      | Description                                                       |
| ------------------------ | ----------------------------------------------------------------- |
| `completeness/super`     | Each piece has only one solved attitude                           |
| `completeness/real`      | All non-core internal pieces are present and each can be unsolved |
| `completeness/complex`   | The puzzle is [complex]                                           |
| `completeness/laminated` | The puzzle is [laminated]                                         |

[complex]: https://hypercubing.xyz/theory/grip_theory/#complex-puzzles
[laminated]: https://hypercubing.xyz/theory/grip_theory/#lamination

### Algebraic properties

| Tag                                  | Description                                            |
| ------------------------------------ | ------------------------------------------------------ |
| `algebraic/nonpseudo/bandaged`       | Bandaged                                               |
| `algebraic/nonpseudo/doctrinaire`    | Doctrinaire                                            |
| `algebraic/nonpseudo/jumbling`       | Jumbling                                               |
| `algebraic/pseudo/bandaged`          | Pseduobandaged                                         |
| `algebraic/pseudo/doctrinaire`       | Pseudodoctrinaire                                      |
| `algebraic/pseudo/jumbling`          | Pseudojumbling                                         |
| `algebraic/abelian`                  | The puzzle's state space is [abelian] (commutative)    |
| `algebraic/fused`                    | Fused                                                  |
| `algebraic/orientations/non_abelian` | At least one piece has a non-abelian orientation group |
| `algebraic/trivial`                  | Trivial                                                |
| `algebraic/weird_orbits`             | The permutation group of at least one orbit of one piece type is a group other than an [alternating][alternating group] or [symmetric group] |

[abelian]: https://en.wikipedia.org/wiki/Abelian_group
[alternating group]: https://en.wikipedia.org/wiki/Alternating_group
[symmetric group]: https://en.wikipedia.org/wiki/Symmetric_group

!!! info "Definitions"

    - **Unbandaging** is the process of splitting pieces into smaller ones
    - A **finite** puzzle is one with finitely many pieces
    - A **doctrinaire** puzzle is one whose moves are all always accessible (never blocked)
    - A **bandaged** puzzle is one that is not doctrinaire, but can be unbandaged into a finite doctrinaire puzzle
    - A **jumbling** puzzle is one that cannot be unbandaged into a finite doctrinaire puzzle
    - A **pseudodoctrinaire** puzzle is one where the set of accessible moves of a state can be determined only from the orientations of the [1-grip pieces][grip theory]
    - A **pseudojumbling** puzzle is one that cannot be unbandaged into a finite pseudodoctrinaire puzzle
    - A **pseudobandaged** puzzle is one that is not pseudodoctrinaire, but can be unbandaged into a finite pseudodoctrinaire puzzle
    - A **fused** puzzle is one that can be decomposed into several non-interacting puzzles, i.e. the puzzle's state space is a [direct product] and each generator is only from one factor
    - A **trivial** puzzle is one whose state space can be expressed as a [direct product] of "line puzzles," where a "line puzzle" is a puzzle with at most two moves available from any position

    [grip theory]: https://hypercubing.xyz/theory/grip_theory/
    [direct product]: https://en.wikipedia.org/wiki/Direct_product_of_groups

!!! tip "Derived facts"

    - A finite puzzle is always either doctrinaire, bandaged, or jumbling
    - A finite puzzle is always either pseudodoctrinaire, pseudobandaged, or pseudojumbling
    - All doctrinaire puzzles are pseudodoctrinaire
    - All pseudojumbling puzzles are jumbling
    - Trivial puzzles are _not necessarily_ abelian
    - Trivial puzzles are _not necessarily_ doctrinaire

### Cuts

The `cuts` tags indicate properties of the cuts of the puzzle. A puzzle may have any number of these tags.

| Tag                             | Description                                          |
| ------------------------------- | ---------------------------------------------------- |
| `cuts/depth/shallow`            | Shallow-cut                                          |
| `cuts/depth/deep`               | Deep-cut (deeper than shallow-cut, but not half-cut) |
| `cuts/depth/deep/to_adjacent`   | Cut to the adjacent axis                             |
| `cuts/depth/deep/past_adjacent` | Cut past the adjacent axis                           |
| `cuts/depth/deep/past_origin`   | Cut past the origin                                  |
| `cuts/depth/half`               | Half-cut (cut through the center of the puzzle)      |
| `cuts/stored`                   | At least one stored cut                              |
| `cuts/wedge`                    | At least one wedge cut                               |

!!! info "Definitions"

    - A **stored cut** is a division between pieces that cannot be split by a move accessible from the solved state of the puzzle
    - A **wedge cut** is a cut constructed piecewise from multiple (hyper)planes

### Categories

| Tag         | Description                                                                                                                                             |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `canonical` | The puzzle is widely accepted as the "canonical" example for its shape and turning element (e.g., the Rubik's cube is the canonical face-turning cube)  |
| `meme`      | The puzzle is included as a joke and is not intended to be studied or solved                                                                            |

#### Families

See [`tags.kdl`][tags.kdl] for a full list of families.

| Tag                   | Description                                                                                                                                             |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `family/gap/sliding`  | Sliding gap puzzle (e.g., 15-puzzle)                                                                                                                    |
| `family/gap/rotating` | Rotating gap puzzle                                                                                                                                     |
| `family/multicore`    | Puzzle with multiple turning centers                                                                                                                    |

#### Variants

Variant tags are only used for puzzles that were originally invented as variations on existing puzzles.

| Tag                  | Type   | Description                                 |
| -------------------- | ------ | ------------------------------------------- |
| `variant/stickermod` | puzzle | Puzzle that this is a stickermod of         |
| `variant/shapemod`   | puzzle | Puzzle that this is a shapemod of           |
| `variant/bump`       |        | Bump puzzle (e.g., Mirror Blocks)           |
| `variant/bandaging`  |        | The puzzle is a bandaging of another puzzle |

TODO: should `variant/bump` and `variant/bandaging` also have type `Puzzle`?

### Attribution

| Tag        | Type        | Description                                             |
| ---------- | ----------- | ------------------------------------------------------- |
| `author`   | string list | Author(s) of the puzzle definition file                 |
| `inventor` | string list | Inventor(s)/designer(s) of the original physical puzzle |

### Miscellaneous tags

| Tag                     | Type    | Description                                                                      |
| ----------------------- | ------- | -------------------------------------------------------------------------------- |
| `solved` (auto)         |         | Whether the user has solved this puzzle                                          |
| `generated` (auto)      |         | Whether the puzzle was generated from a generator                                |
| `builtin`               | string  | Version of Hyperspeedcube that the puzzle was added, if it is a built-in puzzle  |
| `external/gelatinbrain` | string  | ID in [Gelatinbrain Puzzle Simulator][gelatinbrain]                              |
| `external/hof`          | string  | Hall Of Fame URL                                                                 |
| `external/mc4d`         |         | Whether the puzzle exists in [Magic Cube 4D][mc4d]                               |
| `external/museum`       | integer | ID in the [TwistyPuzzles.com Museum][museum]                                     |
| `external/wca`          | string  | ID in the [World Cube Association rankings][wca]                                 |
| `experimental`          |         | Whether the puzzle is experimental and so is likely to change in future versions |
| `big`                   |         | Whether the puzzle may take a long time to load                                  |

[gelatinbrain]: https://github.com/Hypercubers/gelatinbrain/
[mc4d]: https://hypercubing.xyz/software/magiccube4d/
[museum]: https://twistypuzzles.com/app/museum/museum_search.php
[wca]: https://www.worldcubeassociation.org/results/rankings/

## Specification

A Lua **tag specification** is a table specifying a set of tags for a puzzle or other tagged object.

The sequence values of the table must be strings. If a string does not start with `!`, then that tag is assigned the value `true`. If a string starts with `!`, then it is a **negated tag**: the `!` is removed and the tag specified in the rest of the string is assigned the value `false`.

Non-sequence keys must be strings containing tag names. The value of each entry may be a tag value of the appropriate type, or table containing a specification for subtags.

Tag names may contain multiple `/`-separated components, which is used to specify subtags.

Generated puzzles inherit tags from their generators, unless explicitly overridden with a negated tag.

### Types

Booleans, integers, and strings are all specified using the corresponding Lua types.

A string list may be specified as a single string (such as `"Andrew Farkas"`) table containing a sequence of strings (such as `{"Andrew Farkas", "Milo Jacquet"}`).

A puzzle name is specified using a string containing its ID.

### Expected tags

Many tags are expected to be specified on all puzzles, to ensure that nothing is forgotten and that new tags are added to existing puzzles. Hyperspeedcube will emit a warning when loading a puzzle definition that leaves certain tags unspecified. See [`tags_template.lua`][tags_template.lua].

[tags_template.lua]: https://github.com/HactarCE/Hyperspeedcube/blob/main/tags_template.lua

### Examples

```lua title="Example tag specification"
local tags = {
  builtin = '1.0.0',
  external = {
    gelatinbrain = '3.1.2', -- external/gelatinbrain
    '!hof',                 -- external/hof = false
    museum = 7629,          -- external/museum
    wca = '333',            -- external/wca
  },

  author = { "Andrew Farkas", "Milo Jacquet" }, -- string list (2)
  inventor = "Ernő Rubik",                      -- string list (1)

  'shape/3d/platonic/cube',
  algebraic = {
    'doctrinaire', 'pseudo/doctrinaire',
    '!fused', '!orientations/non_abelian', '!trivial', '!weird_orbits',
  },
  axes = { '3d/elementary/cubic', '!hybrid', '!multicore' },
  colors = { '!multi_facet_per', '!multi_per_facet' },
  cuts = { depth = { 'shallow' }, '!stored', '!wedge' },
  turns_by = { 'face', 'facet' },
  '!experimental',
  '!family',
  '!variant',
  '!meme',
  '!shapeshifting',
}
```
