# 众达AI 官网原型

「众人连接 · 通达服务 · 智能应用」——公司官网单页营销站原型，同时可作为 Sites 托管交付物。

## 技术栈

| 层 | 选型 |
|---|---|
| 构建 | Vite 6.4 |
| 框架 | React 19.2 |
| 图标 | `@phosphor-icons/react`（按名按需引入） |
| 图表 | `recharts`（仅演示区使用） |
| 样式 | 单一 `src/styles.css`，CSS 变量 + 三个响应式断点 |

## 本地开发

```bash
npm install
npm run dev          # http://localhost:5173
```

## 构建与交付校验

```bash
npm run build        # 产出 dist/client/index.html、dist/server/index.js、dist/.openai/hosting.json
npm run test:sites   # Sites 打包契约测试，必须全绿
```

`dist/` 是构建产物，不入库；Sites 交接前本地重新构建即可。

## 目录结构

```
src/App.jsx                 全站单文件组件（导航 / 首屏 / 能力 / 解决方案 / 关于 / 弹窗）
src/styles.css              全站样式（设计令牌 + 响应式）
public/assets/              图片资源（hero 主视觉、logo、行业图）
worker/index.js             Sites 边缘 Worker（SPA 回退）
scripts/prepare-sites-build.mjs   Sites 打包准备
tests/sites-worker.test.mjs       Sites 契约测试
.openai/hosting.json        托管声明（勿改）
```

## 设计约定

- **品牌色**：主蓝 `#1767ff`，浅冰蓝底 `#f4f8ff`，正文 `#12284c`。
- **响应式断点**：`max-width:1100px`（平板）、`max-width:760px`（手机）、`min-width:1450px`（宽屏首屏图改为 contain）。

## 修改须知

`AGENTS.md` 记录了原型的协作约定：`.openai/hosting.json`、`worker/index.js`、
`scripts/prepare-sites-build.mjs`、`tests/sites-worker.test.mjs` 需保持完整，
业务 UI 一律写在 `src/` 下。
