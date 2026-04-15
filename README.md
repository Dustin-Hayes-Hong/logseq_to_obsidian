# Logseq → Obsidian Converter

![tests](https://github.com/sercxanto/logseq_to_obsidian/actions/workflows/tests.yml/badge.svg)
![lint](https://github.com/sercxanto/logseq_to_obsidian/actions/workflows/lint.yml/badge.svg)
[![codecov](https://codecov.io/gh/sercxanto/logseq_to_obsidian/graph/badge.svg?token=bTE5A3niNf)](https://codecov.io/gh/sercxanto/logseq_to_obsidian)

[简体中文](#简体中文) | [English](#english)

---

<h2 id="简体中文">简体中文</h2>

### 概述

- 将 Logseq 库（Markdown 格式）转换为对 Obsidian 友好的 Markdown 格式。
- 自动处理：页面属性 → YAML 正文、任务状态、块 ID 锚点、以及跨文件的块引用映射。
- 目录结构：默认将 `pages/` 内容移动到库根目录（可通过参数保留），并自动重命名日记文件。
- **属性转标签**：可选将特定属性（如 `person:: [[A]]`）转换为全局标签。
- **缺失页面生成**：可选自动为未定义的 Wiki 链接创建占位文件。

### 核心功能

- **页面属性转换**：
    - 将顶部的 `key:: value` 转换为 YAML front matter。
    - **引号引用**：自动对数字别名（如 `alias:: 1`）和含有 Wiki 链接的属性（如 `word:: [[link]]`）添加引号，确保元数据在 Obsidian 中被正确识别。
- **标签规范化**：
    - 将 `tags::` 转换为 YAML 数组。
    - 优先级顺序：按逗号分隔符、Wiki 链接、`#hashtag` 的顺序依次提取并排重。
- **任务标记系统**：
    - 兼容 Logseq 的 10 种任务状态（如 `TODO`, `DOING`, `DONE`, `CANCELED` 等）。
    - 状态映射：`DONE/CANCELED` 映射为已完成 `[x]`，其余映射为未完成 `[ ]`。
    - 支持优先级：`[#A/B/C]` 可选输出为 Emoji (`⏫/🔼/🔽`) 或 Dataview 字段。
    - 日期支持：自动解析 `SCHEDULED` 和 `DEADLINE` 及其循环规则（Repeater），并移除多余的时间戳标记。
- **块 ID 与块引用解析**：
    - 块 ID：将 `id:: <uuid>` 转换为行尾的 `^<uuid>` 锚点。
    - 跨文件引用：扫描全库以将 `((<uuid>))` 转换为正确的 `[[文件名#^<uuid>]]` 格式。
- **路径与文件管理**：
    - **层级转换**：自动将文件名中的 `___` 转换为子目录结构（例如 `pages/A___B.md` → `A/B.md`）。
    - **自动过滤**：转换过程中会自动跳过 `.git/`, `logseq/` 元数据目录及不支持的 `whiteboards/` 目录。
    - **特殊处理**：对含有百分比编码（Percent-encoded）的文件名发出警告，提示可能的链接失效风险。
- **标题与折叠优化**：
    - 如果标题行后直接紧跟缩进列表，会自动将标题转换为列表项（`- # 标题`），以保持 Obsidian 中的折叠逻辑与 Logseq 一致。

### 安装

```bash
# 推荐使用 pipx
pipx install logseq-to-obsidian
# 或使用 pip
pip install logseq-to-obsidian
```

### 使用方法

```bash
logseq-to-obsidian \
  --input /path/to/logseq-vault \
  --output /path/to/obsidian-vault \
  --daily-folder "Daily Notes" \
  --tasks-format emoji \
  --keep-pages
```

### 参数详解

- `--input`: Logseq 库根目录（必须包含 `pages/`）。
- `--output`: 目标 Obsidian 库路径。
- `--daily-folder <name>`: 将日记（journals）移动到指定的子文件夹。
- `--tasks-format {emoji|dataview}`: 任务元数据的表现风格。
- `--field-key <key>`: 将 `[[key/value]]` 形式的链接转换为 Dataview 行内字段。
- `--tag-property <key>`: 将指定属性（Key 和所有 Value）转换为标签（Tags）。
- `--keep-pages`: 在输出库中保留 `pages/` 顶级文件夹。
- `--create-missing-pages`: 自动为库中不存在但被引用的 Wiki 链接创建占位文件。
- `--dry-run`: 预览转换结果而不执行写入。

---

<h2 id="english">English</h2>

### Overview

- Converts a Logseq vault to Obsidian-friendly Markdown.
- Handles automated mapping of properties, task states, block anchors, and cross-file references.
- Optimizes folder structure by flattening `pages/` (optional) and renaming journals.

### Key Features

- **YAML Front Matter**:
    - Converts `key:: value` pairs with smart quoting for numbers and `[[wikilinks]]` to ensure Obsidian compatibility.
- **Property to Tags**:
    - Optionally promotes specific properties (and their values) to the `tags` field.
- **Missing Pages**:
    - Optionally creates placeholder files for wikilinks pointing to non-existent pages.
- **Tag Normalization**:
    - Extracts tags from comma-separated strings, wikilinks, and hashtags while preserving a natural order.
- **Advanced Task States**:
    - Supports all 10 Logseq states, priorities, and complex date/repeater metadata.
- **Block Reference Resolution**:
    - Scans the entire vault to turn `((uuid))` references into precise `[[File#^id]]` links.
- **Path & Hierarchy**:
    - Automatically expands `___` separators in filenames into a real folder hierarchy.
    - Filters out `.git/`, `logseq/` metadata, and `whiteboards/`.
- **Folding Consistency**:
    - Identifies headings followed by indented lists and converts them to list-item headings (`- # Heading`) for consistent UI behavior.

### Usage

```bash
logseq-to-obsidian --input <logseq_path> --output <obsidian_path> --keep-pages
```

### Options

- `--input`, `--output`: Source and destination directories.
- `--daily-folder`: Rename and move journals.
- `--keep-pages`: Opt-out of `pages/` flattening.
- `--create-missing-pages`: Automatically create placeholder files for missing wikilink targets.
- `--tag-property`: Promote specific properties to global tags.
- `--field-key`: Convert namespaced wikilinks to Dataview fields.

---

### Development / Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md)
