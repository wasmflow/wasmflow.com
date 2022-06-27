---
title: "Publishing components"
linkTitle: "Publishing components"
weight: 9
notoc: true
description: >
  Store and run signed artifacts from an OCI registry.
---

Wasmflow tools will automatically pull from an OCI registry if the passed filename isn't found on the local filesystem. If you don't have an OCI registry handy, you can start one easily with Docker.

```sh
docker run -it --rm -p 5000:5000 registry
```

{{< card title="Tip" >}}
_Note: This will spin up an insecure local registry. It's suitable for testing but production registries will need more configuration._
{{< /card >}}

### Publish your artifact

Run the following command to publish `build/my_component_s.wasm` as `test/my-component:latest` on our local registry.

```sh console
$ wafl registry push 127.0.0.1:5000/test/my-component:latest build/my_component_s.wasm --insecure 127.0.0.1:5000
2022-06-20T22:23:50  INFO Pushing artifact...
Manifest URL: http://127.0.0.1:5000/v2/test/my-component/manifests/sha256:d9117f3cd306b4f82179923d7cfd1283fb37e3ba3fa5b1faeac84964b208749d
Config URL: http://127.0.0.1:5000/v2/test/my-component/blobs/sha256:44136fa355b3678a1146ad16f7e8649e94fb4fc21fe77e8310c060f61caaff8a
```

### Fetch your remote components

To run your components remotely all you do is pass the registry URL to vow in place of the filename we've used previously. `wasmflow` will take care of fetching and caching the remote artifact.

```sh
$ wasmflow invoke 127.0.0.1:5000/test/my-component:latest greet --latest --insecure 127.0.0.1:5000 -- --input="Jane Doe"
{"output":{"value":"Hello Jane Doe"}}
```

Hopefully everything feels like it "just works". That's what we're striving for with Wasmflow. Now that we can make simple connections between local and remote components, it's time to step it up to more complex configurations.
