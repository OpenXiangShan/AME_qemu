# AME_qemu: AME Instruction Set Extension for QEMU

AME_qemu adds Attached Matrix Extension (AME) support, based on Ztt v0.6,
to QEMU's RISC-V TCG backend, enabling AME programs to run without AME hardware.

It supports system and Linux user emulation for AME functional verification,
compiler and runtime development, and operator validation.

## Key Features

- **AME Ztt v0.6 instructions:** resource, datatype, and state management;
  memory access, register moves, and data conversion; elementwise arithmetic
  and math functions; bitwise operations, comparisons, and predication;
  permutations; matrix multiply-accumulate; reductions and prefixes.
- **Configurable matrix resources:** matrix size, unit width, and M/Acc
  register counts, selected through CPU options.
- **Kernel examples:** FP32, INT8, and
  mixed-precision GEMM; RMSNorm, GELU, and Softmax; RoPE, MHA, and FFN.

## Quick Start

Run the following commands from the repository root on a Linux host. The
examples use RV64 bare-metal programs.

### Prerequisites

- QEMU build dependencies: a C/C++ compiler, Ninja, pkg-config, Python 3
  with venv, and development libraries for GLib, Pixman, zlib, and libfdt.
- A RISC-V GNU bare-metal toolchain with binutils and `libgcc.a`.
- An LLVM-based RISC-V toolchain with AME Ztt v0.6 support.

### Build QEMU

```sh
mkdir -p build
cd build
../configure \
  --target-list=riscv64-softmmu,riscv64-linux-user \
  --enable-fdt=system \
  --disable-docs
cd ..
ninja -C build qemu-system-riscv64 qemu-riscv64
```

This produces `build/qemu-system-riscv64` for system emulation and
`build/qemu-riscv64` for Linux user emulation. AME is enabled at runtime with
`-cpu max,xame=true`.

Then, install the toolchain into `toolchain/install`:

```sh
./scripts/build_toolchain.sh
```

Alternatively, build it from source:

```sh
./scripts/build_toolchain.sh --src
```

### Run examples and tests

Build and test FP32 GEMM:

```sh
make -C example gemm_fp32
```

Build and test all nine operators:

```sh
make -C example test
```

Test report: `example/build/reports/EXAMPLE_REPORT.md`.

## License

QEMU as a whole is licensed under GPLv2; see [LICENSE](LICENSE) and [COPYING](COPYING).
Individual files and components retain their own license notices.
