# DFT_Skills

## 项目作用

面向 Clouds_Coder、Codex、Claude Code 和 OpenCode 的跨平台材料计算技能包。核心 `dft-workflow-orchestrator` 根据科学问题选择最小足够尺度，并按需扩展到 DFT、AIMD、验证过的 MLIP-MD、统计约化和 FEM。

## 技术与结构

仓库以 `skills/` 为入口，配套 references、case studies、presets 和 helper scripts，并提供各客户端镜像、安装说明和校验工具。重点是检索/证据、工作流决策、输入生成、结果审查和可复现项目组织，而不是一个新的 DFT 求解器。

## 使用边界

技能只能编排和约束计算，不能替代具体软件、算力、物理模型选择或研究者对收敛性和实验意义的判断。
