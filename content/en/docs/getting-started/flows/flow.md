---
title: "How to Wire Existing Components Together"
linkTitle: "Flows & Manifests"
weight: 2
notoc: true
draft: false
description: >
  Bringing it all together
---

We've done a lot so far without ever touching the core of what's inside Wasmflow. Wasmflow is all about stitching together disparate logic into one application. We do that in `flow`s. Flows define how components and their ports connect together. It's a little like a network client connecting to a network port, except it's code connecting to other code.

Create a new file called `salutations.yaml` in your project root. You can put it anywhere and name it anything, just make sure you change the examples appropriately.

The current version of the manifest is version `1`. The format will change over time and the version will keep things working.

```yaml {title="./salutations.yaml"}
---
# yaml-language-server: $schema=https://wasmflow.com/schema.json
version: 1
```

First we pull in our external dependencies. Each key here is the namespace to use for the dependency.

```yaml {title="./salutations.yaml"}
---
# yaml-language-server: $schema=https://wasmflow.com/schema.json
---
version: 1
external:
  getting_started: reg.candle.run/candle/getting-started
```

{{< card title="Note" >}}
_As the name suggests, Wasmflow is WebAssembly-focused but that doesn't mean your external collections of components must be WASM. They can be external microservices, native binaries, workers over a message queue, or other manifests. The default assumes WASM. To specify a different collection type, use the extended form:_

```yaml
---
version: 1
external:
  getting_started: reg.candle.run/candle/getting-started
  my_microservice:
    kind: GrpcUrl
    url: 127.0.0.1:8080
```

{{< /card >}}

Next, we define our flow-based components:

```yaml {title="./salutations.yaml"}
# yaml-language-server: $schema=https://wasmflow.com/schema.json
---
version: 1
external:
  getting_started: reg.candle.run/candle/getting-started
components:
  hello:
```

The key `"hello"` here is the name of our flow-based component.

Next we need to define the collections this component has access to and the instances of a collection's components that we'll be using in our flow.

```yaml
# yaml-language-server: $schema=https://wasmflow.com/schema.json
---
version: 1
external:
  getting_started: reg.candle.run/candle/getting-started
components:
  hello:
    collections:
      - getting_started
    instances:
      greet: getting_started::greet
      concatenate: getting_started::concatenate
```

{{< card title="Note" >}}
_Instances are like pointers to a component. Multiple instances can point to the same component._
{{< /card >}}

Now we're ready to wire everything up.

```yaml {title="./salutations.yaml"}
# yaml-language-server: $schema=https://wasmflow.com/schema.json
---
version: 1
external:
  getting_started: reg.candle.run/candle/getting-started
components:
  hello:
    collections:
      - getting_started
    instances:
      greet: getting_started::greet
      concatenate: getting_started::concatenate
    flow:
      - <>.first_name -> concatenate.left
      - <>.last_name -> concatenate.right
      - concatenate.output -> greet.input
      - greet.output -> <>
```

The connections in a `flow` describe how an instance of a component connects to instances of other components. It works similarly to how you pipe commands together on the command line, except the connections are embedded in configuration and can have multiple ins and outs.

{{< card title="Tip" >}}
The `connections` above are written in short form syntax which is a light DSL that simplifies writing connections by hand. See [short form syntax documentation](/docs/configuration/short-form-syntax/) for more details.
{{< /card >}}

Now run your schematic directly with `wasmflow invoke`.

```sh
$ wasmflow invoke salutations.yaml hello -- --first_name=Jane --last_name=" Doe"
{"output":{"value":"Hello Jane  Doe"}}
```

Just as with a wasm file, start a GRPC microservice or HTTP server with `wasmflow serve`:

```sh
$ wasmflow serve salutations.yaml --rpc --rpc-port 8060
2021-11-16T14:19:39  INFO Starting RPC server
2021-11-16T14:19:39  INFO Host started
2021-11-16T14:19:39  INFO GRPC server bound to 127.0.0.1 on port 8060
2021-11-16T14:19:39  INFO Waiting for Ctrl-C
```

And use `wafl` the same way we already have.

```sh
$ wafl rpc invoke --port=8060 hello -- --first_name=Jane --last_name=Doe
{"output":{"value":"Hello Jane Doe"}}
```

We just turned a yaml file into a collection of new components with the same capabilities as compiled code. Next: [dive into the composability you just unlocked]({{< ref "/docs/getting-started/flows/composability" >}}).

[`wafl`]: /docs/tools/wafl/
