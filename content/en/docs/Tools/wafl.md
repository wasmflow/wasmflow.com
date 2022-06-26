---
title: "Wafl"
linkTitle: "wafl"
description: >
  The Wasmflow utility binary.
---

`wafl` (lovingly pronounced "waffle") is the Wasmflow utility binary. It is an RPC client, project bootstrapper, and registry manipulator (among other things).

You don't need `wafl` to run `wasmflow`. It can be distributed or updated on its own and omitted from deployments.

## USAGE

```sh
wafl <COMMAND> <SUBCOMMAND>
```

## Subcommands

### rpc

The `rpc` subcommands connect to a remote `wasmflow` host and issue an RPC request.

- `rpc invoke`: Invoke a component on the remote host.
- `rpc list`: Retrieve the list of exposed components on the remote host.
- `rpc stats`: Get the running statistics of the remote host.

### project

The `project` subcommands houses functions that help you build, deploy, and maintain applications.

- `project new`: Create a new `Wasmflow` project.

### component

The `component` subcommands help you build new components in an existing project.

- `component new`: Create a new schema for a `Wasmflow` component.

### registry

`registry` subcommands are for interacting with an OCI registry.

- `registry push`: Push an image to an OCI registry.
- `registry pull`: Pull an image from an OCI registry.

### wasm

Wafl's `wasm` subcommands help you sign, inspect, and manage WasmFlow WebAssembly artifacts.

- `wasm sign`: Sign a `.wasm` file with local keys.
- `wasm inspect`: Inspect the embedded details of a signed Wasmflow `.wasm` file.

### bundle

The `bundle` subcommands help you manage native, multi-architecture binary collections.

- `bundle pack`: Create a signed archive bundle.

### help

Prints the help message with more details on commands and arguments.
