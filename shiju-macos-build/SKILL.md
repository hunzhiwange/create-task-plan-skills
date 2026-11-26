---
name: shiju-macos-build
description: "为 ShiJu 生成本地 macOS 安装包。先运行 make release-check 并修复错误，再使用 scripts/bundle_macos.sh 构建本机 DMG，修复打包错误并验证产物。"
---

# ShiJu 本地 macOS 打包

在 macOS 上从 ShiJu 仓库根目录依次执行。

1. 运行 `make release-check`。定位并修复检查发现的错误，重新运行检查直到通过。
2. 通过 Cargo 槽位运行原生打包脚本，生成本机架构的 DMG。脚本默认只生成 `.app`，安装包需要开启 `CREATE_DMG=1`。

   ```bash
   MODE=app PROFILE=release SKIP_BUILD=0 TARGET= OUT_DIR=dist/macos \
     MACOS_SIGN_MODE=adhoc MACOS_NOTARIZE=0 PACKAGE_APP=0 CREATE_DMG=1 \
     python3 scripts/cargo_slot.py ./scripts/bundle_macos.sh
   ```

3. 打包失败时，定位并修复构建、签名或镜像生成错误，然后重跑打包。如果修复改动了代码或发布配置，先重新通过 `make release-check`。遇到当前环境无法解决的问题时，报告具体阻塞。
4. 从本次打包日志的 `DMG package:` 取出实际产物路径，确认文件存在且非空，再运行 `hdiutil verify` 验证镜像。向用户提供 DMG 的绝对路径及检查结果。
