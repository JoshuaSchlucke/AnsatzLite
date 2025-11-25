# mini Quantum Alternating Operator Ansatz framework

A minimal and modular implementation of the Quantum Alternating Operator Ansatz (QAOA) in Effekt.
The project defines a tiny quantum DSL using effects and provides multiple handlers that interpret the same QAOA program in different ways.

## Must-have

A very small QAOA DSL using effects:
- prepare a quantum state,
- apply a cost operator (with γ),
- apply a mixer operator (with β),
- measure the final state.

A state-vector simulator handler:
- interprets the DSL operations on a classical state vector,
- returns a numeric measurement.

A CLI program that:
- accepts p (number of layers) and γ/β parameter lists,
- executes the alternating operator ansatz,
- prints out the final measurement.
- is crash resistant against invalid/malformed input

At least one example for the Presentation (See small lib in Can-have. Something like MaxCut or similar)

Testing/Linting where possible (esp. important parts)

## Can-have

Input validation for invalid parameters or malformed input.

A pretty-print handler that prints the structure of the ansatz instead of executing it.

A sampling handler that outputs bitstrings instead of expectation values.

A simple noise handler (e.g. depolarizing noise after each operator).

A small library of example operators or tiny example problems:
- number partitioning,
- small scheduling constraint,
- or a simple custom objective.

A grid search or simple local search for γ, β parameters.

A small interactive menu (choose p, enter parameters, run).

## Will-not-have

Gradient or parameter-shift handler

Constraint-preserving mixers for nontrivial problems (graph coloring, TSP, etc.).

Performance optimizations

GUI, visualization tools, or circuit diagram rendering.

Sophisticated optimizers (Adam, BFGS, etc.).

## Effects and Handlers

QAOA/Quantum effects:
- for prepare, cost, mixer, measure

Handlers:
- state-vector handler (main execution)
- pretty-print handler (optional)
- optional sampling, noise, or debug handlers.

Random effect for sampling (optional)

## FFI and Libraries

<!-- rule "Keep FFI surface minimal" applies:
     implement as much as possible in Effekt directly and keep FFI bindings small. -->

Existing in stdlib:
- Basic numeric types
- Lists
- I/O
- Randomness

Need to implement:
- Complex number type + operations


