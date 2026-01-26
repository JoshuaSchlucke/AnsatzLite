# AnsatzLite

AnsatzLite is a small internal DSL and runtime for the Quantum alternating operator ansatz, written in Effekt.
It showcases algebraic effects by separating pure problem definitions from effectful backends.

## Quick start

Using Nix (recommended):

```bash
nix build
./result/bin/ansatz --help
```

Without Nix (dev):

```bash
effekt src/main.effekt -- --help
```

For frequent use, add the binary to your PATH
## Usage

Show all flags:

```bash
ansatz --help
```

Run a small MaxCut instance:

```bash
ansatz --problem maxcut --graph tiny --p 1 --gammas 0.2 --betas 0.4
```

Run SAT with a generated k-SAT formula:

```bash
ansatz --problem sat --formula k3-10-6 --p 1 --gammas 0.2 --betas 0.4
```

Enable optimization:

```bash
ansatz --opt grid --opt-steps 5 --opt-verbose --p 1 --problem maxcut --graph tiny
```

## Testing

Run all tests: (fzf)

```bash
effekt tests/**
```

## Documentation

- User guide: docs/USER_GUIDE.md
- Developer guide: docs/DEVELOPER_GUIDE.md
