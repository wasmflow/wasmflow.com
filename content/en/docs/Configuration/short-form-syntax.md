---
title: "Short Form Syntax"
linkTitle: "Short Form Syntax"
description: >
  Alternative syntax for connections and connection targets
---

Writing out full [ConnectionDefinition][] structures in [Host Manifests](/docs/configuration/host-manifests/v0/) can be repetitive. An alternative is to specify either the entire [ConnectionDefinition][] or individual [ConnectionTargetDefinition][]s in short form syntax.

### Common Example

**Verbose form**

```yaml
- from:
    instance: instance1
    port: instance1_output
  to:
    instance: instance2
    port: instance2_input
```

**Alternative short form**

```yaml
- instance[instance1_output] => instance2[instance2_input]
```

### Schematic Input/Output example

A common practice is to mirror the port name of the connected ports as the name for the attached schematic input and output. The diamond, `<>` is short form for `<input>` when in the `from` position and `<output>` in the `to` position with a port name of the connected port.

**Verbose form**

```yaml
- from:
    instance: <input>   // <input> refers to the schematic input
    port: instance1_input
  to:
    instance: instance1
    port: instance1_input
- from:
    instance: instance1
    port: instance1_output
  to:
    instance: <output>
    port: instance1_output
```

**Alternative short form**

```yaml
- <> => instance1[instance1_input]
- instance1[instance1_output] => <>
```

### Schematic Input/Output with explicit port name

**Verbose form**

```yaml
- from:
    instance: <input>
    port: schematic_input
  to:
    instance: instance1
    port: instance1_input
```

**Alternative short form**

```yaml
- <>[schematic_input] => instance1[instance1_input]
```

### Specifying default input

It's common to specify default inputs to ports and this can be done by omitting the `from` reference and providing a `default` value. Defaults are parsed as JSON.

**Verbose form**

```yaml
- to:
    instance: instance1
    port: instance1_input
  default: '"Default input"'
```

**Alternative short form**

```yaml
- '"Default input" => instance1[instance1_input]'
```

### Specifying single ConnectionTarget

The short form for a [ConnectionTargetDefinition][] is valid on either the `from` or `to` fields individually.

**Verbose form**

```yaml
- from:
    instance: instance1
    port: instance1_output
  to:
    instance: instance2
    port: instance2_input
```

**Alternative short form**

```yaml
- from:
    instance: instance1
    port: instance1_output
  to: instance2[instance2_input]
```

[connectiondefinition]: /docs/configuration/host-manifests/v0/#ConnectionDefinition
[connectiontargetdefinition]: /docs/configuration/host-manifests/v0/#ConnectionTargetDefinition
