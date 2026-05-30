# Wan2.1 T2V 8GB 工作流 — 搭建指南

## 前提

Windows ComfyUI 已安装以下节点和模型：

**已安装节点：**
- `ComfyUI-WanVideoWrapper` ✅ — Wan2.1/2.2 视频生成核心
- `ComfyUI-VideoHelperSuite` ✅ — 视频工具

**已安装模型（8GB 友好）：**
- `wan2.1-t2v-1.3b-Q3_K_M.gguf` — 文本→视频，1.3B 参数量(Q3量化)，~1GB
- `wan2.1-t2v-1.3b-Q3_K_S.gguf` — 同上，更小量化版本
- `wan2.1-i2v-14b-480p-Q3_K_M.gguf` — 图像→视频，14B 量化版
- `Wan2.2-TI2V-5B-Q4_K_M.gguf` — Wan2.2 图生视频，5B

## 搭建步骤

### 1. 基础 T2V 工作流

```
[WanVideoModelConfig] → [WanVideoTextEncode] → [WanVideoSampler] → [VAEDecode] → [Video Output]
                                           ↑
                                    [CLIP Loader]
```

**节点参数（8GB 优化）：**

| 节点 | 关键参数 | 值 |
|------|---------|-----|
| WanVideoModelConfig | model | `wan2.1-t2v-1.3b-Q3_K_M.gguf` |
| WanVideoModelConfig | model_type | `t2v-1.3B` |
| WanVideoModelConfig | t5vram | `cpu`（关键！省显存） |
| WanVideoBlockSwap | ... | 启用 block swap |
| WanVideoTextEncode | prompt | 你的提示词 |
| WanVideoSampler | width | 512 |
| WanVideoSampler | height | 512 |
| WanVideoSampler | num_frames | 41 (约1.4秒@30fps) |
| WanVideoSampler | steps | 20 |
| WanVideoSampler | cfg | 5.0 |
| CLIP Loader | clip_name | `umt5_xxl_fp8_e4m3fn_scaled.safetensors` |
| VAE Loader | vae_name | `wan_2.1_vae.safetensors` |

### 2. 8GB 显存优化设置

**关键技巧：T5 文本编码器放 CPU**
WanVideoModelConfig 的 `t5vram` 设为 `cpu`——T5 编码器本身占 ~5GB，放 CPU 能省下大量显存给生成用。

**Block Swap（块交换）**
启用 WanVideoBlockSwap，把部分 transformer 层 swap 到 CPU，生成速度慢一些但显存占用从 10GB+ 降到 7GB 左右。

**分辨率与帧数权衡（8GB）**

| 场景 | 分辨率 | 帧数 | 显存 | 生成时间(4070) |
|------|--------|------|------|--------------|
| 快速测试 | 384×384 | 17帧 | ~5GB | ~30秒 |
| 标准质量 | 512×512 | 41帧 | ~7GB | ~2分钟 |
| 高质量 | 512×768 | 25帧 | ~8GB | ~3分钟 |
| 最高 | 512×512 | 81帧 | ~8.5GB(溢出) | ~5分钟 |

### 3. Wan2.2 I2V 工作流

Wan2.2-TI2V-5B 是图生视频模型，需要一张起始图。

```
[LoadImage] → [WanVideoModelConfig] → [WanVideoSampler] → [VAEDecode] → [Video Output]
                                  ↑                        ↑
                           [CLIP Loader]            [Image Latent]
```

| 参数 | 值 |
|------|-----|
| model | `Wan2.2-TI2V-5B-Q4_K_M.gguf` |
| model_type | `i2v-5B` |
| 起始图 | 任意图片（会按比例缩放） |
| num_frames | 41-81 |
| steps | 20 |
