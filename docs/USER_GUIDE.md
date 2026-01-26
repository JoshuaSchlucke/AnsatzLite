# User Guide

This guide covers how to run AnsatzLite as a CLI tool and how to configure problems, backends, and optimizers.

## Installation

Using Nix (recommended):

```bash
nix build
./result/bin/ansatz --help
```

Without Nix (dev):

```bash
effekt src/main.effekt -- --help
```

## Problems

Choose a problem with `--problem`:

- `maxcut`
- `mis` (Maximum Independent Set)
- `vertex-cover`
- `sat` (Max-SAT / k-SAT)

### Graph-based problems

For `maxcut`, `mis`, and `vertex-cover`, select a graph with `--graph`:

- `tiny` (2 nodes)
- `small` (3 nodes)
- `medium` (4 nodes)
- `large` (6 nodes)

Example:

```bash
ansatz --problem maxcut --graph medium --p 1 --gammas 0.2 --betas 0.4
```

### SAT problems

For SAT, select a formula with `--formula`:

- `tiny` (small built-in example)
- `k<k>-<m>-<n>` (k-SAT with m clauses and n variables)

Example:

```bash
ansatz --problem sat --formula k3-10-6 --p 1 --gammas 0.2 --betas 0.4
```

## Backends

Select a backend with `--backend`:

- `statevector` (exact simulation)
- `shots` (shot-based expectation)
- `noisy` (statevector + noise)
- `sampling` (returns bitstrings)
- `pretty` (prints program structure)
- `pretty-sample` / `pretty+sample` (structure + samples)

Shot-based backends require `--num-shots`:

```bash
ansatz --backend shots --num-shots 2000 --seed 42 --p 1 --problem maxcut --graph tiny
```

Noise uses `--noise-model` (default: `bitflip:0.05`):

```bash
ansatz --backend noisy --noise-model bitflip:0.25 --p 1 --problem maxcut --graph tiny
```

## Parameters

The number of layers is `p`. You must provide exactly `p` gammas and `p` betas, or omit them to default to zeros:

```bash
ansatz --p 3 --gammas 0.1,0.2,0.3 --betas 0.4,0.5,0.6 --problem maxcut --graph tiny
```

## Optimization

Enable optimizers with `--opt`:

- `none` (default)
- `grid`
- `random`
- `local` (coordinate descent)

Common flags:

- `--opt-steps` (grid steps per dimension or number of random samples)
- `--gamma-min`, `--gamma-max`, `--beta-min`, `--beta-max`
- `--opt-verbose` (print optimizer output)
- `--print-config` (print resolved config)

### Grid search

```bash
ansatz --opt grid --opt-steps 5 --gamma-min 0.0 --gamma-max 3.14 --beta-min 0.0 --beta-max 1.57 \
  --opt-verbose --p 1 --problem maxcut --graph tiny
```

### Random search

```bash
ansatz --opt random --opt-steps 25 --seed 123 --p 1 --problem maxcut --graph tiny
```

### Local search (coordinate descent)

Local search starts from a grid or random init:

```bash
ansatz --opt local --opt-init grid --opt-steps 5 --local-steps 10 --local-step 0.05 --local-decay 0.5 \
  --p 1 --problem maxcut --graph tiny
```

## Output control

Print the resolved config before running:

```bash
ansatz --print-config --p 1 --problem maxcut --graph tiny
```

Pretty-print (structure only):

```bash
ansatz --backend pretty --p 1 --problem sat --formula tiny
```

Pretty + sample:

```bash
ansatz --backend pretty-sample --num-shots 10 --seed 7 --p 1 --problem maxcut --graph tiny
```

## Performance notes

Statevector simulation scales exponentially with qubits. For larger graphs/formulas, prefer `--backend shots`.
