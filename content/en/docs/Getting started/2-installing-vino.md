---
title: "Installing Wasmflow"
linkTitle: "Installing Wasmflow"
weight: 2
notoc: true
description: >
  How to install `wasmflow`, & `wafl`
---

## Pre-build binary installation

Head over to [wasmflow/releases](https://github.com/wasmflow/wasmflow/releases) to get the latest version of the Wasmflow tools for your platform

Unless you are developing on Wasmflow's internal code, you should stick with the pre-built binaries.

## Building from source

{{< card title="Tip" >}}
_Note: The source code for Wasmflow tools is available to limited users at the moment. Wasmflow and its tools will be open sourced once the Wasmflow runtime is mature enough to be released publicly._
{{< /card >}}

`make install` will build everything in the Wasmflow monorepo and install any binaries into your `~/.cargo/bin` directory.

```sh
$ git clone https://github.com/wasmflow/wasmflow
$ cd wasmflow && make install
```
