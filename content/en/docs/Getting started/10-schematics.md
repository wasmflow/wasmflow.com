---
title: "Wiring your component into a schematic"
linkTitle: "Schematics"
weight: 10
draft: false
description: >
  Bringing it all together
---

We've done a lot so far without ever touching the core of what's inside Wasmflow. Wasmflow is all about stitching together disparate logic into one application. We do that in `schematic`s. Schematics define how components and their ports connect together. It's a little like a network client connecting to a network port, except it's code connecting to other code.

Create a new file called `salutations.yaml` in your project root. You can put it anywhere and name it anything, just make sure you change the examples appropriately.

At this point all manifests are version `0`. The format may change over time and the version will keep things working.

```yaml {title="./salutations.yaml"}
---
# yaml-language-server: $schema=https://wasmflow.com/schemas/manifest/v0.json
version: 0
```

Next we start defining the `network` portion of the manifest. A Wasmflow network is a collection of schematics. The first step in defining a network is defining the providers `vino` will initialize.

```yaml {title="./salutations.yaml"}
---
# yaml-language-server: $schema=https://vino.dev/schemas/manifest/v0.json
version: 0
network:
  providers:
    - namespace: my_component
      kind: WaPC
      reference: ./build/my_component_s.wasm
```

The `namespace` is how you'll reference the provider in your schematics. The `kind` denotes the type of provider. There are multiple kinds but we're focusing on the WaPC WebAssembly provider for now. `reference` is a `kind`-specific way of finding or initializing the provider. For `WaPc` providers, the `reference` is a filesystem path or OCI url.

Next, we define our schematics:

```yaml {title="./salutations.yaml"}
---
# yaml-language-server: $schema=https://vino.dev/schemas/manifest/v0.json
version: 0
network:
  providers:
    - namespace: my_component
      kind: WaPC
      reference: ./build/my_component_s.wasm
  schematics:
    - name: hello
      providers:
        - my_component
      instances:
        greet:
          id: my_component::greet
        concatenate:
          id: my_component::concatenate
```

A schematic's `name` is how you'll reference it by external invocation or from within other schematics.

The `providers` field is a list of providers (by namespace) that this schematic has access to.

The `instances` are named pointers to components in a namespace. In this case `greet` points to `my_component::greet` and `concatenate` points to `my_component::concatenate`. Think of `instances` like variables. Multiple `instances` can point to the same component, but each `instance` can be referenced separately. This is important for the next section: `connections`.

```yaml {title="./salutations.yaml"}
---
# yaml-language-server: $schema=https://vino.dev/schemas/manifest/v0.json
version: 0
network:
  providers:
    - namespace: my_component
      kind: WaPC
      reference: ./build/my_component_s.wasm
  schematics:
    - name: hello
      providers:
        - my_component
      instances:
        greet:
          id: my_component::greet
        concatenate:
          id: my_component::concatenate
      connections:
        - <>[first_name] => concatenate[left]
        - <>[last_name] => concatenate[right]
        - concatenate[output] => greet[input]
        - greet[output] => <>
```

`connections` are how an instance of a component connect to other instances of other components. It works just like how you piped commands together on the command line, except the connections are now embedded in configuration.

{{% pageinfo %}}
The `connections` above are written in [short form syntax](/configuration/short-form-syntax/) which is a light layer of abstraction to simplify writing connections by hand.

###### `<>[first_name] => concatenate[left]`

- Defines a schematic input port `first_name` and connects it to the `left` port of `concatenate`.

###### `<>[last_name] => concatenate[right]`

- Does the same for `last_name` and `right`

###### `concatenate[output] => greet[input]`

- Connects the output of `concatenate` to the `input` port of `greet`.

###### `greet[output] => <>`

- Connects the output of `greet` to an output port on the schematic of the same name (in this case, `output`).

`<>` in upstream position refers to the schematic's input. In downstream position, `<>` refers to a schematic's output. Using `<>` without a port name simply takes the port name of the connected port. See the [short form syntax](/configuration/short-form-syntax/) for more information.

Manifest generators and UIs will generate connections in expanded syntax like below:

```yaml
connections:
  - from:
      instance: input
      port: first_name
    to:
      instance: concatenate
      port: left
  - from:
      instance: input
      port: last_name
    to:
      instance: concatenate
      port: right
  - from:
      instance: concatenate
      port: output
    to:
      instance: greet
      port: input
  - from:
      instance: greet
      port: output
    to:
      instance: output
      port: output
```

{{%/ pageinfo %}}

Now run your schematic directly with `vino run`.

```sh
$ vino run salutations.yaml hello -- --first_name=Jane --last_name=" Doe"
{"output":{"value":"Hello Jane Doe"}}
```

Just as with a wasm file, start a GRPC microservice or HTTP server with `vino serve`:

```sh
$ vino serve salutations.yaml --rpc --rpc-port 8060
2021-11-16T14:19:39  INFO Starting RPC server
2021-11-16T14:19:39  INFO Host started
2021-11-16T14:19:39  INFO GRPC server bound to 127.0.0.1 on port 8060
2021-11-16T14:19:39  INFO Waiting for Ctrl-C
```

And use `wafl` the same way we already have.

```sh
$ wafl invoke --port=8060 hello -- --first_name=Jane --last_name=Doe
{"output":{"value":"Hello Jane Doe"}}
```

We just turned this manifest's network into a provider with each schematic as a new component.

[`wafl`]: /tools/wafl/
[`vow`]: /tools/vow/
[`vino`]: /tools/vino/
[short form syntax]: /configuration/short-form-syntax/
