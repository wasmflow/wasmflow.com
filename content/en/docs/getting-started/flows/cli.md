---
title: "Experimenting on the Command Line"
linkTitle: "Starting on the CLI"
weight: 1
notoc: true
description: >
  Start connecting simple components on the command line.
---

The command line is a pipeline of streaming data where every component feeds directly into one other. To Wasmflow it's like a flow that never branches. It's a great way to test and experiment with small bits of logic.

The following command pipes one execution of wasmflow into another, both running the same `.wasm` file but pointing to different components.

```sh
$ wasmflow invoke reg.candle.run/candle/getting-started concatenate -- --left=Jane --right=Doe |\
 wasmflow invoke reg.candle.run/candle/getting-started greet
{"output":{"value":"Hello Jane Doe"}}
```

`wasmflow` output is directly pipable to another execution of `wasmflow`! You can string together any number of connections to experiment, test, or build on the command line.

{{< card title="Tip" >}}
_`wasmflow` connects output from a named port to an incoming port of the same name with one exception shown above. Data coming out of a port named `output` will be mapped to a port named `input` on the downstream component._

_This is a common practice and makes CLI testing more intuitive. Turn it off by passing the `--raw` flag to `wasmflow`._
{{< /card >}}

### Connecting to remote components

Every tool in the Wasmflow suite talks the same language so switching from one to another should be seamless. This means that you can pipe from `wasmflow` running WebAssembly locally to `wafl` which connects to any remote provider and then back to `wasmflow` again.

Start serving a WebAssembly module with `wasmflow` to use it as a sample microservice.

```sh
$ wasmflow serve reg.candle.run/candle/getting-started --rpc --rpc-port 8060
2022-06-20T22:21:10  INFO GRPC server bound to 127.0.0.1 on port 8060
2022-06-20T22:21:10  INFO Starting RPC server
2022-06-20T22:21:10  INFO Waiting for ctrl-C
```

Then run the following command in another terminal.

```sh
$ wasmflow invoke reg.candle.run/candle/getting-started concatenate -- --left=Jane --right=Doe |\
 wafl rpc invoke --port 8060 greet
{"output":{"value":"Hello Jane Doe"}}
```

From here, let's move on to more complex connections with [Flows]({{< ref "/docs/getting-started/flows/flow" >}}).
