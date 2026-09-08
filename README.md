# Antigravity Tools

<div align="center">
  <img src="public/icon.png" alt="Antigravity Logo" width="120" height="120" style="border-radius: 24px; box-shadow: 0 10px 30px rgba(0,0,0,0.15);">

  <p><strong>专业级 AI 账号管理与协议代理系统（Tauri 2 桌面应用）</strong></p>
  <p>将 Web 端 Session（Google / Anthropic）转化为标准化 API 接口</p>

  <p>
    <a href="https://github.com/lzm04521/Antigravity-Manager/releases">
      <img src="https://img.shields.io/badge/Version-4.7.0-local1-blue?style=flat-square" alt="Version">
    </a>
    <img src="https://img.shields.io/badge/Tauri-v2-orange?style=flat-square" alt="Tauri">
    <img src="https://img.shields.io/badge/Backend-Rust-red?style=flat-square" alt="Rust">
    <img src="https://img.shields.io/badge/Frontend-React-61DAFB?style=flat-square" alt="React">
    <img src="https://img.shields.io/badge/License-CC--BY--NC--SA--4.0-lightgrey?style=flat-square" alt="License">
  </p>

  <p>
    <strong>简体中文</strong> |
    <a href="./README_EN.md">English</a>
  </p>
</div>

---

**Antigravity Tools** 是一个 Tauri 2 桌面应用：React 19 + TypeScript 前端与 Rust 后端同进程运行。它把常见的 Web 端 Session（Google / Anthropic 的 OAuth 授权）转化为本地标准化 API，消除不同厂商间的协议鸿沟，本地 API 代理服务由后端在应用内启动。

## 功能特性

- **多账号管理**：Google / Anthropic 账号统一管理，支持列表 / 网格视图、单条 Token 录入与 JSON 批量导入、403 封禁检测与自动标注。
- **OAuth 登录**：OAuth 2.0 授权流程，授权链接预生成可复制，任意浏览器完成授权后自动回存账号。
- **API 代理**：本地代理服务提供 OpenAI `/v1/chat/completions`、Anthropic `/v1/messages` 与 Gemini 原生端点，OpenAI ↔ Gemini ↔ Claude 协议自动转换；请求遇到 429 / 401 时自动重试并静默轮换账号。
- **代理池**：内置代理池配置，为上游请求统一分配 HTTP 代理。
- **配额监控**：仪表盘实时监控各账号配额与健康状态，按配额冗余度推荐最佳账号并支持一键切换。
- **Token 统计**：按账号、模型、时间维度统计 Token 用量与请求情况。
- **安全设置**：API Key、管理密码等安全项的集中配置。

## 快速开始

环境要求：

- Node.js ≥ 20
- Rust stable 工具链（Windows 下为 MSVC 目标，另需满足 [Tauri 2 系统依赖](https://tauri.app/start/prerequisites/)）

```bash
npm install
npm run tauri dev
```

开发模式下前端 dev server 运行在 `http://localhost:1420`，Rust 后端随之启动。

## 构建打包

```bash
npm run tauri build
```

产出桌面应用安装包，构建配置见 `src-tauri/tauri.conf.json`。

## 发布

推送 `v*` 格式的 tag 触发 GitHub Actions 自动构建 Windows 安装包并创建 GitHub Release：

```bash
git tag v4.7.0-local1
git push origin v4.7.0-local1
```

发布产物见 [Releases](https://github.com/lzm04521/Antigravity-Manager/releases)。

## 关于本仓库

本仓库 fork 自 [lbjlaq/Antigravity-Manager](https://github.com/lbjlaq/Antigravity-Manager)，并在其基础上做了功能瘦身。跟随上游更新时，只合并本仓库保留功能相关的改动，已移除功能的相关变更跳过。

## License

沿用上游仓库 [lbjlaq/Antigravity-Manager](https://github.com/lbjlaq/Antigravity-Manager) 的 **CC BY-NC-SA 4.0**（Attribution-NonCommercial-ShareAlike 4.0 International）许可，严禁任何形式的商业行为。

所有账号数据加密存储于本地 SQLite 数据库，不经配置不会离开你的设备。
