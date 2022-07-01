---
title: "What is Wasmflow?"
linkTitle: "What is Wasmflow?"
weight: 1
description: >
  An overview of Wasmflow.
---

At a high level, Wasmflow orchestrates code and manages communication between dependencies. That code can exist as WebAssembly modules, external microservices, workers over a queue, or anything else that speaks the Wasmflow protocol.

Wasmflow takes functional and flow-based concepts from projects like Erlang, Rx, FBP, and Haskell and combines them with concepts from Docker and Kubernetes.

# A Brief History of Containers

While Docker has become synonymous with containers, they didn't invent the concept. Virtual machines were the original software containers.

Full OS virtualization is where a _host_ machine runs one or more _guest_ operating systems by replicating every observable aspect of physical hardware as a "virtual machine." A hypervisor manages these guests and and acts like a "super" operating system. That is, an operating system that operates at a higher privilege.

{{< imgproc "abstraction-1" Fit "700x450" >}}
Hypervisors were the original orchestrators and abstracted hardware away from the OS.
{{< /imgproc >}}

Virtualization let users run multiple different operating systems simultaneously on the same hardware. More importantly though, was that it encapsulated the entire running state of an operating system into an easily distributable – albeit massive – VM image. You could automate the creation of a virtual machine then turn around and deploy that same image across a cluster with the press of a button. Before VMs, you'd frequently find system administrators manually imaging physical hard drives to install in new servers.

VMs were great but they were large and unwieldy. Building, maintaining, and licensing an operating system for every VM was wasteful and slow. The value was high but so was the cost. That's where os-level containers come into play.

OS-level virtualization – or containerization – is the paradigm where the operating system kernel partitions processes into isolated, virtual environments. It's not as flexible as a virtual machine – an os-level container must typically run on the same hardware and OS it was compiled for – but it's _much_ lighter. Containers gave users a lighter, more reusable format to distribute. Companies like Docker further innovated and added the concept of layered, composable images to further maximize reusability.

{{< imgproc "abstraction-2" Fit "700x450" >}}
Containers and container runtimes abstracted the process and its environment away from the host OS.
{{< /imgproc >}}

The industry invented containers because the value of VMs was clear as was the waste. Generating reliable, reproducible artifacts was great. Building the same exact systems repeatedly in slightly different ways wasn't.

Containers freed us from duplicating effort by abstracting at a deeper level. Now we see different duplication. We're building and rebuilding the same web applications, microservices, queue workers, workflows, build systems, and mobile apps just to house the small bit of code we want to run.

The next step is containerizing the code itself, separating business logic from generic application logic. This abstraction is the foundation of the "serverless" and functions-as-a-service trend which lets users deploy structured code to someone else's application. And that's where this story is still being written. "Serverless" isn't containerization. It's a product that exploits the problem. It doesn't solve it. To solve this abstraction we needed a cross-platform, cross-language, standard unit of distribution and a way to isolate it from its environment. That's not insurmountable, but it was a large ask. Until 2018.

{{< imgproc "abstraction-3" Fit "700x450" >}}
WebAssembly gives us the power to containerize and orchestrate code.
{{< /imgproc >}}

WebAssembly gave us a standardized, distributable bytecode and a runtime that baked in all the isolation and security that twenty-five years of securing the web taught us. Many languages already compile into it, it can run just about everywhere, and it is showing more potential day-by-day. It's barely usable and not very capable on its own. We need a runtime.

# Why Wasmflow?

Wasmflow embeds many of the best ideas from containerization, serverless, and general software development.

Wasmflow gives you:

- **Security** Wasmflow uses WebAssembly to sandbox all dependencies and further restricts functionality to CPU only by default. It isolates memory per-transaction so if an attacker did happen to exploit a component, they can only interact with their own memory.
- **Composability** You can compile code into Wasmflow components and connect them together with a manifest. After you're done? That manifest becomes a collection of new components that you can depend on the same way. It's components all the way down.
- **Productivity** Wasmflow normalizes the interfaces in and out of WebAssembly so everything connects together the same way. That means no more complex integrations and the freedom to swap in and out any dependency with little to no effort.
- **Testability** Since every component connects the same way, that also means no complex integrations simply to test code. You can use Wasmflow's own test runner to rapidly run unit tests on the command line.
- **Maintainability** Take any part of a Wasmflow application and scale it out as a microservice without rebuilding anything. Take another piece and turn it into a dozen workers on the other end of a message queue. Every boundary of a code container is a point that an application can be cut, molded, extended or scaled independently.

Wasmflow is an opinionated runtime that lets users turn their code into artifacts that can run as a web server, microservice, worker, CLI app, or on the client without any changes. It was built with security, reusability, and composability at its core.
