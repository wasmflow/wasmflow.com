---
title: "Wasmflow"
linkTitle: "wasmflow"
description: >
  The Wasmflow host.
---

`wasmflow` is the runtime executor for Wasmflow manifests and WebAssembly collections.

## USAGE

```sh
wasmflow <SUBCOMMAND>
```

## Subcommands

### serve

Start a persistent host from a manifest or .wasm module, optionally exposing an RPC microservice or connecting to a mesh of hosts across a message queue.

### invoke

Invoke a component exposed in a manifest or a .wasm module.

### list

Print the components exposed by a manifest or a .wasm module.

### test

Load a manifest or .wasm module and run automated unit tests against its components.

### help

Prints this message or the help of the given subcommand(s)
