# MetalVASP

## 项目作用

为 Apple Silicon 上用户自有授权 VASP 提供 Metal GPU 加速适配。项目包含双单精度内核、受限工作区、波函数基底复用和非局域力/应力投影，并保留 CPU fallback。

## 技术与结构

- `Sources/`、`Fortran/` 是后端与接口；`adapters/` 用源码指纹约束 VASP 版本修改。
- `Scripts/metalvasp.py` 与 `setup.sh` 负责诊断、规划、隔离构建和运行统计；`Tests/`、`benchmarks/` 提供数值与性能验证。
- 适配重点为 VASP 6.4.1，代码不包含 VASP 源码、赝势或可执行文件。

## 使用边界

需要 arm64 Mac 和本地合法授权的 VASP/工具链。README 中的加速数据是特定测试集结果，不能直接推断所有体系的普适加速。
