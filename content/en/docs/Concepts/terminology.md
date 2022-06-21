---
title: "Terminology"
linkTitle: "Terminology"
description: >
  Providers, modules, components, schematics, oh my!
---

### Components

Wasmflow _components_ are the smallest unit of logic in Wasmflow and can be thought of like a common `function` in any other platform. Components have input and output ports that can connect to any other component.

### Ports

Wasmflow _ports_ are asynchronous streams of (vino) packets. A packet is a versioned unit of data that can hold an internal signal, a successful value (in multiple intermediary formats) or a failure value.

In everyday terms, Wasmflow's ports are the inputs and outputs to components. They can represent any kind of data, from simple to complex, asynchronous to synchronous, successful computation to internal error or edge cases. Wasmflow's ports are what make it possible to connect arbitrary components.

### Providers

Wasmflow _providers_ are collections of components. If components are functions, providers are libaries.

### Schematics

Wasmflow _schematics_ are the configuration that defines how component connect, what namespace a provider is registered under, etc. Schematics have inputs and outputs as well and thus can also be treated like components.

### Networks

Wasmflow _networks_ are collections of schematics. Since schematics are components, networks are also providers and can be exposed as such to be referenced by other schematics.

### Runtime

The Wasmflow _runtime_ is the library implementation that manages a network, its schematics, and exposes internal components.

### Hosts

A Wasmflow _host_ is an implementation of a runtime that exposes a network to connections and requests.

### WaPC

The WebAssembly Procedure Call project is an open source standard for communicating in and out of WebAssembly modules.

### Apex

The Apex IDL is a minimal language for defining the interface of your components (among other things). It is loosely based on GraphQL and is generic enough to use for non-WebAssembly purposes.

### Manifest

Manifests are configuration definitions for different parts of Wasmflow.

#### Host Manifest

The configuration of a Wasmflow host

#### Network Manifest

The configuration of a Wasmflow network, its schematics, providers, et al.

#### Schematic Manifest

A configuration for a single schematic.

### WebAssembly Module

A "WebAssembly Module" is a generic term for any compiled WebAssembly but when used in Wasmflow's context it refers to a WebAssembly implementation of a Wasmflow provider, a collection of components.

### `wasmflow`

The `wasmflow` binary is the executable implementation of a Wasmflow host. It can start a network and schematics from a host manifest.

### `wafl`

`wafl` (pronounced _waffle_) is the Wasmflow utility binary. It contains useful subcommands for managing or interacting with Wasmflow hosts or components.
