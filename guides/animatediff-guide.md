# AnimateDiff 8GB 工作流 — 搭建指南

## 前提

**已安装节点：** `ComfyUI-AnimateDiff-Evolved` ✅
**已安装模型：**
- `mm_sd_v15_v2_fp16.safetensors` — AnimateDiff v2 运动模块
- `sd15_animatediff_lightning_4step.safetensors` — Lightning 加速模块

**推荐底模：** `DreamShaper_8_pruned.safetensors`（SD1.5，~2GB）

## 搭建步骤

### 基础工作流

```
[Checkpoint Loader] → [AnimateDiffLoader] → [KSampler] → [VAEDecode] → [Video Output]
                      (DreamShaper)          (motion module)  (AnimateDiff)
```

### 节点连接

1. **CheckpointLoaderSimple** → 加载 DreamShaper 等 SD1.5 底模
2. **AnimateDiffLoaderV1Legacy** → 选择运动模块 `mm_sd_v15_v2_fp16`
3. **AnimateDiffSampler** → 替代普通 KSampler
4. **VAEDecode** → 解码到图像
5. **VideoCombine** (VideoHelperSuite) → 合成视频

### 8GB 优化参数

| 参数 | 推荐值 | 说明 |
|------|--------|------|
| 运动模块 | mm_sd_v15_v2 | v2 比 v1 省显存 |
| 上下文长度 | 8-12帧 | 每批处理帧数 |
| 步数 | 20-25 | 越多越精细 |
| 分辨率 | 512×512 | SD1.5 标准 |
| 总帧数 | 16-48 | 约1-2秒@24fps |
| CFG Scale | 7.0 | AnimateDiff 推荐 |

### 不同场景配置

**场景1：短视频（GIF，1秒）**
- 总帧数：16
- 上下文长度：16
- 步数：20
- 时间：~1分钟

**场景2：中等长度（2秒）**
- 总帧数：48
- 上下文长度：12
- 步数：25
- 时间：~3分钟

**场景3：长循环（Loop）**
- 总帧数：48（循环）
- 上下文长度：16
- 步数：20
- Loop 模式：A-B-A 循环
