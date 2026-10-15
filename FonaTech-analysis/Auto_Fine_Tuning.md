# Auto_Fine_Tuning

## 项目作用

本地单机大模型微调工作台。它可识别 SFT 或偏好数据，将 SFT 样本自动生成 rejected 响应，再运行 DPO/ORPO；同时支持普通 SFT、LoRA、监控、检查点和 Optuna 搜参。

## 技术与结构

- `core/` 实现数据检测、训练流程、后端选择和 MLX 内存优化。
- `ui/` 与 `app.py` 提供 Gradio 界面。
- 根据硬件选择 MLX、Unsloth CUDA、Hugging Face CUDA/MPS/CPU；拒答生成放在独立子进程，降低训练器与 Metal/CUDA 状态冲突。
- MLX 路径用数学等价的 `logits-logsumexp` 和逐步缓存清理降低峰值内存，不修改已安装包源码。

## 使用边界

质量取决于基础模型、数据和硬件；自动生成 rejected 不是人工偏好标注的等价替代，CPU/MPS 主要适合小规模或调试。
