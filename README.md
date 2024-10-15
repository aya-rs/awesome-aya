# Awesome Aya [![Awesome](https://awesome.re/badge.svg)](https://github.com/sindresorhus/awesome)

> A curated list of awesome eBPF 🐝 projects using aya-rs and Rust 🦀

eBPF is a technology that allows running user-supplied programs inside the Linux kernel.
For more info see https://ebpf.io/what-is-ebpf.

[Aya](https://github.com/aya-rs/aya) is an eBPF library built with a focus on operability
and developer experience. It does not rely on libbpf nor bcc - it's built from the ground
up purely in Rust, using only the libc crate to execute syscalls. With BTF support and
when linked with musl, it offers a true compile once, run everywhere solution, where
a single self-contained binary can be deployed on many linux distributions and kernel
versions.

Some of the major features provided include:

- Support for the **BPF Type Format** (BTF), which is transparently enabled when supported by
  the target kernel. This allows eBPF programs compiled against one kernel version to run
  on different kernel versions without the need to recompile.
- Support for function call relocation and global data maps, which allows eBPF programs to
  make **function calls** and use **global variables and initializers**.
- **Async support** with both tokio and async-std.
- Easy to deploy and fast to build: aya doesn't require a kernel build or compiled
  headers, and not even a C toolchain; a release build completes in a matter of seconds.

> Note: The eBPF ecosystem in general is constantly evolving, including Aya itself. We'd
> love your help to keep this list up to date. Please feel free to file an issue or make
> a PR if you would like to make a correction or want to have your awesome project included.

## Contributing

Contributions are welcome! Please see [the contributing guide](./CONTRIBUTING.md).
If you would like to have your project included in this list, please file a pull request.

## Contents

- [Reference Documentation](#reference-documentation)
- [Article and Presentations](#articles-and-presentations)
- [Small Tools that Use Aya](#small-tools-that-use-aya)
- [Major Projects that Use Aya](#major-projects-that-use-aya)
- [Aya eBPF-Side Libraries](#aya-ebpf-side-libraries)
- [Acknowledgements](#acknowledgements)

## Reference Documentation

- [The official Aya Discord server](https://discord.gg/xHW2cb2N6G) - Community support and discussion
- [The Aya Book](https://github.com/aya-rs/book) - The official Aya book, currently a work in progress
    - Compiled mdbook version is [available here](https://aya-rs.github.io/book/)
    - Some code examples are [available here](https://github.com/aya-rs/book/tree/main/examples)
- [The official Aya docs on docs.rs](https://docs.rs/aya/0.10.5/aya/) - Up-to-date documentation on the Aya userspace library
- [Aya templates for `cargo-generate`](https://github.com/aya-rs/aya-template) - An easy way to generate a new Aya project using [`cargo-generate`](https://github.com/cargo-generate/cargo-generate)

## Articles and Presentations

### Aya-related blog posts

- [The original announcement of the Aya project](https://confused.ai/posts/announcing-aya)
    - Covers the goals of the project and some early logistics
- [Adding BPF target support to the Rust compiler](https://confused.ai/posts/rust-bpf-target)
- [Aya: your tRusty eBPF companion](https://deepfence.io/aya-your-trusty-ebpf-companion/)
    - Shows examples how Aya makes it easier to write eBPF programs.
    - Explains how [Deepfence](https://deepfence.io/) uses Aya, especially XDP/TC packet filtering stack.
- [UDP Load-Balancer Demo using Aya](https://github.com/shaneutt/ebpf-rust-udp-loadbalancer-demo)
    - Provides a basic example of how to get started inspecting and modifying packets in XDP programs

### Aya-related talks

- [eBPF Summit 2021: Why Rust for writing eBPF programs](https://www.youtube.com/watch?v=HzXpnxUVZB0)
    - Alessandro Decina
- [eCHO episode 25: eBPF, Rust, and Aya](https://www.youtube.com/watch?v=TQ0ou-eFLAk)
    - Dave Tucker and Alessandro Decina discuss the Aya project and writing eBPF programs in Rust
- [Linux Plumbers Conference 2021: Improving the eBPF Developer Experience with Rust](https://www.youtube.com/watch?v=yCf6AYpA8u0)
    - Dave Tucker and Alessandro Decina

## Small Tools that Use Aya

- [suidsnoop](https://github.com/willfindlay/suidsnoop) - Uses Aya and eBPF LSM programs to implement audit logging and policy enforcement for suid binaries
    - Includes examples of:
        - Writing LSM programs in aya-bpf
        - Getting LSM program arguments in aya-bpf
        - Enforcing custom security policy in aya-bpf
        - Using `aya::AsyncPerfEventArray` to pass events to userspace
- [mybee](https://github.com/elbaro/mybee) - An eBPF profiler for MySQL 8.0
    - Aya is used to attach to mysqld uprobes.
    - mybee does not have to understand mysql wire protocol and utilize what mysqld already provides.
    - Uses less CPU than AF_PACKET or other eBPF-based tools that monitor TCP packets.
- [cir](https://github.com/seanyoung/cir) - An tool for loading infrared remote control keymaps for Linux
    - Aya is used to load, query and remove eBPF infrared decoders
    - Compiles IRP Notation to eBPF using LLVM
        - IRP Notation is compiled to LLVM IR using inkwell crate
        - LLVM libraries compile LLVM IR to valid object file
        - Aya loads object file
        - All done in-process, no external files are read/written, or any other dependencies
- [oryx](https://github.com/pythops/oryx) - A TUI (Terminal User Interface) for sniffing network traffic.

## Major Projects that Use Aya

- [lockc](https://github.com/rancher-sandbox/lockc) - An eBPF LSM-based MAC security audit system for container workloads
    - Works with Docker and Kubernetes (with containerd CRI)
    - Enforcing 3 pre-defined policy levels on containers
- [blixt](https://github.com/kong/blixt) - A [Kubernetes](https://kubernetes.io) [Gateway API](https://gateway-api.sigs.k8s.io) based Layer 4 Load-Balancer for ingress
    - Aya is used for eBPF code AND userspace (also uses [Kube-RS](https://github.com/kube-rs/kube) for control-plane)
    - TC is used on the Kubernetes nodes for load-balancer functionality
- [kunai](https://github.com/kunai-project/kunai) - A threat hunting/detection security monitoring tool:
    - Utilizes a bunch of Aya-based eBPF probes to generate security-relevant events for various cyber threat hunting and detection tasks.
    - Can serve as a log source for Incident Response and forensic analysis.
    - With the right rules, it can function as a real-time behavioral detection system.

## Aya eBPF-Side Libraries

- [aya-log](https://github.com/aya-rs/aya-log) - A logging library for eBPF programs written using aya-bpf
    - This is a fully-reusable logging library for eBPF programs written in aya
    - It provides a logging interface for eBPF programs that emulates Rust's standard log crate

## Acknowledgements

The original idea for [awesome](https://github.com/sindresorhus/awesome) comes from
[Sindre Sorhus](https://github.com/sindresorhus). The format of this repository is based
on [zoidbergwill's Awesome eBPF list](https://github.com/zoidbergwill/awesome-ebpf).

## License

[![CC0](http://mirrors.creativecommons.org/presskit/buttons/88x31/png/by-sa.png)](https://creativecommons.org/licenses/by-sa/4.0/)

All text in this repository is governed by the Creative Commons Attribution-ShareAlike 4.0 International License.
