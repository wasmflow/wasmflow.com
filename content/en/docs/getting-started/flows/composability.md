---
title: "Composability in Wasmflow"
linkTitle: "Composability"
weight: 3
notoc: true
draft: false
description: >
  Coming full circle
---

Wasmflow was built for the sole reason of making software more reusable. Ultimately, that means making code connect with other code more easily. In the [Flow]({{< ref "/docs/getting-started/flows/flow" >}}) section we learn how manifests connect code to other code to provide new capabilities. This section keeps adding to that concept by having manifests depend on other manifests.

Save the following in a file called `host.yaml`.

```yaml {title="./host.yaml"}
---
# yaml-language-server: $schema=https://wasmflow.com/schema.json
---
version: 1
external:
  getting_started: reg.candle.run/candle/getting-started
  salutations:
    kind: Manifest
    reference: ./salutations.yaml
components:
  good_day:
    collections:
      - getting_started
      - salutations
    instances:
      hello: salutations::hello
      concatenate: getting_started::concatenate
      message:
        id: core::sender
        config:
          output: ", have a great day"
    flow:
      - <> -> hello.first_name
      - <> -> hello.last_name
      - hello.output -> concatenate.left
      - message.output -> concatenate.right
      - concatenate.output -> <>
```

{{< card title="Tip" >}}

```yaml
message:
  id: core::sender
  config:
    output: ", have a great day"
```

This new instance is one of the core Wasmflow components. It sends a value out of its output port.
{{< /card >}}

Run your new manifest via `wasmflow` just as you did the last.

```sh
$ wasmflow invoke host.yaml good_day -- --first_name=Jane --last_name="Doe"
{"output":{"value":"Hello Jane Doe, have a great day"}}
```

Now we've extended functionality simply by connecting the same modules we've already written without writing new code or rebuilding anything.

Collections, components, and flows are the core pieces for connecting disparate software via the same component/port interface we've been using in this guide. [Next: Publishing components]({{< ref "/docs/getting-started/publishing" >}})

[`wafl`]: /docs/tools/wafl/
