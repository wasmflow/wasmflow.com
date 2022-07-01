---
title: "Running your component via Wasmflow"
linkTitle: "Via Wasmflow"
weight: 1
notoc: true
description: >
  Use `wasmflow` on the command line to invoke your components.
---

The general CLI syntax to invoke a component is:

```sh console
wasmflow invoke [path or OCI URL] [Component name] -- [Input arguments]
```

For example:

```sh
$ wasmflow invoke reg.candle.run/candle/getting-started greet -- --input="my_input"
{"output":{"value":"Hello my_input"}}
```

By default, wasmflow won't print anything but the component's output. To print logs to the terminal, Pass `--trace` or `--debug` to [`wasmflow`] to print logs to the terminal, e.g.

```sh
$ wasmflow invoke reg.candle.run/candle/getting-started greet --trace -- --input="my_input"
2022-07-01T03:13:20 TRACE Logger initialized
2022-07-01T03:13:20 DEBUG Writing logs to /Users/jsoverson/.local/state/wasmflow/logs
2022-07-01T03:13:20 DEBUG load from cache path=/var/folders/pg/h7s94pd90w54hcbjpkvm58240000gn/T/wafl/ocicache/reg_candle_run_candle_getting-started.store
2022-07-01T03:13:20 TRACE bytes include wasm header? is_wasm=true bytes=[0, 97, 115, 109]
2022-07-01T03:13:20 DEBUG wasi enabled id=my-project config=Permissions { dirs: {} }
2022-07-01T03:13:20 DEBUG Collection permissions params=Permissions { dirs: {} }
2022-07-01T03:13:20 DEBUG WASI configuration params=WasiParams { argv: [], map_dirs: [], env_vars: [], preopened_dirs: [] }
2022-07-01T03:13:21 TRACE wasmtime instance loaded duration_μs=122273
2022-07-01T03:13:21 DEBUG wasmtime initialize duration_μs=122469
2022-07-01T03:13:21 DEBUG Input 'my_input' for argument 'input' is not valid JSON. Wrapping it with quotes to make it a valid string value.
2022-07-01T03:13:21 TRACE wasm invoke target=wafl://__local__.coll/greet
2022-07-01T03:13:21 DEBUG wasm invoke component="greet" id=213710532 payload={"input": [168, 109, 121, 95, 105, 110, 112, 117, 116]}
2022-07-01T03:13:21 TRACE wapc callback
2022-07-01T03:13:21 TRACE wapc callback command=0 arg1=output arg2=1 len=29
2022-07-01T03:13:21 TRACE output payload id=213710532 port="output" payload=[129, 162, 118, 49, 129, 161, 48, 129, 161, 49, 174, 72, 101, 108, 108, 111, 32, 109, 121, 95, 105, 110, 112, 117, 116]
2022-07-01T03:13:21 TRACE deserialized packet id=213710532 port="output" packet=V1(Success(Struct(String("Hello my_input"))))
2022-07-01T03:13:21 TRACE wapc callback done command=0 arg1=output arg2=1 duration_μs=169
2022-07-01T03:13:21 TRACE wapc callback
2022-07-01T03:13:21 TRACE wapc callback command=0 arg1=output arg2=1 len=13
2022-07-01T03:13:21 TRACE output payload id=213710532 port="output" payload=[129, 162, 118, 49, 129, 161, 50, 161, 48]
2022-07-01T03:13:21 TRACE deserialized packet id=213710532 port="output" packet=V1(Signal(Done))
2022-07-01T03:13:21 TRACE wapc callback done command=0 arg1=output arg2=1 duration_μs=111
2022-07-01T03:13:21 TRACE wasm call finished component="greet" id=213710532 duration_μs=21256
2022-07-01T03:13:21 TRACE wasm call result component="greet" id=213710532 result=Ok([])
2022-07-01T03:13:21 TRACE output message {"output":{"value":"Hello my_input"}}
{"output":{"value":"Hello my_input"}}
2022-07-01T03:13:21 TRACE output message {"output":{"signal":"Done","value":null}}
2022-07-01T03:13:21 TRACE stream complete
```
