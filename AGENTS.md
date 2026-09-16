# Prototype Instructions

Run the local server yourself and open the preview in the browser available to this environment. Do not give the user server-start instructions when you can run it.

Before making substantial visual changes, use the Product Design plugin's `get-context` skill when the visual source is unclear or no longer matches the current goal. When the user gives durable prototype-specific design feedback, preferences, or decisions, record them in `AGENTS.md`.

When implementing from a selected generated mock, treat that image as the source of truth for layout, component anatomy, density, spacing, color, typography, visible content, and hierarchy.

Build app UI in `src/`. Keep `.openai/hosting.json`, `worker/index.js`, `scripts/prepare-sites-build.mjs`, and `tests/sites-worker.test.mjs` intact so the same local prototype can be handed to Sites. Before a Sites handoff, run `npm run build` and `npm run test:sites`; the build must leave `dist/client/index.html`, `dist/server/index.js`, and `dist/.openai/hosting.json`.

## 已确认的设计决策

以下为评审后确认并已落地的约定，后续改动请沿用，不要重新引入散值。

**调性定位**：站点是「克制的企业服务站」，不是「科技公司站」。高级感来自留白、字号层级与单色克制；不要靠堆渐变、发光、粒子来制造科技感。

**设计令牌**（定义在 `src/styles.css` 的 `:root`，所有新样式必须走令牌）：

| 令牌 | 值 | 用途 |
|---|---|---|
| `--r-xs` / `--r-sm` / `--r-md` / `--r-lg` / `--r-pill` | 6 / 8 / 12 / 16 / 999px | 圆角阶梯：仪表盘内部件 / 按钮与输入框 / 卡片与面板 / 大区块与弹窗 / 胶囊按钮 |
| `--elev-1` / `--elev-2` / `--elev-3` | 三段 `box-shadow` | 静置卡片 / 悬浮态与面板 / 弹窗。均含 `inset 0 1px 0` 内描边高光 |
| `--ease` | `cubic-bezier(.22,.61,.36,1)` | 统一缓动 |

- **禁止再写死圆角值**（`50%` 的圆形关闭按钮除外）。
- 阴影一律用 `var(--elev-*)`，不要回到 `#183e7407` 这类 3%~6% 不透明度的不可见阴影。

**动效**：滚动入场由 `App.jsx` 的 `useReveal()` 通过 IntersectionObserver 下发 `.reveal` / `.is-in`，样式整块包在 `@media(prefers-reduced-motion:no-preference)` 内；动画结束后 JS 会摘掉标记与内联 `transition-delay`，避免拖慢 hover 反馈。指标数字滚动见 `Metric` 组件，同样受 reduced-motion 保护。**新增动效必须沿用这个模式**，不得让内容在无 JS 或减弱动效时不可见。

**演示区（Dashboard）可读性**：内部文案不得低于 9px，主指标 `strong` 为 24px（弹窗放大版 29px）。调整字号后必须复核容器高度——`.dashboard` 为 444px（移动端 358px）、`.expanded` 为 532px（移动端 440px），溢出会被 `overflow:hidden` 静默裁切。核对方法：比较容器 `scrollHeight` 与 `clientHeight`，差值应为 0。

**首屏标语**：`.hero-caption`（以连接，创造价值）在移动端显示（`@media(max-width:760px)` 内重新定位为居中），**桌面端隐藏**——它会与右侧玻璃质感主视觉重叠导致文字不可读。
