---
title: "Prerequisites for Rust components"
linkTitle: "Prerequisites"
weight: 2
notoc: true

description: >
  Dependencies you may need to install for this guide
---

### Rust & Cargo

To build Rust components you will need the Rust compiler and companion tools installed.

Install rust & cargo via [rustup](https://rustup.rs/) for Windows, Mac, or Linux.

Windows users may need to install the [C++ build tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/) with the "Desktop development with C++" module.

### WebAssembly target for Rust

To compile Rust into WebAssembly, you need to install a wasm-\* target.

Install both the cross-platform wasm32 target and the WASI (WebAssembly Systems Interface) implementation with this rustup command:

```sh
rustup target add wasm32-unknown-unknown wasm32-wasi
```

### Node.js & npm

Wasmflow's code generators are written in JavaScript to make it as easy as possible for people to contribute generators for new languages.

If you don't have node.js & npm, install it via [nvm](https://github.com/nvm-sh/nvm) on Mac or Linux or [nvm-windows](https://github.com/coreybutler/nvm-windows/releases) on Windows.

### `tomlq`

[tomlq](https://github.com/jamesmunns/tomlq) is a command line parser for TOML files and Wasmflow uses it to grab information from toml files like `Cargo.toml`.

Install `tomlq` with the command

```sh
cargo install tomlq
```

### `wasmflow-codegen`

[wasmflow-codegen](https://github.com/wasmflow/wasmflow) is Wasmflow's code generator.

Install `wasmflow-codegen` via `npm install -g @candlecorp/codegen`

### `make` on Windows

Wasmflow and generated projects use Makefiles to automate builds and code generation. The easiest way to install `make` on Windows is via [Chocolatey](https://chocolatey.org/install).

```sh
$ choco install make
```
