# E3-miu-GNN

## 项目作用

研究型原子机器学习平台，用 E(3)/O(3) 等变图网络学习能量面，并联合预测电、磁和电子响应。支持 GUI、命令行、ASE、无界面 Python 和 LLM 工作流。

## 技术与结构

- `models/`、`e3mu/` 实现局部标量/向量/高阶张量表示与混合粒度反馈。
- 可微 QEq、Ewald/PME、Thole 极化、DFT-D4，以及 Heisenberg、单离子各向异性和 DMI 等响应层。
- `datasets/`、`workflow/`、`analysis/` 和 PyQt6 界面覆盖数据准备、训练、评估和可视化。

## 使用边界

项目声明为 source-available research platform；报告的短跑指标是功能验证，不代表已收敛或普适的通用势能面。
