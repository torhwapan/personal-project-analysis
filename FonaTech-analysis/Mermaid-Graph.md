# Mermaid-Graph

## 项目作用

浏览器端 Mermaid 图表工作台，提供代码编辑、实时预览、流程图可视化编辑、自动布局、本地项目保存和导出。

## 技术与结构

React 19 + TypeScript + Vite；Mermaid 负责渲染，Monaco 提供编辑器，React Flow 与 ELK 支持节点编辑和布局，Dexie/IndexedDB 保存本地项目，可导出 MMD、SVG、PNG。

## 使用边界

重点是本地浏览器交互工具，项目数据默认保存在浏览器端，不是协作式云端图数据库。
