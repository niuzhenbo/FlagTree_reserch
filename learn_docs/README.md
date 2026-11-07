# FlagTree 学习文档

这里保存 FlagTree 多 backend 的源码研究、设计分析、问题记录和实验结论。

## 目录约定

- `backend-overview/`：跨 backend 的综合问题，例如统一编译流程、compiler stages、IR 设计、runtime 接口、TLE/HINTS 共性机制和后端对比。
- 各 backend 目录：只记录对应 backend 的实现、调试、性能和适配问题。

新增文档时，优先按“问题所属范围”归档：

1. 影响多个 backend 的问题放入 `backend-overview/`。
2. 只影响一个 backend 的问题放入对应 backend 目录。
3. 同时包含共性机制和后端特化内容时，共性结论放在 `backend-overview/`，特化细节放在对应 backend 目录，并互相链接。

## Backend 目录

| Backend | 目录 |
|---|---|
| NVIDIA | [`nvidia/`](nvidia/) |
| NVIDIA TileIR | [`tileir/`](tileir/) |
| AMD | [`amd/`](amd/) |
| Enflame | [`enflame/`](enflame/) |
| ILUVATAR | [`iluvatar/`](iluvatar/) |
| HCU | [`hcu/`](hcu/) |
| Moore Threads | [`mthreads/`](mthreads/) |
| Thrive / DAMO Academy | [`thrive/`](thrive/) |
| RPU | [`rpu/`](rpu/) |
| MetaX | [`metax/`](metax/) |
| Sunrise | [`sunrise/`](sunrise/) |
| XPU | [`xpu/`](xpu/) |
| PPU | [`ppu/`](ppu/) |
| SpacemiT | [`spacemit/`](spacemit/) |
| Tsingmicro | [`tsingmicro/`](tsingmicro/) |
| Ascend | [`ascend/`](ascend/) |
| AIPU | [`aipu/`](aipu/) |
| CPU | [`cpu/`](cpu/) |
| Triton Shared | [`triton-shared/`](triton-shared/) |
| Cambricon | [`cambricon/`](cambricon/) |
