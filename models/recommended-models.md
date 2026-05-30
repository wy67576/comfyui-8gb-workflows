# ComfyUI 8GB — 显卡 & 模型兼容性速查

## 8GB 显卡清单

| 显卡 | 架构 | 显存 | AI视频能力 |
|------|------|------|-----------|
| RTX 4070 | Ada Lovelace | 8GB GDDR6X | ✅ FramePack/Wan2.1/AnimateDiff |
| RTX 4060 Ti 8GB | Ada Lovelace | 8GB GDDR6 | ✅ FramePack/AnimateDiff (Wan2.1 勉强) |
| RTX 3060 | Ampere | 8GB GDDR6 | ✅ FramePack/AnimateDiff |
| RTX 2060 Super | Turing | 8GB GDDR6 | ✅ FramePack (慢) |
| GTX 1080 | Pascal | 8GB GDDR5X | ⚠️ 仅 AnimateDiff (无 FP16 支持) |

## 推荐模型清单

| 模型 | 显存 | 速度(8GB) | 质量 | 推荐度 |
|------|------|-----------|------|--------|
| FramePack I2V | ~6GB | ★★★★ | ★★★★ | ⭐⭐⭐⭐⭐ |
| Wan2.1 1.3B | ~8GB | ★★★ | ★★★★★ | ⭐⭐⭐⭐ |
| AnimateDiff v3 | ~6GB | ★★★★★ | ★★★ | ⭐⭐⭐ |
| Stable Video Diffusion | ~7GB | ★★★ | ★★★ | ⭐⭐⭐ |

## 不推荐（显存不够）

- HunyuanVideo (12GB+)
- CogVideoX (14GB+)
- Wan2.1 14B (20GB+)
- Kling/Hailuo (云端，收费)
