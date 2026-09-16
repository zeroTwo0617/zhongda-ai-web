# 项目长期记忆 — 众达AI 公司官网原型

## 定位
`D:\通用框架\公司网站\web` 是**公司官网原型**（营销单页站），同时遵守 `AGENTS.md` 里的 Sites 交接约定：`src/` 写业务 UI；`.openai/hosting.json`、`worker/index.js`、`scripts/prepare-sites-build.mjs`、`tests/sites-worker.test.mjs` **不得改动**；交接前必须 `npm run build` + `npm run test:sites`，产物需含 `dist/client/index.html`、`dist/server/index.js`、`dist/.openai/hosting.json`。

## 技术栈与约定
- Vite 6.4 + React 19.2 + `@phosphor-icons/react`（图标按名按需引入）+ `recharts`（仅 Dashboard 用面积图）。
- 全站集中在 `src/App.jsx` 单文件 + `src/styles.css` 单文件。**CSS 是压缩成 4 行长行的风格**，改样式前要先用脚本按 `}` 拆规则或计算大括号深度定位，别直接肉眼改。
- 三个响应式断点：`max-width:1100px`（产品网格 6→3 列、图标转绝对定位）、`max-width:760px`（导航折叠为汉堡、hero 变 730px 双段式、产品网格 2 列）、`min-width:1450px`（hero 图改 `object-fit:contain`）。
- 视觉语言：主蓝 `--blue:#1767ff`；浅冰蓝底 `--ice:#f4f8ff`；正文字色 `#12284c`。品牌三理念「众人连接 · 通达服务 · 智能应用」，英文口号 CONNECT PEOPLE. ENABLE INTELLIGENCE.
- 交互组件：统一用原生 `<dialog>` 包一层 `Modal`（showModal + 焦点归还 + body 滚动锁 + 遮罩点击关闭）。联系表单**只在前端生成可下载 txt，不上传**——原型阶段不要擅自接后端。
- 行业照片方案：`reference.png` 是 6688×3764 的**精灵图**，靠 `.photo-0~3` 的 `background-position` 裁切。优化时改成分图要同步改这 4 条规则。

## 反复踩坑 / 环境注意
- **沙箱内给本地页面临时截图**：`agent-browser` 未安装；nuphus MCP 浏览器工具被 SSRF 守卫拒绝访问 127.0.0.1。可行方案见用户级技能 `local-page-screenshot`（chrome-headless-shell + 原生 CDP）。
- 截图输出目录用 ASCII 临时路径（如 `C:\Users\86151\AppData\Local\Temp\...`），因为 Chrome 对含中文的工作区路径处理不稳。
- 本地 dev server 端口 5173（`npm run dev -- --port 5173 --strictPort`，vite.config 已开 `host: 0.0.0.0`）。

## 仓库与协作
- **远程**：https://github.com/zeroTwo0617/zhongda-ai-web （PRIVATE）。
- `main` 是基线，`feat/visual-upgrade` 是设计令牌与动效改造分支（已推送，领先 main 3 个提交）。
- **沙箱里 `git push` 永远不通**：`github.com` 被网络策略拦截（CONNECT 502），只有 `api.github.com` 可通。推送一律走技能 `github-push-via-api`（Git Data API 桥，SHA 与本地一致，不会分叉）。本地也 `fetch` 不到远程，分支名是唯一真相来源——**不要在 GitHub 网页上直接改文件**，否则 SHA 对不上后续推送会被拒。
- `git init` 后若报 `detected dubious ownership`（目录属主是沙箱用户），已加过 `safe.directory` 豁免；换新项目路径需重新加一条。
- `dist/` 与 `node_modules/` 已 gitignore；`.workbuddy/memory` 与 `AGENTS.md` 入库（项目数据，非临时缓存）。
- `.gitattributes` 强制 `eol=lf`，避免 Windows 下 CRLF 反复产生噪音 diff。
