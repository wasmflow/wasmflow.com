---
title: "Adding logic to your component"
linkTitle: "Adding logic"
weight: 5
notoc: true
description: >
  How to work with component ports
---

Open up the newly generated component file at `./src/components/greet.rs` and add to it so it looks like this:

```rs
pub use crate::components::generated::greet::*;

#[async_trait::async_trait]
impl wasmflow_sdk::v1::ephemeral::BatchedComponent for Component {
    async fn job(
        inputs: Self::Inputs,
        outputs: Self::Outputs,
        _config: Option<Self::Config>,
    ) -> Result<(), Box<dyn std::error::Error + Send + Sync>> {
        let greeting = format!("Hello {}", inputs.input);
        outputs.output.done(greeting)?;
        Ok(())
    }
}
```

Take note of these lines:

```rust
let greeting = format!("Hello {}", inputs.input);
outputs.output.done(greeting)?;
```

The first line uses Rust's `format!()` macro to format our input string into a suitable greeting.

The second line takes that greeting string and pushes it to the output port named `output`.

Finally, `done()` sends a packet and closes the port in one command.

{{< card title="Tip" >}}
_Change the port names in your Apex schema and rebuild your component to see how the code generation reflects the changes._
{{< /card >}}

Build and run your component with the new logic to see the output:

```sh
$ make
$ wasmflow invoke ./build/my_project.signed.wasm greet -- --input="my_input"
{"output":{"value":"Hello my_input"}}
```

{{< card title="Tip" >}}
_Change the `inputs.input` expression to something like `inputs.input.to_uppercase()` to see how modules can act on data as it comes through._
{{< /card >}}

Next we'll [add new components]({{< ref "/docs/getting-started/wasm-components/rust/adding-components" >}}) to our collection.
