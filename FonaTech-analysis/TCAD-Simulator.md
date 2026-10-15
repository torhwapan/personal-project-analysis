# TCAD-Simulator

## 项目作用

面向半导体制造流程的 TCAD-like 教学/研究仿真器。它集成工艺 recipe、体素晶圆模型、光刻、沉积、刻蚀、CMP、离子注入、退火、氧化/氮化、测量、可视化和导出。

## 技术与结构

`tcad_simulator.py` 是权威单文件入口；`ProcessModel` 使用 NX×NY×NZ 材料网格、height map、掩膜、掺杂/缺陷场和快照。PyQt5 桌面 GUI、浏览器 WebUI、HTTP API、headless CLI、Admin/会话隔离和可选 LLM recipe 助手共用控制层。

## 使用边界

这是 process-focused TCAD-like 模型，不是商业 TCAD 的完整物理求解器；精度取决于离散化、参数和所选近似，结果需要与实验或专业工具交叉验证。
