---
title: "Wrapping it up"
linkTitle: "Wrap-up"
weight: 12
draft: false
description: >
  Where to go next.
---

Great job getting this far! The network/schematic, provider/component model allows us to connect code from all over with configuration alone. Take a look at the [Manifest Documentation](/configuration/host-manifests/v0/) to find additional capabilities and documentation.

### Other providers

#### `GrpcUrl`

The `GrpcUrl` provider turns a GRPC microservice into a provider and components. Use the [`vino.proto`](https://releases.vino.dev/artifacts/latest/vino.proto) definition to automatically generate a compatible interface.

#### `Lattice`

The `Lattice` provider connects components across a [NATS](https://nats.io) message queue to distribute an application globally with no additional effort.

### Handling errors

Errors short-circuit downstream connections. There are two types of errors, `Error`s and `Exception`s. A component that fails completely, e.g. a component that panics or dies mid-execution, generates an `Error` and short circuits every downstream connection. Component developers can deliver `Exception`s to individual ports which short-circuits that path only. `Exception`s handle edge cases that can be analyzed or recovered from downstream, `Error`s are problems.

Short-circuited connections bypass all downstream components until it reaches a component that has a `default` value. That value is then substituted and normal execution continues.

[`wafl`]: /tools/wafl/
[`vow`]: /tools/vow/
[`vino`]: /tools/vino/
