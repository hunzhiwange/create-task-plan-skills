---
name: shiju-release
description: "在 ShiJu 主仓库按固定顺序发版：运行 make release-check 并修复错误、更新产品版本、提交、打版本 tag，再推送 tag 触发 GitHub Actions。用于明确要求发布 ShiJu 或调用 $shiju-release 发版时。"
---

# ShiJu 发版

在 ShiJu 仓库根目录执行。使用用户指定的 `X.Y.Z`，版本带 `v` 前缀时去掉前缀。实际发版需要明确版本和推送授权。用户明确要求用该技能发布指定版本时，按以下步骤执行。

1. 先运行 `make release-check`。定位并修复检查发现的代码错误，再运行同一命令，直到通过。遇到无法解决的环境或权限问题时，报告具体阻塞并停在此步。
2. 运行 `scripts/switch_version.sh X.Y.Z`，更新产品版本和锁文件。
3. 检查变更和未跟踪文件，确认即将提交的内容属于本次发布且无敏感文件。发现范围不清的改动时，保留这些改动并向用户确认。然后在仓库根目录运行 `git add .`。
4. 运行 `git commit -m 'feat: new version X.Y.Z'`。
5. 运行 `git tag vX.Y.Z`，让 tag 指向刚创建的发版提交。
6. 运行 `git push origin vX.Y.Z`，推送该 tag 以触发 GitHub Actions。

将命令中的 `X.Y.Z` 替换为实际版本。每一步成功后再进入下一步。遇到同名 tag 或推送失败时，检查本地和远端 tag 状态，保留已有 tag，避免重复创建提交。完成后报告版本、提交 SHA 和 tag 推送结果。
