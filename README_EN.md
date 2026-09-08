# Antigravity Tools

<div align="center">
  <img src="public/icon.png" alt="Antigravity Logo" width="120" height="120" style="border-radius: 24px; box-shadow: 0 10px 30px rgba(0,0,0,0.15);">

  <p><strong>Professional AI Account Management &amp; Protocol Proxy System (Tauri 2 Desktop App)</strong></p>
  <p>Turns Web Sessions (Google / Anthropic) into standardized API interfaces</p>

  <p>
    <a href="https://github.com/lzm04521/Antigravity-Manager/releases">
      <img src="https://img.shields.io/badge/Version-4.6.9-local1-blue?style=flat-square" alt="Version">
    </a>
    <img src="https://img.shields.io/badge/Tauri-v2-orange?style=flat-square" alt="Tauri">
    <img src="https://img.shields.io/badge/Backend-Rust-red?style=flat-square" alt="Rust">
    <img src="https://img.shields.io/badge/Frontend-React-61DAFB?style=flat-square" alt="React">
    <img src="https://img.shields.io/badge/License-CC--BY--NC--SA--4.0-lightgrey?style=flat-square" alt="License">
  </p>

  <p>
    <a href="./README.md">简体中文</a> |
    <strong>English</strong>
  </p>
</div>

---

**Antigravity Tools** is a Tauri 2 desktop app: a React 19 + TypeScript frontend and a Rust backend run in the same process. It transforms common Web Sessions (OAuth authorization for Google / Anthropic) into a local standardized API, eliminating the protocol gap between providers. The local API proxy service is started by the backend inside the app.

## Features

- **Multi-Account Management**: Unified management of Google / Anthropic accounts with list / grid views, single token entry, JSON batch import, and 403 ban detection & labeling.
- **OAuth Login**: OAuth 2.0 flow with a pre-generated copyable authorization URL; complete auth in any browser and the account is saved automatically.
- **API Proxy**: Local proxy service exposing OpenAI `/v1/chat/completions`, Anthropic `/v1/messages`, and native Gemini endpoints with automatic OpenAI ↔ Gemini ↔ Claude protocol conversion; 429 / 401 responses trigger automatic retry and silent account rotation.
- **Proxy Pool**: Built-in proxy pool configuration to assign HTTP proxies for upstream requests.
- **Quota Monitoring**: Dashboard with real-time quota and health monitoring across accounts, best-account recommendation, and one-click switching.
- **Token Stats**: Token usage and request statistics by account, model, and time.
- **Security Settings**: Central configuration for API keys, admin password, and other security options.

## Getting Started

Requirements:

- Node.js ≥ 20
- Rust stable toolchain (MSVC target on Windows, plus [Tauri 2 prerequisites](https://tauri.app/start/prerequisites/))

```bash
npm install
npm run tauri dev
```

In dev mode the frontend dev server runs at `http://localhost:1420` and the Rust backend starts alongside it.

## Build

```bash
npm run tauri build
```

Produces the desktop installer. Build configuration lives in `src-tauri/tauri.conf.json`.

## Release

Push a `v*` tag to trigger GitHub Actions, which builds the Windows installer and creates a GitHub Release:

```bash
git tag v4.6.9-local1
git push origin v4.6.9-local1
```

See [Releases](https://github.com/lzm04521/Antigravity-Manager/releases) for artifacts.

## About This Repository

This repository is forked from [lbjlaq/Antigravity-Manager](https://github.com/lbjlaq/Antigravity-Manager) and has been slimmed down. When following upstream updates, only merge changes related to the features retained in this repository and skip changes to removed features.

## License

Inherits the **CC BY-NC-SA 4.0** (Attribution-NonCommercial-ShareAlike 4.0 International) license of the upstream repository [lbjlaq/Antigravity-Manager](https://github.com/lbjlaq/Antigravity-Manager). Strictly for non-commercial use.

All account data is encrypted and stored in a local SQLite database and never leaves your device unless explicitly configured.
