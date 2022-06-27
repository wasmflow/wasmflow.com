---
title: "Adding logic to your component"
linkTitle: "Adding logic"
weight: 5
notoc: true
description: >
  How to work with component ports
---

To get our component doing something we can see, add to `./src/components/greet.rs` so that it looks like the following:

```rs
pub use crate::components::generated::greet::*;

pub(crate) type State = ();

#[async_trait::async_trait]
impl wasmflow_sdk::v1::ephemeral::BatchedComponent for Component {
    type State = State;
    async fn job(
        inputs: Self::Inputs,
        outputs: Self::Outputs,

        state: Option<Self::State>,
        _config: Option<Self::Config>,
    ) -> Result<Option<Self::State>, Box<dyn std::error::Error + Send + Sync>> {
        let greeting = format!("Hello {}", inputs.input);
        outputs.output.done(greeting)?;
        Ok(state)
    }
}
```

Take note of these lines:

```rust
let greeting = format!("Hello {}", inputs.input);
outputs.output.done(greeting)?;
```

The first line uses Rust's `format!()` macro to format our input string into a suitable greeting. The second line takes that greeting string and pushes it to the output port named `output`. `done()` sends and closes a port in one command.

{{< card title="Tip" >}}
_Tip: Change the port names in your Apex schema and rebuild your component to see how the code generation reflects the changes._
{{< /card >}}

Build and run your component with the new logic to see the output:

```sh
$ make
$ wasmflow invoke ./build/my_component_s.wasm greet -- --input="my_input"
{"output":{"value":"Hello my_input"}}
```

{{< card title="Tip" >}}
_Tip: Change the `input.input` expression to something like `input.input.to_uppercase()` to see how modules can act on data as it comes through._
{{< /card >}}

The next tutorial goes over how to add additional components to our WebAssembly module.
