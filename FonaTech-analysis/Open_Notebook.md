# Open_Notebook

## 项目作用

基于 SenseNova 的本地优先可视化内容工作区。它从用户请求和上传资料生成整页 PPT、海报、研究图，也支持图像编辑，并将结果组合为 PPTX/PDF。

## 技术与结构

- Python/FastAPI 后端位于 `backend/`，SQLite 保存会话、上传、任务、事件和工件。
- `frontend/` 提供 Web UI；已构建前端时可由 `start.py` 直接服务。
- 有 Auto/PPT/Poster/Research Figure/Image Edit 工作流、LLM 配置兼容层、本地 fake-image 冒烟模式和工件目录。

## 使用边界

真实生成依赖 SenseNova API 与密钥；生成图像是完整画布输出，不等同于可编辑的原生幻灯片矢量排版。
