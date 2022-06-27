---
title: "Prerequisites for the getting started guide"
linkTitle: "Prerequisites"
weight: 1
notoc: true

description: >
  Dependencies you may need to install for this guide
---

### Rust & Cargo

Wasmflow can run WebAssembly built from any language, but the current code generation tools prioritize Rust above others and this guide expects a Rust development environment with Cargo.

Install rust & cargo via [rustup](https://rustup.rs/) for Windows, Mac, or Linux. Windows users may need to install the [C++ build tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/) with the "Desktop development with C++" module.

### WebAssembly target for Rust

To build WebAssembly from Rust, you will need to install a suitable target for the compiler.

You'll start off using baseline WebAssembly but may eventually start using WASI extensions. You can install them both via the rustup command

```sh
rustup target add wasm32-unknown-unknown wasm32-wasi
```

### Node.js & npm

Wasmflow's code generators are written in JavaScript to make it as easy as possible for people to contribute generators for new languages.

If you don't have node.js & npm (you probably do), install it via [nvm](https://github.com/nvm-sh/nvm) on Mac or Linux or [nvm-windows](https://github.com/coreybutler/nvm-windows/releases) on Windows.

### `tomlq`

[tomlq](https://github.com/jamesmunns/tomlq) is a command line parser for TOML files and Wasmflow uses it to grab information from toml files like `Cargo.toml`.

Install `tomlq` with

```
cargo install tomlq
```

### `wafl-codegen`

[wafl-codegen](https://github.com/wasmflow/wasmflow) is Wasmflow's code generator.

Install `wafl-codegen` via `npm install -g @candlecorp/codegen`

## `make` on Windows

Wasmflow and generated projects use Makefiles to automate builds and code generation. The easiest way to install `make` on Windows is via [Chocolatey](https://chocolatey.org/install).

```sh
$ choco install make
```

## Optional: `docker` or an existing OCI registry for publishing artifacts

Install docker from [docker.com](https://docs.docker.com/get-docker/).
