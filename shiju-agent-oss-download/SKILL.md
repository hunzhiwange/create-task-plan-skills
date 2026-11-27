---
name: shiju-agent-oss-download
description: 从 hunzhiwange/shiju 的 GitHub Releases 下载 ShiJu 六平台八个桌面安装包和 Web 静态版。用于单独下载 Web ZIP 供安装，或下载并校验指定版本或最新版的 OSS 发布文件。
---

# 下载 ShiJu 发布产物

使用用户指定的稳定版本（`X.Y.Z` 或 `vX.Y.Z`）。用户要求最新版时，先在 [GitHub Releases](https://github.com/hunzhiwange/shiju/releases/latest) 确认最新正式版的 tag，再使用实际 tag。

## 单独下载 Web 版

只下载 `shiju-web-X.Y.Z.zip`。桌面端的 Web 安装也是直接下载 ZIP 并检查解压内容。将下例的 `X.Y.Z` 换成实际版本，执行前切换到用户指定的输出目录，或用 `--output` 指定目标文件。

```sh
version=X.Y.Z
curl --fail --location --remote-name \
  "https://github.com/hunzhiwange/shiju/releases/download/v${version}/shiju-web-${version}.zip"
```

## 准备 OSS 发布文件

从当前 ShiJu 源码仓库根目录运行。

```sh
python3 scripts/prepare_oss_release.py \
  --version vX.Y.Z \
  --output-dir "$(cd /tmp && pwd -P)/shiju-oss"
```

将 `vX.Y.Z` 换成实际版本。用户指定输出位置时，传入对应的绝对路径，并解析符号链接组件。脚本校验下载文件的大小和 SHA-256，并将产物保存到输出目录下的 `vX.Y.Z/`。

- 桌面版包含两份 DEB、两份 RPM、两份 MSI 和两份 DMG，均列入 `manifest.json`。
- Web 静态版保存为同目录下的 `shiju-web-X.Y.Z.zip` 和 `shiju-web-X.Y.Z.zip.sha256`（文件名中的版本省略 `v`）。OSS 发布器用 `.sha256` 复验 ZIP。Web 两个文件独立于桌面清单。

用户仅需预览时加 `--metadata-only`，生成桌面 `manifest.preview.json`，并在终端显示 Web 两个文件的元数据。下载失败时可用原命令重试，脚本会复用已校验的文件。完成后报告实际版本、产物目录、Web ZIP 与校验文件路径，以及校验结果。到本地文件准备完成为止。
