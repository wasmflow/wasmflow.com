---
title: "Installing Wasmflow"
linkTitle: "Installing Wasmflow"
weight: 1
notoc: true
description: >
  How to install `wasmflow` & `wafl`
---

## Pre-built binary installation

Head over to [wasmflow/releases](https://github.com/wasmflow/wasmflow/releases) to get the latest version of the Wasmflow tools for your platform

## Building from source

`make install` will build everything in the Wasmflow monorepo and install any binaries into your `~/.cargo/bin` directory.

```sh
$ git clone https://github.com/wasmflow/wasmflow
$ cd wasmflow && make install
```

Now that you've got `wasmflow` and `wafl` installed, learn how to [Create WebAssembly Components]({{< ref "/docs/getting-started/wasm-components" >}}) or jump straight to [Invoking Wasmflow Components]({{< ref "/docs/getting-started/invoking-components" >}}).
