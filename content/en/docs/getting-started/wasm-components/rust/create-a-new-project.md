---
title: "Create a new project"
linkTitle: "Create a new project"
weight: 3
notoc: true
description: >
  Using `wafl` to create a new boilerplate project
---

## Start a new project with `wafl`

```sh
$ wafl project new my-project rust
```

`wafl` will set up a new directory named `my-project` in the current directory and will clone a suitable boilerplate project. You can specify a git repository URL in place of the language argument to use your own boilerplate projects.

{{< card title="Note" >}}
_The default boilerplate project comes with some suggested settings for Visual Studio Code users. Feel free to ignore, change, or remove them. They are not required to build the project._
{{< /card >}}

## Your first schema

Take note of the schema found in `schemas/my-component.apex`

```graphql
namespace "my-component"

type Inputs {
  input: string
}

type Outputs {
  output: string
}

type Config {

}

```

This schema defines a component named `my-component` with one input port named `input` and one output port named `output`, both dealing with `string` data. The schema also defines a `Config` type that is currently empty.

Let's change our component name to 'greet' so we have something slightly more meaningful. Your new schema should look like this:

```graphql
namespace "greet"

type Inputs {
  input: string
}

type Outputs {
  output: string
}

type Config {

}
```

Finally, let's finish this first step by adding our personal and project details to the `Cargo.toml` file. You can fill out the name, description, and authors to be anything you want, but the rest of this guide assumes your project is named `my-project`. Change the built artifact names accordingly if you use something different.

```toml
[package]
name = "my-project"
version = "0.0.0"
description = "A cool new project"
authors = ["You <you@email.com>"]
edition = "2018"
license = "BSD-3-Clause"
```

Next we'll [Build and Run]({{< ref "/docs/getting-started/wasm-components/rust/build-and-run" >}}) your component.
