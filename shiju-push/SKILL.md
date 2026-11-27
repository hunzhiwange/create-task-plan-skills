---
name: shiju-push
description: 用户指定版本发布并推送 ShiJu 时使用。只需版本号，整理工作区提交，验证发布目标，更新产品版本并推送 shiju 分支和版本标签。
---

# ShiJu 发布与推送

用法 `$shiju-push v0.8.2`。唯一参数是 `X.Y.Z` 或 `vX.Y.Z`，将它规范为版本 `X.Y.Z` 和标签 `vX.Y.Z`。调用此技能并提供版本即授权提交，以及向 `origin` 推送 `shiju` 分支和该版本标签。

1. 在 ShiJu 主仓库根目录执行。当前目录属于主仓库时使用当前仓库，否则定位 `~/worker/code/shiju`。核对当前分支为 `shiju`，`origin` 指向 `hunzhiwange/shiju`。检查工作区、未完成的 Git 操作及本地和远端同名标签。比较当前产品版本和目标版本，正常发版须递增。当前版本已等于目标版本或已有同名标签时，走下方的恢复流程。
2. 工作区有改动时，按本机 `git-commit` 技能审查暂存、未暂存和未跟踪内容，按用途提交属于本次发布的变更。遇到敏感文件或归属不明的改动，保留相应文件，说明发现并确认范围。确认工作区干净后继续。
3. 运行 `make release-check`。定位并修复检查错误，重跑直至通过。若修复改动了源码或配置，按用途提交这些修复。环境或权限阻塞时报告具体原因。记录通过检查的提交。
4. 运行 `scripts/switch_version.sh X.Y.Z`。用 `git status --short`、`git diff` 和 `git diff --check` 核对仅有 `Cargo.toml`、`Cargo.lock` 的预期版本变更，运行 `scripts/switch_version.sh --check`。暂存这两个文件，核对暂存内容后运行 `git commit -m 'feat: new version X.Y.Z'`。
5. 确认发布提交接在通过检查的代码上，再用 `git tag vX.Y.Z` 指向该提交。使用一次原子推送同时更新分支和标签：

   ```bash
   git push --atomic origin refs/heads/shiju:refs/heads/shiju refs/tags/vX.Y.Z:refs/tags/vX.Y.Z
   ```

6. 用 `git ls-remote origin refs/heads/shiju refs/tags/vX.Y.Z` 确认目标发布提交已进入远端分支、标签指向同一发布提交。报告版本、发布提交 SHA、发布检查和推送结果。

## 恢复同版本发布

遇到同名标签或推送失败，先查询本地与远端引用，保留已有标签。只有确认当前 `HEAD` 就是该版本的发布提交、版本文件一致且现存标签都指向该提交，才复用该提交。若远端已经包含发布提交且标签匹配，报告已经完成。需要继续推送时，核对原发布检查结果，缺少通过记录时重跑检查，然后补齐缺失的本地标签并原子推送。遇到标签冲突、远端分支分叉或推送失败原因无法排除时，报告具体状态及提交 SHA。远端分支始终使用快进更新。
