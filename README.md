<div align="center">

# 🎬 ComfyUI 8GB Workflows

### AI 视频生成工作流合集 · 专为 **8GB 显存** 显卡优化

**RTX 4070 · RTX 4060 Ti · RTX 3060 · RTX 2060 Super**

[English](#english) | [中文](#中文)

---

</div>

## 中文

### 谁说你 8GB 显存不能跑 AI 视频？

大部分人以为 AI 视频生成需要 24GB+ 显存。**不是的。**

这个仓库收集了经过实测、能在 **8GB 显存显卡** 上流畅运行的 ComfyUI 视频生成工作流。全部用 **RTX 4070 8GB** 实机验证，每套工作流附带：
- 显存占用实测数据
- 生成速度
- 常见问题

### 包含的工作流

| 模型 | 最低显存 | 8GB 实测 | 最长视频 | 下载 |
|------|---------|---------|---------|------|
| **FramePack** | ~6GB ✅ | 流畅运行 | 60秒+ | [Workflow](workflows/framepack/) |
| **Wan2.1 1.3B** | ~8GB ✅ | 刚好跑满 | 15秒 | [Workflow](workflows/wan2.1/) |
| **AnimateDiff** | ~6GB ✅ | 流畅运行 | 无限(循环) | [Workflow](workflows/animatediff/) |
| **HunyuanVideo** | ~12GB ❌ | 跑不动 | - | 需量化版本 |

### 快速开始

```bash
# 1. 安装 ComfyUI
# 推荐使用 秋葉aaaki 整合包 V9.5+
# 下载: https://pan.quark.cn/s/74ca1db39db7?pwd=Fyw6

# 2. 安装所需节点
cd ComfyUI/custom_nodes
git clone https://github.com/您的节点

# 3. 下载模型（详见各工作流文件夹内的 guide）
```

### 硬件实测环境

| 项目 | 配置 |
|------|------|
| 显卡 | NVIDIA RTX 4070 8GB |
| CPU | Intel i7-12700 |
| 内存 | 16GB DDR4 |
| 系统 | Windows 11 |
| ComfyUI | 秋葉整合包 / 官方版 |
| Python | 3.10 / 3.11 |

### 工作流详细说明

#### FramePack（推荐！6GB显存，最长60秒视频）
FramePack 是当前 **8GB 显卡的最佳选择**。6GB 就能跑，支持生成长达 60 秒的视频。
- 工作流：[framepack-basic.json](workflows/framepack/framepack-basic.json)
- 模型要求：FramePack I2V 模型 (~2GB)
- 8GB 实测：5-8 秒/帧，30秒视频约 3-4 分钟
- [详细指南](guides/framepack-guide.md)

#### Wan2.1 1.3B（满占用，15秒视频）
Wan2.1 1.3B 模型刚好能吃满 8GB 显存，画质优秀。
- 工作流：[wan2.1-basic.json](workflows/wan2.1/wan2.1-basic.json)
- 模型要求：Wan2.1 1.3B (~2.6GB)
- 8GB 实测：10-15 秒/帧，15秒视频约 3-5 分钟

#### AnimateDiff（轻量，循环视频）
适合做 GIF 和短视频循环，也是 8GB 友好的经典方案。
- 工作流：[animatediff-basic.json](workflows/animatediff/animatediff-basic.json)

### 变现思路

- **付费工作流**：复杂的工作流封装成 ¥99 付费包
- **1v1 部署指导**：¥299 远程帮装 ComfyUI + 全部模型
- **B站/小红书**：发实测视频引流
- **定制服务**：按需调整工作流

### 贡献

提交 PR 或 Issue，分享你的 8GB 工作流经验！

### 许可

MIT License - 随便用，标出处就行。

---

## English

### Who says 8GB VRAM can't do AI video?

Most people think AI video generation requires 24GB+ VRAM. **That's wrong.**

This repo collects ComfyUI video generation workflows that are **verified to run on 8GB VRAM GPUs**. All tested on an actual **RTX 4070 8GB**.

### Included Workflows

| Model | Min VRAM | 8GB Performance | Max Length | Download |
|-------|----------|----------------|------------|----------|
| **FramePack** | ~6GB ✅ | Smooth | 60s+ | [Workflow](workflows/framepack/) |
| **Wan2.1 1.3B** | ~8GB ✅ | Fills VRAM | 15s | [Workflow](workflows/wan2.1/) |
| **AnimateDiff** | ~6GB ✅ | Smooth | Loop | [Workflow](workflows/animatediff/) |
| **HunyuanVideo** | ~12GB ❌ | N/A | - | Needs quantized version |

### Quick Start

```bash
# Install ComfyUI (use 秋葉整合包 or official)
# Install required custom nodes
# Download models (see guide in each workflow folder)
```

### Test Bench

| Component | Spec |
|-----------|------|
| GPU | NVIDIA RTX 4070 8GB |
| CPU | Intel i7-12700 |
| RAM | 16GB DDR4 |
| OS | Windows 11 |

### Recommendations

**FramePack is the #1 choice for 8GB cards** — only 6GB VRAM needed, supports up to 60-second videos. Start there.

### License

MIT
