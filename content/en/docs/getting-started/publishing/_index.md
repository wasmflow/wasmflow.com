---
title: "Publishing components"
linkTitle: "Publishing components"
weight: 6
notoc: true
description: >
  Store and run signed artifacts from an OCI registry.
---

Wasmflow tools will automatically pull from an OCI registry if the passed filename isn't found on the local filesystem. If you don't have an OCI registry handy, you can start one easily with Docker.

```sh
$ docker run -it --rm -p 5000:5000 registry
```

{{< card title="Tip" >}}
_This will spin up a local registry with default settings. It's suitable for testing but production registries will need more configuration._
{{< /card >}}

### Publish your artifact

The `wafl registry push` command will publish WebAssembly, manifests, or multi-architecture binaries to OCI registries.

If you've built the sample component in this guide, use this command to push it to `test/getting-started:latest` on our local registry.

```sh console
$ wafl registry push 127.0.0.1:5000/test/getting-started:latest build/my_project.signed.wasm --insecure 127.0.0.1:5000
2022-06-20T22:23:50  INFO Pushing artifact...
Manifest URL: http://127.0.0.1:5000/v2/test/getting-started/manifests/sha256:d9117f3cd306b4f82179923d7cfd1283fb37e3ba3fa5b1faeac84964b208749d
Config URL: http://127.0.0.1:5000/v2/test/getting-started/blobs/sha256:44136fa355b3678a1146ad16f7e8649e94fb4fc21fe77e8310c060f61caaff8a
```

### Fetch your remote components

To run your components remotely all you do is pass the registry URL to vow in place of the filename we've used previously. `wasmflow` will take care of fetching and caching the remote artifact.

```sh
$ wasmflow invoke 127.0.0.1:5000/test/getting-started:latest greet --latest --insecure 127.0.0.1:5000 -- --input="Jane Doe"
{"output":{"value":"Hello Jane Doe"}}
```
