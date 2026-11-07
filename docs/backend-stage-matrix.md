# FlagTree backend 与 compiler stages

基于当前工作区源码整理。当前 checkout 的 stage 名称来自各后端 `backend/compiler.py` 的 `add_stages()`。

## Backend 支持矩阵

| Backend ID | 厂商 / 目标 | 分支 | 源码状态 | 已注册 stages |
|---|---|---|---|---|
| nvidia | NVIDIA GPU | main | 当前 checkout | ttir, ttgir, llir, ptx, cubin |
| tileir | NVIDIA TileIR | main | 当前 checkout | ttir, tileir, cubin |
| amd | AMD GPU | main | 当前 checkout | ttir, ttgir, llir, amdgcn, hsaco |
| enflame | Enflame / 燧原 | main | 当前 checkout | ttir, ttgir, llir, gcuir, fatbin |
| iluvatar | ILUVATAR / 天数智芯 | main | 当前 checkout | ttir, ttgir, llir, asm, cubin |
| hcu | HYGON / 海光信息 | main | 当前 checkout | ttir, ttgir, llir, amdgcn, hsaco |
| mthreads | Moore Threads / 摩尔线程 | main | 当前 checkout | ttir, ttgir, llir, mubin |
| thrive | DAMO Academy / 达摩院（README 别名 damoacademy） | main | 当前 checkout | ttir, thvtileir, thvpeir, tttir, memref, llir, dfcafb |
| rpu | RPU | main | 当前 checkout | ttir, rpuplan, rpuexec, rpurc, rpuinstr, rpuelf, rpubin |
| metax | MetaX / 沐曦 | main | 当前 checkout | ttir, ttgir, mlir, llir, mcfatbin |
| sunrise | Sunrise / 曦望芯科 | main | 当前 checkout | ttir, ttgir, llir, asm, stcu |
| xpu | Intel XPU | main | 当前 checkout | ttir, ttxir, llir, elf, xpubin |
| ppu | PPU | main | 当前 checkout | ttir, ttgir, llir, hgbin |
| spacemit | SpacemiT / 进迭时空 | main | 当前 checkout | ttir, linalgdir, llir, so |
| tsingmicro | Tsingmicro / 清微智能 | main | 当前 checkout | ttir, coreir, txir, llir, so |
| ascend | Huawei Ascend / 昇腾 | triton_v3.5.x | 保护分支（当前 checkout 无目录） | 源码需切换到对应分支核对 |
| aipu | ARM China AIPU | triton_v3.3.x | 保护分支（当前 checkout 无目录） | 源码需切换到对应分支核对 |
| cpu | ARM64 / x86_64 CPU | triton_v3.3.x | 保护分支（当前 checkout 无目录） | 源码需切换到对应分支核对 |
| triton-shared | Triton Shared CPU | triton_v3.3.x | 外部组件（当前 checkout 无目录） | 源码需切换到对应分支核对 |
| cambricon | Cambricon / 寒武纪 | triton_v3.2.x | 保护分支（当前 checkout 无目录） | 源码需切换到对应分支核对 |

## 当前 checkout 的全部 stage 与生成物

| Stage key | 生成物类型 | 生成物含义 | 使用该 stage 的 backend |
|---|---|---|---|
| `ttir` | Triton Tensor IR | Triton 前端生成并经 TTIR 优化后的 MLIR 模块 | nvidia, tileir, amd, enflame, iluvatar, hcu, mthreads, thrive, rpu, metax, sunrise, xpu, ppu, spacemit, tsingmicro |
| `ttgir` | TritonGPU IR | 加入线程、warp、CTA、layout 和 GPU memory 语义后的 MLIR 模块 | nvidia, amd, enflame, iluvatar, hcu, mthreads, metax, sunrise, ppu |
| `llir` | LLVM IR | LLVM dialect/LLVM IR，接近目标代码生成 | nvidia, amd, enflame, iluvatar, hcu, mthreads, thrive, metax, sunrise, xpu, ppu, spacemit, tsingmicro |
| `ptx` | PTX | NVIDIA PTX 汇编文本 | nvidia |
| `cubin` | CUBIN | NVIDIA CUDA GPU 二进制模块 | nvidia, tileir, iluvatar |
| `tileir` | TileIR | NVIDIA TileIR 专用中间表示 | tileir |
| `amdgcn` | AMDGCN assembly | AMD GPU 汇编文本 | amd, hcu |
| `hsaco` | HSACO | AMD GPU 可加载 code object | amd, hcu |
| `gcuir` | GCU IR | Enflame GCU 中间表示 | enflame |
| `fatbin` | Enflame fatbin | Enflame 可加载 fat binary | enflame |
| `asm` | Target assembly | 厂商目标汇编文本；Sunrise/ILUVATAR 使用 | iluvatar, sunrise |
| `mubin` | MUBIN | Moore Threads GPU 二进制模块 | mthreads |
| `thvtileir` | THV Tile IR | thrive 后端 Tile 级中间表示 | thrive |
| `thvpeir` | THV PE IR | thrive 后端处理器执行级中间表示 | thrive |
| `tttir` | THV Triton IR | thrive 后端专用 Triton IR | thrive |
| `memref` | MemRef IR | thrive 后端转换出的 MLIR MemRef 表示 | thrive |
| `dfcafb` | Dfcafb fatbin | thrive 后端最终 fat binary | thrive |
| `rpuplan` | RPU plan | RPU 执行计划 | rpu |
| `rpuexec` | RPU exec | RPU 执行描述/中间产物 | rpu |
| `rpurc` | RPU RC | RPU runtime configuration 产物 | rpu |
| `rpuinstr` | RPU instructions | RPU 指令流 | rpu |
| `rpuelf` | RPU ELF | RPU ELF 目标文件 | rpu |
| `rpubin` | RPU binary | RPU 最终二进制 | rpu |
| `mlir` | MetaX MLIR | MetaX 后端专用 MLIR 阶段 | metax |
| `mcfatbin` | MetaX fatbin | MetaX 可加载 fat binary | metax |
| `stcu` | STCU | Sunrise/Tang 可加载设备二进制；由 assembly 打包生成 | sunrise |
| `ttxir` | TTXIR | XPU 后端专用中间表示 | xpu |
| `elf` | ELF | XPU ELF 目标文件 | xpu |
| `xpubin` | XPU binary | XPU 最终可加载二进制 | xpu |
| `hgbin` | HGBIN | PPU/HCU 目标二进制模块 | ppu |
| `linalgdir` | Linalg dialect IR | SpacemiT 后端的 Linalg 方言 IR | spacemit |
| `so` | Shared object | 生成 `.so` 动态库 | spacemit, tsingmicro |
| `coreir` | CoreIR | Tsingmicro 后端 CoreIR | tsingmicro |
| `txir` | TXIR | Tsingmicro 后端 TXIR | tsingmicro |

> 说明：保护分支 backend 在本 checkout 中没有对应源码目录，因此其 stage 未臆测。
