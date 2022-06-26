---
title: "Terminology"
linkTitle: "Terminology"
description: >
  Providers, modules, components, schematics, oh my!
---

### Components

Wasmflow _components_ are the smallest unit of logic in Wasmflow and can be thought of like a common `function` in any other platform. Components have input and output ports that can connect to any other component.

### Ports

Wasmflow _ports_ are asynchronous streams of (wasmflow) packets. A packet is a versioned unit of data that can hold an internal signal, a successful value (in an intermediary formats) or a failure value.

In everyday terms, Wasmflow's ports are the inputs and outputs to components. They can represent any kind of data, from simple to complex, asynchronous to synchronous, successful computation to internal error or edge cases. Wasmflow's ports are what make it possible to connect arbitrary components.

### Collections

Wasmflow _collections_ are collections of components. If components are analogous to functions, collections are like libraries that house functions in a package or namespace.

### Flows

Wasmflow _flows_ are configuration-based components. You take other components, connect them together, and you end up with a new component that can connect the same way.

### Runtime

The Wasmflow _runtime_ is the library implementation that manages resolving, loading, validating, and instantiating everything that Wasmflow needs to run.

### Hosts

A Wasmflow _host_ is a running implementation of the runtime that speaks Wasmflow RPC protocols. A host embeds the runtime and is what you execute and interact with.

### WaPC

The WebAssembly Procedure Call project is an open source standard for communicating in and out of WebAssembly modules. Wasmflow uses WaPC concepts and is committed to progressing open standards in WebAssembly.

### Apex

The Apex IDL is a minimal language for defining the interface of your components (among other things). It is loosely based on GraphQL and is generic enough to use for non-WebAssembly purposes.

### Manifests & `.wafl` files

Wasmflow manifests are configuration files that define flow-based components and host configurations.

### WebAssembly Module

A "WebAssembly Module" is a generic term for any compiled WebAssembly but when used in Wasmflow's context it refers to a WebAssembly implementation of a Wasmflow collection of components.

### `wasmflow`

The `wasmflow` binary is an executable Wasmflow host.

### `wafl`

`wafl` (pronounced _waffle_) is the Wasmflow utility binary. It contains useful subcommands for managing or interacting with Wasmflow hosts or components.
