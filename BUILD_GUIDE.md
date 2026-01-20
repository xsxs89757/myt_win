# Tauri 多平台打包（GitHub Actions）

本文档说明如何通过 GitHub Actions 自动构建：
- macOS aarch64（Apple Silicon）
- macOS x86_64（Intel）
- Windows x64（面向 Windows 10/11，7/8 兼容性取决于 WebView2 支持）

## 一、触发方式
工作流文件位于 `.github/workflows/tauri-build.yml`，触发方式：
- 手动触发：GitHub → Actions → **Tauri Build** → Run workflow
- 自动触发：推送到 `main` 或 `master` 分支

## 二、产物下载
构建完成后进入对应的 workflow run：
- `macos-aarch64`：通常包含 `.app` 和 `.dmg`
- `macos-x86_64`：通常包含 `.app` 和 `.dmg`
- `windows-x64`：当前 CI 配置输出 NSIS 安装包（`.exe`）

路径统一为：`src-tauri/target/release/bundle/**`

## 三、Windows 7/8 说明
Tauri 在 Windows 端依赖系统 WebView2 运行时：
- Windows 10/11：官方支持最稳定
- Windows 7/8：能否运行取决于系统是否可安装/运行 WebView2

如果目标环境为 7/8，建议先在实际机器上测试安装与运行。

## 四、本地打包（可选）
### macOS
```bash
pnpm install
pnpm tauri build
```

### Windows
```bash
pnpm install
pnpm tauri build
```

> Windows 需要：Rust（MSVC）、Node.js、pnpm、以及 Visual Studio Build Tools。

## 六、如果需要 MSI
当前 CI 为避免 WiX `light.exe` 报错，改为只构建 NSIS。  
如需 MSI，可将 Windows 构建命令改为：
```bash
pnpm tauri build --bundles msi
```

## 五、常见问题
### 1. 未签名/未公证
macOS 上未签名的 `.app/.dmg` 可能被拦截，需要在“系统设置 → 隐私与安全性”里放行。

### 2. 想修改版本号或名称
修改 `src-tauri/tauri.conf.json` 中的：
- `productName`
- `version`
- `identifier`（影响 Cookie/本地存储目录）
