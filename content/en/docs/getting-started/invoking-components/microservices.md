---
title: "Running your WebAssembly component as a microservice"
linkTitle: "As a microservice"
weight: 2
notoc: true
description: >
  Use `wasmflow` to turn any WebAssembly module into a standalone microservice.
---

To turn any WebAssembly module or manifest into a microservice, change the command you use with `wasmflow` from `invoke` to `serve` and pass the `--rpc` flag to enable the GRPC service. You can supply a custom port with `--rpc-port`

If you're following the guide, you can use your own WebAssembly module in place of `reg.candle.run/candle/getting-started`

```sh
$ wasmflow serve reg.candle.run/candle/getting-started --rpc --rpc-port 8060
2022-06-20T22:16:51  INFO GRPC server bound to 127.0.0.1 on port 8060
2022-06-20T22:16:51  INFO Waiting for ctrl-C
2022-06-20T22:16:51  INFO Starting RPC server
```

{{< card title="Tip" >}}
Running `wasmflow serve` without providing a `--port` argument will cause `wasmflow` to automatically select a random, unused port.
{{< /card >}}

### Testing your microservice with `wafl`

`wafl` is the Wasmflow utility binary that can speak the Wasmflow RPC protocol (among other things).

Using a different terminal and the port from above, run `wafl rpc invoke` to make the same request we made in the last step.

```sh
$ wafl rpc invoke --port=8060 concatenate -- --left=Hello --right=World
{"output":{"value":"Hello World"}}
```

Congratulations, you just got a command line tool and a streaming GRPC microservice in one!

Next we're going to connect some components and see how we can start building large projects out of small ones. [Next: Flows]({{< ref "/docs/getting-started/flows/cli" >}})
