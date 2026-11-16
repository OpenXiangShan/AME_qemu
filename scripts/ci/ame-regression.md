# AME 服务器回归 CI

此流程用于 AME fork 的专用测试环境，不作为 QEMU 上游贡献内容；参见
[代码来源政策](../../docs/devel/code-provenance.rst)。

现有 `.github/workflows/ci.yml` 中的 `ame-regression` 作业使用
`runs-on: [self-hosted, bosc, open]`。分支 push、PR 和手动选择 `profile=all`
时运行；手动选择 `profile=ame-regression` 可只运行此组及矩阵准备作业。
其他已有作业保持各自的 runner 和触发条件。

## 共享测试输入

将 `/nfs/home/lihaoyan/autotest/build/qemu-regression/` 整个目录部署到：

```text
/nfs/share/ci-runner-RAISA/qemu_run_files/qemu-regression/
├── README.md
├── run.sh
└── elf/          # 895 个预编译 semihosting ELF
```

Runner 用户须能读取全部输入。测试包包含分类目录，不能只复制顶层文件。
共享目录只用于输入；每个 job 在 `$RUNNER_TEMP/ame-regression.XXXXXX/` 中独立
配置、构建并保存日志和报告。源码来自该 job 的 checkout，仅构建
`riscv64-softmmu`，测试使用刚构建的 `qemu-system-riscv64`。

## 环境与运行命令

服务器需要 Bash 4+、C 编译器、Make、Ninja、pkg-config、Python 3.12+（含 venv）、
GLib 和 libfdt 开发文件，以及 GNU coreutils 和 grep。Meson 及部分 Python 依赖
由 QEMU 的 configure 准备；缺少的子项目通过 `--enable-download` 获取。
Ubuntu 24.04 的系统依赖可由服务器管理员安装：

```sh
sudo apt-get install build-essential ninja-build pkg-config python3 python3-venv \
    libglib2.0-dev libfdt-dev zlib1g-dev coreutils grep
```

在服务器手动测试已有 AME QEMU 构建：

```sh
REPORT_DIR=$(mktemp -d /tmp/qemu-ame-results.XXXXXX) \
    bash /nfs/share/ci-runner-RAISA/qemu_run_files/qemu-regression/run.sh \
    /absolute/path/to/qemu-system-riscv64
```

CI 使用 `virt`、`max,xame=true`、`128M`，单例超时为 120 秒，构建并行度为 2，
job 总超时为 240 分钟。ELF 已预编译，无需 AME 交叉工具链。

`run.sh` 的退出码直接决定测试步骤结果，日志管道启用 `pipefail`。PASS 和
UNSUPPORTED 允许通过；FAIL、TIMEOUT 或配置错误使 job 失败。脚本逐例继续运行，
Actions summary 显示状态计数。即使构建或测试失败，也会尝试上传
`ame-regression-logs` artifact（保留 7 天），其中包括配置、构建、逐例命令与输出
以及 `summary.tsv`。取消或总超时导致的报告可能不完整，以 job 状态为准。
