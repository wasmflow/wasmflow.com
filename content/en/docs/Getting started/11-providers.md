---
title: "Wiring networks onto networks"
linkTitle: "Networks as providers"
weight: 11
notoc: true
draft: false
description: >
  Coming full circle
---

Now that we can make new providers and components out of a yaml file, we can connect them the same way we did with WASM modules by using a provider kind of `Network`. As with `WaPC`, the `reference` is a filesystem path or OCI url.

Save the following in a file called `host.yaml`.

```yaml {title="./host.yaml"}
---
# yaml-language-server: $schema=https://vino.dev/schemas/manifest/v0.json
version: 0
network:
  providers:
    - namespace: salutations
      kind: Network
      reference: ./salutations.yaml
    - namespace: my_component
      kind: WaPC
      reference: ./build/my_component_s.wasm
  schematics:
    - name: good_day
      providers:
        - salutations
        - my_component
      instances:
        hello:
          id: salutations::hello
        concatenate:
          id: my_component::concatenate
      connections:
        - <> => hello[first_name]
        - <> => hello[last_name]
        - hello[output] => concatenate[left]
        - '", have a great day" => concatenate[right]'
        - concatenate[output] => <>
```

{{< card title="Tip" >}}

###### `'", have a great day" => concatenate[right]'`

This new line shows how to define defaults inline with connections. Wasmflow parses `", have a great day"` as JSON and delivers it to the connected port. It is equivalent to the following expanded syntax:

```yaml
- to:
    instance: concatenate
    port: right
  default: ", have a great day"
```

{{< /card >}}

Run your new manifest via `vino run` just as you did the last.

```sh
$ vino run host.yaml good_day -- --first_name=Jane --last_name=" Doe"
{"output":{"value":"Hello Jane Doe, have a great day"}}
```

Now we've extended functionality simply by connecting the same modules we've already written without writing new code or rebuilding anything.

Providers and manifests are the core mechanism for connecting disparate software via the same component/port interface we've been using in this guide.

[`wafl`]: /tools/wafl/
[`vow`]: /tools/vow/
[`vino`]: /tools/vino/
