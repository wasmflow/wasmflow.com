---
title: "Running your WebAssembly component as a microservice"
linkTitle: "WebAssembly Microservices"
weight: 7
description: >
  Use `wasmflow` to turn any WebAssembly module into a standalone microservice.
---

To turn your WebAssembly module into a microservice, change the command you use with `wasmflow` from `invoke` to `serve` and remove the extra flags. That's it.

```sh
$ wasmflow serve ./build/my_component_s.wasm --rpc --rpc-port 8060
2022-06-20T22:16:51  INFO GRPC server bound to 127.0.0.1 on port 8060
2022-06-20T22:16:51  INFO Waiting for ctrl-C
2022-06-20T22:16:51  INFO Starting RPC server
```

{{% pageinfo %}}
_Tip: Starting `wasmflow serve` without providing a `--port` argument will cause `wasmflow` to automatically selecte a random, unused port._
{{% /pageinfo %}}

### Testing your microservice with `wafl`

`wafl` is the vino controller which can talk to providers (such as your WebAssembly module) over the Wasmflow RPC protocol. Using the port from above, run `wafl rpc invoke` to make the same request we made in the last step.

```sh
$ wafl rpc invoke --port=8060 concatenate -- --left=Hello --right=World
{"output":{"value":"Hello World"}}
```

Congratulations, you just got a command line tool and a streaming GRPC microservice with a few lines of Rust and WebAssembly!

Next we're going to connect some components and see how we can start building.
