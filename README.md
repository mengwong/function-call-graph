# function-call-graph
This repository generates function call dependency graphs of Haskell files. It is useful for viewing the structure of large libraries, and visualizing the dependencies between subsets of project files.

## Build steps
```sh
stack build
stack install
```

## Usage

The compiled binary is called `fcall`.

`fcall` accepts an arbitrary number of filenames as input arguments, including wildcards. Each file is parsed using Haskell's syntax for function declaration and application. In particular, top-level function definitions occur immediately after a newline character.

### Example usage
```sh
fcall */*.hs | dot -Tsvg -o ~/fcall.svg
```

![function-call-graph function call dependencies](fcall.svg)

### Options

`--clusters` to group functions by file. This will give you a sense of your [coupling](https://en.wikipedia.org/wiki/Coupling_(computer_programming)) and [cohesion](https://en.wikipedia.org/wiki/Cohesion_(computer_science)).

![fcall --clusters](fcall-clusters.svg)

## Current limitations
* Infix operators are not supported.
* Unicode is not supported.
* Since this is a very naive parser, some preprocessor directives and text inside quasiquoters get parsed as functions.
* There's no option to read files or apply a wildcard recursively. Use your shell to do that.
* Multiple functions with the same name, originating in different files, get clobbered.
* The color scheme only goes up to 13. If you have more than 13 input files, you may get unexpected results.
