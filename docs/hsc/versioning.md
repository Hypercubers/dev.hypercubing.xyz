# Puzzle versioning

Puzzle versions use a subset of [Semantic Versionsing 2.0.0](https://semver.org/).

A puzzle version is a string consisting of one, two, or three nonnegative integers, separated by `.`. The first integer is the **major** version, the second integer is the **minor** version, and the third integer is the **patch** version. The minor and patch versions are assumed to be zero when omitted.

!!! example "Examples"
    - `1.2.3` has a major version of `1`, minor version of `2`, and patch version of `3`
    - `0.0.1` has a major version of `0`, minor version of `0`, and patch version of `1`
    - `0.6` has a major version of `0`, minor version of `6`, and patch version of `0`
    - `2` has a major version of `2`, minor version of `0`, and patch version of `0`

- The **major version** should increment whenever changes are made to the puzzle definition that break **log file compatibility**.
- The **minor version** should increment whenever changes are made to the puzzle definition that break **scramble compatibility**.
- The **patch version** should increment whenever **any other changes** are made to the puzzle definition.

Whenever one version increments, the lower ones should be reset to zero.
