---
title: "Vow"
linkTitle: "vow"
description: >
  The Wasmflow WebAssembly wrapper.
---

`vow` is the executable that directly executes Wasmflow WebAssembly providers or serves them as microservices.

```
vow 0.2.8
Wasmflow WebAssembly Wrapper

USAGE:
    vow <SUBCOMMAND>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    help     Prints this message or the help of the given subcommand(s)
    run      Query a provider for a list of its hosted components
    serve    Sign a WaPC component
```

`vow` is the executable that directly executes Wasmflow WebAssembly providers or serves them as microservices.

## USAGE

```sh
vow <SUBCOMMAND>
```

## Subcommands

### run

Execute a component in the target WASM module.

### serve

Start a persistent RPC, HTTP, or Lattice host for the target WASM module.

### test

Run a test file against the passed WASM module.

### help

Prints this message or the help of the given subcommand(s)
