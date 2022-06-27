---
title: "Connecting components"
linkTitle: "Connecting components"
weight: 8
notoc: true
description: >
  Start connecting simple components on the command line.
---

The command line is a pipeline of streaming data where every component feeds directly into one other. To Wasmflow it's like a flow that never branches. It's a great way to test and experiment with small bits of logic.

The following command pipes one execution of vow into another, both running the same `wasm` but pointing to different components.

```sh
$ wasmflow invoke ./build/my_component_s.wasm concatenate -- --left=Jane --right=Doe |\
 wasmflow invoke ./build/my_component_s.wasm greet
{"output":{"value":"Hello Jane Doe"}}
```

`wasmflow` output is directly pipable to another execution of `wasmflow`! You can string together any number of connections to experiment, test, or build on the command line.

{{< card title="Tip" >}}
_Note: `wasmflow` connects output from a named port to an incoming port of the same name with one exception shown above. Data coming out of a port named `output` will be mapped to a port named `input` on the downstream component. This is a common practice and makes CLI testing more intuitive. Turn it off by passing the `--raw` flag to `wasmflow`._
{{< /card >}}

### Connecting to remote components

Every tool in the Wasmflow suite talks the same language so switching from one to another should be seamless. This means that you can pipe from `wasmflow` running WebAssembly locally to `wafl` which connects to any remote provider and then back to `wasmflow` again.

Start serving a WebAssembly module with `wasmflow` to use it as a sample microservice.

```sh
$ wasmflow serve ./build/my_component_s.wasm --rpc --rpc-port 8060
2022-06-20T22:21:10  INFO GRPC server bound to 127.0.0.1 on port 8060
2022-06-20T22:21:10  INFO Starting RPC server
2022-06-20T22:21:10  INFO Waiting for ctrl-C
```

Then run the following command.

```sh
$ wasmflow invoke ./build/my_component_s.wasm concatenate -- --left=Jane --right=Doe |\
 wafl rpc invoke --port 8060 greet
{"output":{"value":"Hello Jane Doe"}}
```

In the next step we will publish our artifact to a remote registry so we can access it anywhere.
