RosettaBoy
==========
Trying to implement a gameboy emulator in a bunch of languages for my own
amusement and education; also giving people an opportunity to compare the
same code written in different languages, similar to
[Rosetta Code](https://www.rosettacode.org) but with a non-trivial codebase :)

The main goals are:

- Readability of the code
- Consistency across langauges
- Idiomatic use of language features
- Basic playability

Notably, 100% accuracy is not a goal - if Tetris works perfectly then I'm
happy, if other games require more obscure hardware features, then I'll
weigh up whether or not the feature is worth the complexity.

Also yes, "consistent across languages" and "idiomatic" can be at odds -
there are subjective compromises to be made, but for the most part that
doesn't seem to be a huge problem. Rust uses `Result`, Python uses
`Exception`, Go uses `error` - but so far it's always been pretty obvious
that eg `NewCart()` in go and `Cart.new()` in rust are doing fundamentally
the same thing in the same way.

So far all the implementations follow a fairly standard layout, with each
module teaching me how to do a new thing. In fact they're all so similar,
I wrote one copy of the documentation for all the implementations:

- [main](docs/main.md): argument parsing
- [cpu](docs/cpu.md): CPU emulation
- [gpu](docs/gpu.md): graphical processing
- [apu](docs/apu.md): audio processing
- [buttons](docs/buttons.md): user input
- [cart](docs/cart.md): binary file I/O and parsing
- [clock](docs/clock.md): timing / sleeping
- [consts](docs/consts.md): lists of constant values
- [ram](docs/ram.md): array access where some array values are special

Pull requests to translate into new languages, or fleshing out existing
languages, are very welcome :)

Completeness
------------
| Feature                       | Python  | C++     | Rust    | Go      | PHP     |
| -------                       | ------- | ---     | ----    | --      | ---     |
| *CPU*                         |         |         |         |         |         |
| gblargh's test suite          | &check; | &check; | &check; | &check; | &check; |
| interrupts                    | &check; | &check; | &check; | &check; | &check; |
| logging                       | &check; | &check; | &check; | &check; | &check; |
| *Graphics*                    |         |         |         |         |         |
| scaled output                 | &check; | &check; | &check; | &check; | &cross; |
| scanline rendering            | &check; | &check; | &check; | &check; | &cross; |
| GPU interrupts                | &check; | &check; | &check; | &check; | &cross; |
| *Audio*                       |         |         |         |         |         |
| audio                         | &cross; | off-key | glitchy | &cross; | &cross; |
| *Inputs*                      |         |         |         |         |         |
| keyboard input                | &check; | &check; | &check; | &check; | &cross; |
| gamepad input                 | &cross; | &cross; | &check; | &cross; | &cross; |
| turbo button                  | &check; | &check; | &check; | &check; | &cross; |
| *Memory*                      |         |         |         |         |         |
| memory mapping                | &check; | &check; | &check; | &check; | &check; |
| bank swapping                 | &cross; | &cross; | &cross; | &cross; | &cross; |

Benchmarks
----------
**Warning**: These implementations aren't fully in-sync, so take numbers with
a large grain of salt. For example: the Python implementation uses native code
to blit whole 8x8 sprites in one go, while the other languages do one pixel at
a time (which is more correct, and necessary for things like parallax effects),
which means that the python version is unfairly fast.

If somebody knows how to measure CPU instructions instead of clock time, that
seems fairer; especially if we can get the measurement included automatically
via github actions. Pull requests welcome :)

Running on an M1 Macbook Pro:

```
$ ./bench.sh
rs:  Emulated 600 frames in  0.34s (1753.36fps)
go:  Emulated 600 frames in  0.96s (624.35fps)
cpp: Emulated 600 frames in  1.05s (568.72fps)
php: Emulated 600 frames in 23.55s (25.47fps)
py:  Emulated 600 frames in 66.32s (9.05fps)
```