---
title: "Build and run your new WebAssembly component"
linkTitle: "Build and Run"
weight: 4
notoc: true
description: >
  Build and run your WebAssembly component with `wasmflow`
---

## Build your component.

Build your component by running `make`

```sh
$ make
```

Make generates source code, builds your WebAssembly module, generates keys, signs your module using [`wafl`] and stores both the signed and unsigned artifacts in the `/build` directory.

{{< card title="Tip" >}}
_The build process also generates source code from your schema(s). Do not edit any that are prefixed with a "This is generated" warning or your changes will be lost on next build._
{{< /card >}}

## Run your component

Use [`wasmflow`] to load your module and execute a component on the command line, sending the string `"my_input"` to the input port named `input`.

```sh
$ wasmflow invoke ./build/my_project.signed.wasm greet -- --input="my_input"


```

If all has gone well, you will see nothing. Our component doesn't output anything yet so naturally we don't get any interesting output. To convince yourself that something is actually happening, pass `--trace` or `--debug` to [`wasmflow`] for more output. Remember to add the flag before the `--` which separates arguments for [`wasmflow`] vs arguments destined for the executed module. You should see something similar to the below.

```sh
$ wasmflow invoke ./build/my_project.signed.wasm greet --trace -- --input="my_input"

2022-07-01T02:39:01 TRACE Logger initialized
2022-07-01T02:39:01 DEBUG Writing logs to ~/.local/state/wasmflow/logs
2022-07-01T02:39:01 DEBUG load as file location="./build/my_project.signed.wasm"
2022-07-01T02:39:01 TRACE bytes include wasm header? is_wasm=true bytes=[0, 97, 115, 109]
2022-07-01T02:39:01 DEBUG wasi enabled id=TODO config=Permissions { dirs: {} }
2022-07-01T02:39:01 DEBUG Collection permissions params=Permissions { dirs: {} }
2022-07-01T02:39:01 DEBUG WASI configuration params=WasiParams { argv: [], map_dirs: [], env_vars: [], preopened_dirs: [] }
2022-07-01T02:39:01 TRACE wasmtime instance loaded duration_μs=118656
2022-07-01T02:39:01 DEBUG wasmtime initialize duration_μs=118859
2022-07-01T02:39:01 DEBUG Input 'my_input' for argument 'input' is not valid JSON. Wrapping it with quotes to make it a valid string value.
2022-07-01T02:39:01 TRACE wasm invoke target=wafl://__local__.coll/greet
2022-07-01T02:39:01 DEBUG wasm invoke component="greet" id=4042109442 payload={"input": [168, 109, 121, 95, 105, 110, 112, 117, 116]}
2022-07-01T02:39:01 TRACE wasm call finished component="greet" id=4042109442 duration_μs=21125
2022-07-01T02:39:01 TRACE wasm call result component="greet" id=4042109442 result=Ok([])
2022-07-01T02:39:01 TRACE stream complete
```

{{< card title="Tip" >}}
_The name of your component is dictated by the `namespace` definition in your schema. The filename is normalized based on best practices for the target language._
{{< /card >}}

Next we'll [add logic to our component]({{< ref "/docs/getting-started/wasm-components/rust/extending-your-component" >}}).

[`wafl`]: /docs/tools/wafl/
