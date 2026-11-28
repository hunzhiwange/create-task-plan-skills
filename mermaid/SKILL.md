---
name: mermaid
description: Render Mermaid diagrams as SVG or ASCII art using beautiful-mermaid. Use when users need to create flowcharts, sequence diagrams, state diagrams, class diagrams, or ER diagrams. Supports both graphical SVG output and terminal-friendly ASCII/Unicode output.
---

<!-- Adapted for ShiJu from AionCore 47e66d0d151123e973b3fd1e77afcb5671b3f8c5. See LICENSE.txt. -->

> **⚠️ Platform note — read before running any command.** The shell snippets in this skill are written for **macOS / Linux** (bash/zsh). Always check which OS you are on first. On **Windows** do **not** run them verbatim — the underlying tool/CLI commands are usually cross-platform, but the surrounding shell syntax is not. Translate it to PowerShell before running:
>
> | bash (macOS / Linux) | PowerShell (Windows) |
> | --- | --- |
> | `a && b` | run as two steps, or `a; if ($?) { b }` |
> | `cat <<'EOF' \| tool …` (heredoc) | write the text to a temp file, then pipe/pass that file to the tool |
> | `VAR=$(cmd)` … `$VAR` | `$VAR = cmd` … `$VAR` |
> | `cmd > /dev/null` | `cmd > $null` |
> | `… \| grep PAT` | `… \| Select-String PAT` |
> | `… \| jq …` | `… \| ConvertFrom-Json`, then read the fields |
> | `python3 x.py` | `python x.py` (or `py x.py`) |
> | `~/dir`, `/tmp` | `$env:USERPROFILE\dir`, `$env:TEMP` |
> | `cp` / `mkdir -p` / `rm -rf` | `Copy-Item` / `New-Item -ItemType Directory -Force` / `Remove-Item -Recurse -Force` |
>
> If a command has no obvious Windows equivalent, prefer the built-in file/HTTP tools over raw shell.

# Mermaid Diagram Renderer

Render Mermaid diagrams using `beautiful-mermaid` library. Supports 5 diagram types with dual output modes.

## Quick Start

Use the absolute path of this skill to locate `scripts/render.ts`. Set up the renderer in a writable tool directory so it can resolve its Node dependencies beside the copied script. The example uses a temporary tool directory and leaves diagram inputs and outputs in the workspace.

```bash
renderer_dir="$(mktemp -d)"
cp "$skill_dir/scripts/render.ts" "$renderer_dir/render.ts"
npm install --prefix "$renderer_dir" tsx beautiful-mermaid
```

On Windows, use PowerShell to create the tool directory and copy the script. Use the same `npm --prefix` arguments and pass the absolute path to the copied script. Use absolute paths for input and output files in the commands below.

### SVG Output (Default)

```bash
# From file
npm --prefix "$renderer_dir" exec -- tsx "$renderer_dir/render.ts" diagram.mmd --output diagram.svg

# From stdin
echo "graph LR; A-->B-->C" | npm --prefix "$renderer_dir" exec -- tsx "$renderer_dir/render.ts" --stdin --output flow.svg
```

### ASCII Output (Terminal)

```bash
# ASCII art for terminal display
npm --prefix "$renderer_dir" exec -- tsx "$renderer_dir/render.ts" diagram.mmd --ascii

# Pipe directly
echo "graph TD; Start-->End" | npm --prefix "$renderer_dir" exec -- tsx "$renderer_dir/render.ts" --stdin --ascii
```

Output example:

```
┌───────┐     ┌─────┐
│ Start │────▶│ End │
└───────┘     └─────┘
```

## Supported Diagrams

| Type      | Syntax            | Best For                |
| --------- | ----------------- | ----------------------- |
| Flowchart | `graph TD/LR`     | Processes, decisions    |
| Sequence  | `sequenceDiagram` | API calls, interactions |
| State     | `stateDiagram-v2` | State machines          |
| Class     | `classDiagram`    | OOP design              |
| ER        | `erDiagram`       | Database schemas        |

## Theming (SVG only)

```bash
npm --prefix "$renderer_dir" exec -- tsx "$renderer_dir/render.ts" diagram.mmd --theme github-dark --output out.svg
```

Use invalid theme name to see available themes list (e.g., `--theme ?`)

## Resources

- `scripts/render.ts` - Main rendering script
- `references/syntax.md` - Mermaid syntax quick reference
