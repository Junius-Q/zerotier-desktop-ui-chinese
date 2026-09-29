# ZeroTier Desktop UI 中文汉化版

[ZeroTier 桌面界面](https://github.com/zerotier/DesktopUI)（Rust + libui-ng 跨平台托盘应用）的**中文汉化构建**。

针对 Windows 平台编译，界面文字已全部汉化（系统托盘菜单、加入网络对话框、关于对话框、系统通知等）。

## 📌 版本说明

**本汉化版的版本号 = 对应的 ZeroTier One 核心版本**，格式与上游保持一致（纯版本号，不加后缀）。

| 项 | 值 |
|---|---|
| 本版版本号 | **1.16.2** |
| 对应 ZeroTier One | 1.16.2 |
| 上游源码基线 | [`zerotier/DesktopUI`](https://github.com/zerotier/DesktopUI) `main @ 933e391`（2026-05-27） |
| 汉化改动范围 | `src/main.rs`、`src/join.rs`、`src/about.rs`（共 3 个文件，**仅字符串翻译，不改逻辑**） |

> **关于上游版本号**：上游仓库的 Releases 页停留在 **`1.8.3`（2021-11-16）**，那是**已废弃的旧发布**，请勿据此判断新旧。
>
> 上游后来不再单独给桌面 UI 打 tag —— 该 UI 现在**随 ZeroTier One 安装包一起分发**（本机路径 `C:\Program Files (x86)\ZeroTier\One\zerotier_desktop_ui.exe`）。当前公开 `main` 的代码是 2026-05-22 从 ZeroTier 内部仓库合并出来的 **1.16.2** 版本；`Cargo.toml` 里的 `1.10.0` 是 2022 年定下的内部代号，**不代表发布版本**。
>
> 界面上「关于」对话框显示的版本号是**运行时**由 ZeroTier 服务传进来的（`src/main.rs` 读 `args[2]`），与 `Cargo.toml` 无关 —— 官方设计上就是让 UI 跟随服务端版本。

## 📦 下载

- 编译好的中文版 exe：见 **`dist/zerotier_desktop_ui_zh.exe`**，或前往 [Releases](../../releases) 下载（当前版本 [`1.16.2`](../../releases/tag/1.16.2)）。

## 🚀 使用方法

1. 关闭正在运行的 ZeroTier 界面（托盘右键 → 退出）。
2. 备份原版 `C:\Program Files (x86)\ZeroTier\One\zerotier_desktop_ui.exe`。
3. 用 `dist/zerotier_desktop_ui_zh.exe` 覆盖安装目录下的同名文件（需要管理员权限）。
4. 重新启动 ZeroTier。

> ⚠️ **SmartScreen 提示**：自行编译的 exe 没有官方数字签名，首次运行若被 Windows 拦截，点击"更多信息" → "仍要运行"即可。
>
> ⚠️ **自动更新**：官方更新会将界面恢复为英文版。更新后可再次替换，或用备份的原版还原。

## 🖥️ 汉化内容

| 位置 | 内容 |
|---|---|
| 系统托盘菜单 | 我的地址 / 加入新网络 / 托管地址 / 托管路由 / 断开连接 / 重新连接 / 忘记 / 登录时启动界面 / 关于 / 退出 ZeroTier 界面 等 |
| 加入网络对话框 | 加入 ZeroTier 网络 / 请输入 16 位网络 ID 以加入：/ 加入 / 取消 |
| 关于对话框 | 关于 ZeroTier UI / 版权说明 / 确定 |
| 系统通知 | 已复制…到剪贴板 等 |

修改的文件：`src/main.rs`、`src/join.rs`、`src/about.rs`。

## 🛠️ 从源码编译（Windows）

依赖：Rust (rustup MSVC target)、Visual Studio BuildTools (MSVC)、Meson、Ninja。

```bat
:: 1. 构建 libui-ng（MSVC）
call "C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools\VC\Auxiliary\Build\vcvars64.bat"
cd libui-ng
meson setup build --buildtype=release -Db_vscrt=mt --default-library=static --backend=ninja
ninja -C build
cd build\meson-out && rename libui.a ui.lib

:: 2. 构建托盘库（cl.exe）
cd ..\..\..\tray
cl /nologo /c /O2 /DTRAY_WINAPI=1 /std:c11 zt_desktop_tray.c
lib /nologo /OUT:zt_desktop_tray.lib zt_desktop_tray.obj

:: 3. cargo 编译
cd ..
set RUSTFLAGS=-C target-feature=+crt-static
cargo build --release --target=x86_64-pc-windows-msvc
```

## 📄 许可证

本项目基于 [MPL-2.0](LICENSE.txt)（Mozilla Public License 2.0）发布，是 [zerotier/DesktopUI](https://github.com/zerotier/DesktopUI) 的中文翻译改动。原项目版权归 ZeroTier, Inc. 所有。

## ⚠️ 免责声明

本仓库为个人汉化学习用途，**非官方**版本，与 ZeroTier, Inc. 无任何关联。使用自行构建版本可能带来安全与稳定性风险，请谨慎评估。
