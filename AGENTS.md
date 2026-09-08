# AGENTS.md

Antigravity Tools —— 专业级 AI 账号管理与协议代理系统（Tauri 2 桌面应用）。将 Web 端 Session（Google/Anthropic）转化为标准化 API 接口，提供多账号管理、协议转换（OpenAI ↔ Gemini ↔ Claude）、代理池、配额监控等能力。上游仓库 `lbjlaq/Antigravity-Manager`，本仓库为其 fork（remote：`lzm04521/Antigravity-Manager`）。

## 协作与范围

- 除代码、命令、路径、日志、报错、配置键、API 名称和专有名词外，回复使用简体中文。
- 本项目使用 Git 托管；修改前检查工作区已有未提交或待提交改动，不覆盖用户修改。
- 优先用 CodeGraph 查定义、引用、调用链和符号信息（仓库已建 `.codegraph/` 索引）；不可用时再退回文本搜索与文件读取。

## 变更流程

- 修改代码、配置、发布脚本或新增文件前，先在 `doc/` 新建或更新实施方案，说明目标、范围、步骤、风险和验证方式，`doc/` 忽略代码托管。注意与项目自带 `docs/`（面向用户的项目文档）区分。
- 实施方案需用户确认后再执行。
- doc 命名方式为 `日期-标题.md`，例如 `20260101-A方案.md`。
- 只读分析、问题解答、现有文档评审不需要创建实施文档。
- 新增文件、模块、类、函数或 helper 前，必须先产出 Dedupe Ticket。
- Dedupe Ticket 必须包含：Intent signature、Queries、Top matches、Decision、Rationale。
- 初始化类命令（`/init`、`/init-project` 等）自身携带完整流程，无需另建实施方案；不要把普通开发任务纳入例外。

## 构建与运行

- `npm install`：安装前端依赖。
- `npm run dev`：Vite 前端 dev server（`http://localhost:1420`）。
- `npm run build`：`tsc && vite build`，产出 `dist/`。
- `npm run tauri dev`：Tauri 开发模式（自动起前端 dev server + Rust 后端）。
- `npm run tauri:debug`：`RUST_LOG=debug npm run tauri dev`。
- `npm run tauri build`：打包桌面应用安装包。
- Rust 侧：在 `src-tauri/` 下 `cargo check` / `cargo build` / `cargo test`；构建配置见 `src-tauri/tauri.conf.json`（identifier `com.lbjlaq.antigravity-tools`）。
- Docker 部署与 Web 版见 `docker/`、`deploy/`；文档站见 `web_site/`（VitePress）。

## 测试

- Rust 单元测试：`#[cfg(test)]` 内联于 `src-tauri/src/modules/`（account、oauth、cache、token_stats、user_token_db、update_checker、version 等）、`src-tauri/src/commands/security.rs` 等；集成测试位于 `src-tauri/src/proxy/tests/`。运行：在 `src-tauri/` 下 `cargo test`。
- 前端：`package.json` 无 test 脚本、无测试框架依赖（前端测试方式未确认）。
- CI：`.github/workflows/ci.yml`。

## 宿主进程

单宿主 Tauri 桌面应用：React 前端（dev 时 `localhost:1420`）与 Rust 后端同进程运行；本地 API 代理服务由后端在应用内启动（监听地址由应用配置决定）。

## 高层架构

- `src/`（React 19 + TypeScript + Vite 前端）
  - `pages/`：路由页面 Dashboard、Accounts、ApiProxy、Monitor、TokenStats、UserToken、Security、Settings（路由定义见 `src/App.tsx`）。
  - `components/`：布局与业务组件（layout、settings、security、debug、common 等）。
  - `stores/`：zustand 状态（`useAccountStore`、`useConfigStore`）。
  - `services/`：后端调用封装（如 `accountService.ts`）。
  - `types/`：类型定义（`config.ts` 含 ProxyConfig、ProxyEntry、ProxyPoolConfig 等）。
  - `locales/` + `i18n.ts`：i18next 多语言（含 RTL 支持）。
- `src-tauri/src/`（Rust 后端）
  - `main.rs` → `antigravity_tools_lib::run()`（组装入口在 `lib.rs`）。
  - `commands/`：Tauri command 层（autostart、patch、proxy、proxy_pool、security、user_token 等）。
  - `modules/`：业务模块（account、oauth、cache、token_stats、user_token_db、update_checker、version 等）。
  - `proxy/`：协议转换核心（OpenAI ↔ Gemini 映射、Claude 流式 mapper 等）。
  - `models/`、`utils/`、`error.rs`、`constants.rs`。
- 前后端通信：前端经 `src/utils/request.ts` 的 `request`（Tauri invoke 封装）调用 `#[tauri::command]`；事件经 `@tauri-apps/api/event` 的 `listen`（如 `tray://account-switched`、`accounts://refreshed`）。

## 核心框架类型

- Tauri 2：`#[tauri::command]` 后端命令 + 前端 `invoke`（经 `src/utils/request.ts`）。
- React 19 + react-router 7 + zustand 5 + antd 5（@lobehub/ui）+ Tailwind 3 + i18next。
- Rust：tokio 异步运行时、reqwest HTTP 客户端。
- CI/发布：GitHub Actions（ci.yml、deploy-pages.yml、release.yml：push `v*` tag 触发 Windows 构建并创建 GitHub Release）、`@tauri-apps/plugin-updater`。

## 配置与集成

- 构建配置：`src-tauri/tauri.conf.json`、`src-tauri/Cargo.toml`、`vite.config.ts`、`tailwind.config.js`、`tsconfig.json`。
- 运行时配置：前端 `useConfigStore` 加载语言、代理等设置；本地数据存于应用数据目录。
- 版本发布：版本号同步维护于 `package.json` 与 `src-tauri/tauri.conf.json`（当前 4.6.9-local1）；推送 `v*` tag 触发 GitHub Actions 自动构建 Windows 安装包并创建 GitHub Release；变更记录见 `CHANGELOG.md` / `CHANGELOG_EN.md`。

## 上游合并策略

- 上游 `lbjlaq/Antigravity-Manager` 发版时逐 commit 审查，只合并本地保留功能相关的改动。
- 已删功能相关的上游改动一律跳过：中转站（ApiKeyFun）、CLI 配置同步（cli_sync/droid_sync/opencode_sync）、z.ai(GLM) 提供商与 MCP 服务、Cloudflared。
- 合并时注意与本地瘦身改动的冲突，防止已删功能被上游变更重新引入。

## 版本控制

- 本项目使用 Git 托管；修改前运行 `git status --short`。
- 保护用户未提交修改，不自动提交、推送、重置、清理或切换分支。
- remote：`origin → https://github.com/lzm04521/Antigravity-Manager.git`；上游为 `lbjlaq/Antigravity-Manager`，跟进上游时注意合并历史与本地定制差异。
