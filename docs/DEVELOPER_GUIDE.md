# Developer Guide

This document describes the code structure, extension points, and testing workflow.

## Project layout

- `src/qaoa/` — core types and the internal DSL
- `src/problems/` — problem encodings (MaxCut, MIS, Vertex Cover, SAT)
- `src/handlers/` — effect handlers / backends (statevector, shots, noise, sampling, pretty)
- `src/optimize/` — optimizers (grid, random, local search)
- `src/cli/` — CLI parsing and validation
- `tests/` — unit tests, grouped by domain
- `docs/` — deeper documentation

## Effects and handlers

The DSL emits effects declared in `src/qaoa/effects.effekt`:

- `prepare`, `cost`, `costEdge`, `costPredicate`, `mixer`, `expect`

Each backend installs handlers for these effects and provides a different semantics.
This keeps problems and DSLs pure, and makes backends swap-in compatible.

## Adding a new problem

1. Create a module under `src/problems/` with a pure cost function and an `executeX` function.
2. Use only effects from `src/qaoa/effects.effekt`.
3. Add a CLI option in `src/cli/validate.effekt` and route it in `src/main.effekt`.
4. Add tests under `tests/problems_*.effekt`.

## Adding a new backend

1. Add a module under `src/handlers/`.
2. Implement handlers for `prepare`, `cost`, `costEdge`, `costPredicate`, `mixer`, `expect`.
3. Register it in `src/main.effekt` and `src/cli/validate.effekt`.
4. Add tests under `tests/backends_*.effekt`.

## Adding a new optimizer

1. Add a function to `src/optimize/basic.effekt` or a new module.
2. Plug it into `optimizeLayers` in `src/main.effekt`.
3. Add CLI flags in `src/cli/parse.effekt` + validation in `src/cli/validate.effekt`.
4. Add tests under `tests/optimize_*.effekt`.

## Testing

Run all tests:

```bash
effekt tests/*
```

## Building
Build is done via nix and defined in `flake.nix` and done with:
```bash
nix build
```

## CI

GitHub Actions includes:

- `flake-check.yml` for Nix checks
- `effekt-tests.yml` to run the test suite

## Conventions

- Keep problem definitions pure.
- Add new effects only in `src/qaoa/effects.effekt`.
- Keep handlers backend-agnostic (no problem-specific imports).
- Use deterministic seeds in tests.
