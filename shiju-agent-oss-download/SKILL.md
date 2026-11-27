---
name: shiju-agent-oss-download
description: 从 hunzhiwange/shiju 的 GitHub Releases 下载并校验 ShiJu 六平台八个桌面安装包，生成供后续 OSS 发布使用的本地目录。用于拉取指定版本或最新版安装包、准备 OSS 上传文件及预览清单。
---

# 下载 ShiJu 发布产物

从当前 ShiJu 源码仓库根目录运行 `scripts/prepare_oss_release.py`。使用用户指定的稳定版本（`X.Y.Z` 或 `vX.Y.Z`）。用户要求最新版时，先在 [GitHub Releases](https://github.com/hunzhiwange/shiju/releases/latest) 确认最新正式版的 tag，再把实际 tag 传给脚本。

```sh
python3 scripts/prepare_oss_release.py \
  --version vX.Y.Z \
  --output-dir "$(cd /tmp && pwd -P)/shiju-oss"
```

将 `vX.Y.Z` 换成实际版本。用户指定输出位置时，传入对应的绝对路径，并解析符号链接组件。脚本校验每个包的大小和 SHA-256，成功后在输出目录下创建 `vX.Y.Z/manifest.json`。桌面清单包含两份 DEB、两份 RPM、两份 MSI 和两份 DMG。

用户仅需预览时加 `--metadata-only`，输出 `manifest.preview.json`。下载失败时可用原命令重试，脚本会复用已校验的文件。完成后报告实际版本、产物目录和校验结果。到本地文件准备完成为止。
