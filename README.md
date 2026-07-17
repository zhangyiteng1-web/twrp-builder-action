# TWRP Builder Action

通过 GitHub Actions 自动化编译 TWRP（Team Win Recovery Project）镜像，完全在云端运行，无需本地环境。

---

## 适用版本

- ✅ **Android 10 及以上**（使用 `twrp-10.0`、`twrp-11.0`、`twrp-12.1` 等分支）
- ❌ **Android 9.0 及更早**：请使用预编译镜像（[TWRP 官网](https://twrp.me/Devices/)）或在 Ubuntu 18.04 环境下自行编译（`twrp-9.0` 分支）

---

## 功能特点

- 基于 TWRP 官方 Minimal Manifest 源码
- 支持自定义设备树仓库和分支
- 自动选择正确的 manifest 仓库（`platform_manifest_twrp_aosp`）
- 产物 `recovery.img` 以 Artifact 形式提供，可直接下载
- 支持传递额外编译环境变量（如 `WITHOUT_TWRP_MAKEFILE=true`）

---

## 使用方法

### 1. 前置条件

- 已有一个可用的 TWRP 设备树仓库（可从 [twrpdtgen-action](https://github.com/yourname/twrpdtgen-action) 生成）
- 设备树必须与目标 Android 版本匹配（例如 Android 12.1 设备树应使用 `android-12.1` 分支）

### 2. 运行工作流

1. 进入本仓库的 **Actions** 选项卡
2. 选择 **Build TWRP** 工作流
3. 点击 **Run workflow** 并填写以下参数：

| 参数 | 必填 | 说明 | 示例 |
|------|------|------|------|
| `device_tree_repo` | ✅ | 设备树仓库的 HTTPS 地址 | `https://github.com/yourname/android_device_xiaomi_beryllium.git` |
| `device_tree_branch` | ✅ | 设备树使用的分支 | `android-12.1` |
| `manufacturer` | ✅ | 设备制造商（小写） | `xiaomi` |
| `codename` | ✅ | 设备代号（小写） | `beryllium` |
| `twrp_branch` | ✅ | TWRP 源码分支（推荐 `twrp-12.1`） | `twrp-12.1` |
| `extra_env` | ❌ | 额外编译环境变量（可选） | `WITHOUT_TWRP_MAKEFILE=true` |

4. 点击 **Run workflow** 开始编译（耗时约 60~90 分钟）

### 3. 获取编译结果

编译成功后，在 Actions 运行页面底部找到 **Artifacts** 区域，下载 `twrp-<设备代号>.zip`，解压后即可获得 `recovery.img`。

---

## 参数详解

### `twrp_branch` 选择指南

| 设备 Android 版本 | 推荐 TWRP 分支 |
|-------------------|----------------|
| Android 10        | `twrp-10.0`    |
| Android 11        | `twrp-11.0`    |
| Android 12 / 12.1 | `twrp-12.1`    |
| Android 13        | `twrp-12.1`（暂未有专用分支，兼容使用） |

> **注意**：请勿使用 `android-*` 格式分支，必须使用 `twrp-*` 前缀。

---

## 环境变量（可选）

如需传递额外环境变量，可在 `extra_env` 字段中填写，每行一个，例如：
WITHOUT_TWRP_MAKEFILE=true
TARGET_KERNEL_CLANG_COMPILE=false
这些变量将在编译前注入构建环境。

---

## 常见问题

**Q: 编译失败，提示 `repo init` 找不到分支？**  
A: 请确认 `twrp_branch` 是否使用了正确的 `twrp-*` 格式，并确保该分支存在于 [platform_manifest_twrp_aosp](https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp) 仓库中。

**Q: 编译报错 `vendor/omni/config/common.mk` 不存在？**  
A: 工作流已自动创建占位文件，此错误不影响正常编译。

**Q: 编译产物无法在设备上启动？**  
A: 请检查设备树是否正确（特别是内核、分区信息）。如有问题，可根据设备实际分区调整 `BoardConfig.mk` 并重新编译。

**Q: 我想编译 Android 9.0 的 TWRP，怎么做？**  
A: 本项目暂不支持自动编译 Android 9.0 及更早版本。建议您从 [TWRP 官网](https://twrp.me/Devices/) 下载官方镜像，或使用 Ubuntu 18.04 本地编译（`twrp-9.0` 分支）。

---

## 许可证

Apache-2.0

---

## 贡献

欢迎提交 Issue 或 PR。如遇问题，请附上完整的 Actions 日志链接。

---
Happy building! 🛠️
