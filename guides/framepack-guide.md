# ComfyUI 8GB Workflows — FramePack 详细指南

## 简介

FramePack 是当前 **8GB 显卡的最佳 AI 视频方案**。最低仅需 ~6GB 显存，支持生成长达 **60 秒+** 的视频。

## 模型下载

1. 从 HuggingFace 下载 FramePack I2V 模型：
   ```
   https://huggingface.co/FramePack/
   ```
2. 放入 ComfyUI 的 `models/checkpoints/` 目录
3. 共计约 2GB

## 工作流使用

1. 打开 ComfyUI
2. 拖入 `workflows/framepack/framepack-basic.json`
3. 在 Load Image 节点选择你的起始图
4. 在 Prompt 节点输入描述文本
5. 设置帧数（1-60秒，按30fps计算）
6. 点击 Queue Prompt

## 8GB 参数推荐

| 参数 | 推荐值 |
|------|--------|
| 分辨率 | 512×512 或 512×768 |
| 帧率 | 16-24 fps（减少显存压力） |
| 最大帧数 | 480-900（16-30秒） |
| 步数 | 20-30 |
| CFG Scale | 3.0-5.0 |

## 常见问题

**Q: 显存不足怎么办？**
- 降低分辨率到 384×384
- 减少帧数
- 使用 --lowvram 模式启动 ComfyUI

**Q: 生成速度太慢？**
- 减小步数到 15-20
- 使用 512×512 而非更高分辨率
- 启用 xFormers 优化
