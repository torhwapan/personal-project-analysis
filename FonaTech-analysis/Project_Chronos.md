# Project Chronos

## 项目作用

实验性 MoE 推理与训练框架，目标是在消费级硬件上把专家权重放在 SSD、DRAM 和 VRAM 的混合层级中，并减少 decode 阶段的 IO 等待。

## 技术与结构

- 通过 prefill 阶段的 IntentClassifier/PrefillScheduler 预测热点专家。
- NVMe cluster-packed `.ctsr` 与 manifest 将随机专家读取转为簇级顺序 mmap；Pinned RAM 作为中转，H2D 使用非阻塞流。
- 每个专家使用 CUDA event 同步，LookaheadRouter 预取未来专家；缺失时通过 soft gating 退化到共享专家。
- 同仓库还包含六阶段 SFT、蒸馏、偏好和强化训练脚本及基准工具。

## 使用边界

项目仍标注 Experimental；SSD 延迟、路由预测错误、硬件后端和模型结构会显著影响收益，README 的模拟/基准结果不能视为所有 MoE 的保证。
