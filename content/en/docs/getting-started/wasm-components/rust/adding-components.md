---
title: "Adding components"
linkTitle: "Adding components"
weight: 6
notoc: true
description: >
  Adding additional components to our module
---

A single component is like a single function. A collection of components is like a library. It's a collection of related functions.

Before we think about writing code, we have to define the contract first. The contract is a schema that describes the inputs, outputs, and configuration for a component.

Use `wafl component new [component name]` to create a new schema quickly. Let's call this component `concatenate`.

```sh
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

{{< card title="Note" >}}
_Without knowing anything about the implementation, can you guess what this component will do? If you guessed that it is going combine two strings together as output, you're correct!_

_Of course this scenario is contrived, but it's a signature part of contract driven development and extends beyond "Hello World" style tutorials. The contract is often more important than the name._
{{< /card >}}

### Generate the new code

The wasmflow code generator will generate all the necessary files based off the `.apex` files found in `./schemas/`.

Run it automatically with:

```sh
$ make codegen
```

### Add our concatenation logic

Add this rust code to the new `src/components/concatenate.rs` file.

```rust {title="./src/components/concatenate.rs"}
pub use crate::components::generated::concatenate::*;

#[async_trait::async_trait]
impl wasmflow_sdk::v1::ephemeral::BatchedComponent for Component {
    async fn job(
        inputs: Self::Inputs,
        outputs: Self::Outputs,
        config: Option<Self::Config>,
    ) -> Result<(), Box<dyn std::error::Error + Send + Sync>> {
        outputs
            .output
            .done(format!("{} {}", inputs.left, inputs.right))?;
        Ok(())
    }
}
```

### Build and run our new component

This command looks similar to our last command, but take note that we're sending data on multiple ports now.

```sh
$ make
$ wasmflow invoke ./build/my_project.signed.wasm concatenate -- --left=Hello --right=World
{"output":{"value":"Hello World"}}
```

Success! Now that we've got a module, let's see how we can [turn it into a microservice]({{< ref "/docs/getting-started/invoking-components/microservices" >}}).
