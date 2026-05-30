
---
title: RTX 4070 8GB 跑 AI 视频生成：FramePack/Wan2.1/AnimateDiff 实战配置
tags: [AI视频, ComfyUI, 8GB显卡, RTX4070, FramePack, Wan2.1, AnimateDiff]
categories: [AI教程]
keywords: 8GB显存AI视频, ComfyUI视频生成, RTX4070视频生成, FramePack教程
description: 谁说8GB显卡不能跑AI视频？本文实测RTX 4070 8GB运行FramePack、Wan2.1、AnimateDiff三种方案，含完整工作流和显存优化技巧。
---

# RTX 4070 8GB 跑 AI 视频生成：FramePack/Wan2.1/AnimateDiff 实战配置

## 前言

大部分人以为 AI 视频生成需要 24GB+ 显存。**不是的。**

我这台 RTX 4070 8GB 的机器，实测能跑三种主流视频生成方案。本文给 8GB 显卡用户一份完整的配置指南。

先放结论：

| 模型 | 最低显存 | 8GB 实测 | 最长视频 |
|------|---------|---------|---------|
| **FramePack** | ~6GB ✅ | 流畅运行 | 60秒+ |
| **Wan2.1 1.3B** | ~8GB ✅ | 刚好跑满 | 15秒 |
| **AnimateDiff** | ~6GB ✅ | 流畅运行 | 无限(循环) |
| 其他(HunyuanVideo等) | ~12GB ❌ | 跑不动 | - |

## 硬件环境

- 显卡：NVIDIA RTX 4070 8GB
- CPU：Intel i7-12700
- 内存：16GB DDR4
- 系统：Windows 11
- ComfyUI：秋葉整合包 / 官方版均可

## 方案一：FramePack（推荐！6GB显存，最长60秒视频）

FramePack 是当前 **8GB 显卡的最佳选择**。最低 6GB 就能跑，支持生成长达 60 秒的视频。

### 安装步骤

1. 安装 ComfyUI（推荐秋葉整合包 V9.5+）
2. 安装 FramePack 自定义节点
3. 下载 FramePack I2V 模型（约 2GB）放到 `models/checkpoints/`

### 8GB 参数推荐

| 参数 | 推荐值 |
|------|--------|
| 分辨率 | 512×512 或 512×768 |
| 帧率 | 16-24 fps |
| 最大帧数 | 480-900（16-30秒） |
| 步数 | 20-30 |
| CFG Scale | 3.0-5.0 |

### 生成速度实测（RTX 4070）

30 秒视频约 3-4 分钟。如果显存吃紧，降低分辨率到 384×384 或使用 `--lowvram` 模式。

## 方案二：Wan2.1 1.3B（满占用，画质最佳）

Wan2.1 1.3B 能用满 8GB 显存，画质是三款中最好的。建议使用 GGUF 量化版本进一步优化显存占用。

### 节点安装

推荐安装 `ComfyUI-WanVideoWrapper` 自定义节点，它打包了 Wan2.1 全系列模型支持。

### 关键优化：T5 编码器放 CPU

在 WanVideoModelConfig 节点中，把 `t5vram` 设为 `cpu`。T5 文本编码器本身占 ~5GB，放 CPU 能省下大量显存给生成用。

### 8GB 参数推荐

| 场景 | 分辨率 | 帧数 | 显存 | 生成时间 |
|------|--------|------|------|---------|
| 快速测试 | 384×384 | 17帧 | ~5GB | ~30秒 |
| 标准质量 | 512×512 | 41帧 | ~7GB | ~2分钟 |
| 高质量 | 512×768 | 25帧 | ~8GB | ~3分钟 |

### 模型选择

推荐使用 GGUF 量化模型，8GB 友好：
- `wan2.1-t2v-1.3b-Q3_K_M.gguf` — 文本→视频，~1GB
- `Wan2.2-TI2V-5B-Q4_K_M.gguf` — 图生视频，5B 量化版

## 方案三：AnimateDiff（轻量级，适合 GIF 和循环视频）

AnimateDiff 是最成熟的方案，适合做短视频和动态封面。

### 安装

1. 安装 `ComfyUI-AnimateDiff-Evolved` 自定义节点
2. 下载运动模块（mm_sd_v15_v2，约 200MB）
3. 配合 SD1.5 底模使用（推荐 DreamShaper 或 Realistic Vision）

### 参数推荐

| 参数 | 推荐值 |
|------|--------|
| 运动模块 | mm_sd_v15_v2 |
| 上下文长度 | 8-12帧 |
| 步数 | 20-25 |
| 分辨率 | 512×512 |
| 总帧数 | 16-48（约1-2秒） |

## 通用显存优化技巧

### 1. 启用 block swap（块交换）

对于 Wan2.1 等大模型，启用 block swap 可以把部分 transformer 层交换到 CPU，显存占用从 10GB+ 降到 7GB 左右。速度慢一些但能跑。

### 2. 使用 --lowvram 模式

启动 ComfyUI 时加参数：
```bash
python main.py --lowvram --windows-standalone-build
```

### 3. 量化模型优先

优先选择 GGUF 或 fp8 量化版本的模型，对 8GB 用户更友好。

### 4. 降低分辨率

不要贪高分辨率。512×512 在视频生成中已经够用。需要高清可以用后处理 upscale。

## 工作流下载

所有工作流已开源，包含在本人的 GitHub 仓库中，欢迎 Star ⭐：

👉 **https://github.com/wy67576/comfyui-8gb-workflows**

仓库包含：
- 完整的 ComfyUI 工作流 JSON
- 每个方案的详细参数指南
- 持续更新

## 结语

8GB 显卡用户的 AI 视频时代已经来了。FramePack 的 6GB 门槛和 60 秒视频支持让它成为首选；Wan2.1 提供最佳画质；AnimateDiff 适合轻量任务。

选哪个？**FramePack 优先**，次选 Wan2.1。

有问题欢迎留言讨论。后续会更新更多 8GB 实战内容。

---

*本文同步发表于 GitHub，工作流持续更新中。*
