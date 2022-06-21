---
title: "Adding components"
linkTitle: "Adding components"
weight: 6
description: >
  Adding additional components to our module
---

A single component is analogous to a simple function. A collection of components then acts like a library of related functions.

Before we think about writing code, we have to define the contract first. The contract is a schema that describes the inputs, outputs, and configuration for a component.

Use `wafl component new [component name]` to create a new schema quickly. Let's call this component `concatenate`.

```console
$ wafl component new concatenate
2022-06-20T22:09:04  INFO Creating new schema for concatenate at schemas/concatenate.apex
```

Edit the Inputs and Outputs in your schema to look like the following.

```graphql {title="./schemas/concatenate.apex"}
type Inputs {
  left: string
  right: string
}

type Outputs {
  output: string
}
```

{{% pageinfo %}}
_Without knowing anything about the implementation, can you guess what this component will do? If you guessed that it is going combine two strings together as output, you're correct!_

_This scenario is contrived but it extends beyond "Hello World" style tutorials. It's an important part of programming on Wasmflow. Components don't need to accept data they don't act on, so you won't often see many inputs nor complex types as input. You'll frequently deal with small bits of data like strings and numbers and aggregate them at some final points. Component ports connect directly to each other in a graph or [flow](/concepts/terminology)), freeing each component to be laser focused on its purpose._
{{% /pageinfo %}}

### Generate the new code

The make task `codegen` will generate all the necessary files based off the `.apex` files found in `schemas/`. Running `make codegen` will automatically generate the integration and boilerplate code necessary to get started without hassle.

```sh
$ make codegen
```

### Add our concatenation logic

Add this rust code to the new `src/components/concatenate.rs` file.

```rust {title="./src/components/concatenate.rs"}
pub use crate::components::generated::concatenate::*;

pub(crate) type State = ();

#[async_trait::async_trait]
impl wasmflow_sdk::v1::ephemeral::BatchedComponent for Component {
    type State = State;
    async fn job(
        inputs: Self::Inputs,
        outputs: Self::Outputs,

        state: Option<Self::State>,
        config: Option<Self::Config>,
    ) -> Result<Option<Self::State>, Box<dyn std::error::Error + Send + Sync>> {
        outputs
            .output
            .done(format!("{} {}", inputs.left, inputs.right))?;
        Ok(state)
    }
}
```

### Build and run our new component

This command looks similar to our last command, but take note that we're sending data on multiple ports now.

```sh
$ make
$ wasmflow invoke ./build/my_component_s.wasm concatenate -- --left=Hello --right=World
{"output":{"value":"Hello World"}}
```

Success!
