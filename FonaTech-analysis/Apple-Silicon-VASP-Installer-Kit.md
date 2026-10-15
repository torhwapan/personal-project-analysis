# Apple-Silicon-VASP-Installer-Kit

## 项目作用

为 Apple Silicon macOS 准备 VASP 构建环境的公开安全安装器。它负责芯片/工具链识别、资源缓存、路径规范化、VASP/VASPKIT 包装器生成和 DFT+U、D3(BJ) 模板。

## 技术与结构

- 核心入口位于 `installer/`，Shell 自动化配合模板、补丁和资源导入文档。
- 支持 FFTW、ScaLAPACK 等公开依赖的缓存或下载。
- 许可证边界清晰：仓库不包含 VASP 源码、PAW/POTCAR 数据或受许可的第三方归档。

## 使用边界

用户必须自行提供已购买的 VASP 和 PAW 文件。它是安装/封装工作流，不是 VASP 本身，也不授予任何商业软件使用权。
