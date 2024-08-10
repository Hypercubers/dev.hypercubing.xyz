# Hyperspeedcube Rules

Here is a list of rules that new additions to Hyperspeedcube must follow. Of course, rules are made to be broken, so many of these rules have exceptions.

## Thou shalt not reveal excess information

**Features of the program (piece filters, keybinds, macros, etc.) must not reveal excess information.**

For example, the X-centers on 4^3 are indistinguishable. If you could make a piece filter that selected individual X-centers, then you could use those to solve centers into their original positions and avoid orientation parity.

Exception: Debugging tools may provide this information. These are meant to be used by puzzle designers, not solvers.

## Thou shalt not let input handling know puzzle state

**User input (via mouse or keyboard) must behave the same way regardless of the current state of the puzzle.**

If this were not the case, then the user could make a key that simply solves the current case, and pressing a certain sequence of keys repeatedly could solve the puzzle.

Exception: The state of _which moves are allowed_ is impossible to hide, so this may be revealed.

## Thou shalt not execute more than one twist per input

**Each keyboard or mouse input must execute at most one twist.**

If this were not the case, then the user could make a single key that executes an entire algorithm. This would essentially be a macro, which is undesirable for speedsolving.

Exception: If a key is bound to a macro (and the user is not doing a macroless speedsolve) then of coures that key may perform multiple twists.
