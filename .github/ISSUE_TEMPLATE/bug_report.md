---
name: Bug report
about: Report a bug in QEMU or the AME / Ztt extension
title: ''
labels: bug
assignees: ''
---

<!--
This tracker is for the AME QEMU fork. Search existing issues before reporting.
If possible, reproduce the problem with the latest revision of this fork.
State whether you have also tested unmodified upstream QEMU, where applicable.

Do not report potential security issues in a public issue. Consult QEMU's
security boundary in docs/system/security.rst and follow the confidential
reporting process at https://www.qemu.org/contribute/security-process/.
TCG-only emulation bugs are not treated as security bugs by QEMU.
-->

## Description of problem
<!-- Describe what happens, including any error or crash messages. -->

## Expected behavior
<!-- Describe what you expected to happen. -->

## Host environment
- Operating system:
- OS/kernel version: <!-- For POSIX hosts, include `uname -a`. -->
- Architecture: <!-- x86_64, aarch64, riscv64, etc. -->
- QEMU executable: <!-- qemu-system-riscv64, qemu-riscv64, qemu-img, etc. -->
- QEMU version: <!-- Include the output of `<qemu-executable> --version`. -->
- Branch and commit: <!-- `git branch --show-current` and `git rev-parse HEAD`. -->
- Host compiler and version:

## Build configuration and local modifications
<!-- Include the exact configure options and build command, or package source. -->

```shell
# Configure and build commands
```

- Local source modifications: <!-- Describe any changes, or state "none". -->

## Emulated/Virtualized environment
<!-- Use N/A for fields that do not apply, such as a guest OS for qemu-img. -->
- Operating system: <!-- Include the distribution, or state "bare metal". -->
- OS/kernel version:
- Architecture:
- Machine and CPU model: <!-- Include all `-machine` and `-cpu` properties. -->
- Accelerator: <!-- tcg, kvm, hvf, etc.; N/A for user-mode emulation. -->
- Guest compiler/toolchain and version:

## AME / Ztt details (if applicable)
<!-- Remove this section for issues unrelated to AME. -->
- Specification version and relevant section: <!-- For example, Ztt 0.6. -->
- AME CPU properties: <!-- xame, ame-n, ame-unit-bits, ame-m-regs, ame-acc-regs. -->
- Affected instructions and element types:
- Relevant CSR/register state:

## Steps to reproduce
<!--
Assume a clean checkout. Include guest build commands, compiler flags, input
files and any scripts or parameters needed to reproduce the problem.
If using libvirt, include the generated QEMU command line and domain XML.
-->
1.
2.
3.

### QEMU command line
<!-- Give the smallest, complete command line that exhibits the problem. -->

```shell
# QEMU command
```

### Reproducer
<!--
Include a minimal source program or a link to its source, with build commands.
If attaching a binary, explain where it came from and how it was built.
For an existing test, include its path, invocation and any required inputs.
-->

## Actual output
<!-- Paste relevant terminal output. Attach longer logs or stack traces. -->

```text
# Terminal output
```

## Additional information
<!--
Include logs, backtraces, screenshots, related issues or commits as useful.
For a regression, include the last known working commit or a bisect result.
For incorrect AME results, include input data, expected and actual values,
and the reference used to calculate the expected values.

If automated tools (LLMs, static analysis, fuzzers, etc.) helped discover this
issue, disclose their use. Manually validate the reproduction steps and all
reported findings before submitting.
-->
