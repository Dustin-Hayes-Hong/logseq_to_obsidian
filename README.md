# Logseq → Obsidian Converter

![tests](https://github.com/sercxanto/logseq_to_obsidian/actions/workflows/tests.yml/badge.svg)
![lint](https://github.com/sercxanto/logseq_to_obsidian/actions/workflows/lint.yml/badge.svg)
[![codecov](https://codecov.io/gh/sercxanto/logseq_to_obsidian/graph/badge.svg?token=bTE5A3niNf)](https://codecov.io/gh/sercxanto/logseq_to_obsidian)

[简体中文](#简体中文) | [English](#english)

---

<h2 id="简体中文">简体中文</h2>

### 概述

将 Logseq 库（Markdown 格式）转换为对 Obsidian 友好的格式。本工具旨在解决 Logseq 与 Obsidian 之间的元数据差异、链接逻辑及插件兼容性问题。

### 核心功能

- **页面属性转换**：
    - 将顶部的 `key:: value` 转换为 YAML front matter。
    - **支持中文属性名**：完美识别如 `类别::`、`作者::` 等非 ASCII 属性。
    - **嵌套标签支持**：可选将属性转换为 `key/value` 格式的标签，并自动处理英文字符小写。
- **全局别名 (Alias) 解析**：
    - 自动扫描全库别名。如果引用了别名 `[[B]]`，脚本会根据库索引将其转换为 `[[原页面标题|B]]`，确保 Obsidian 链接不断联。
- **任务标记系统**：
    - 支持 Logseq 的所有 10 种状态（TODO, DOING, DONE 等）。
    - 映射优先级、日程（SCHEDULED）和截止日期（DEADLINE）为 Obsidian 任务插件格式。
- **块 ID 与块引用**：
    - 将 Logseq 的块 ID 转换为 Obsidian 锚点 (`^uuid`)。
    - 自动将全库的 `((uuid))` 引用转换为正确的 `[[文件名#^uuid]]`。
- **文件层级优化**：
    - 自动将文件名中的 `___` 转换为子目录结构（如 `A___B.md` → `A/B.md`）。
- **缺失页面处理**：
    - 自动为被引用但不存在的页面创建纯净的占位文件（仅含 H1 标题）。

### 安装指南

本项目需要 **Python 3.9+**。

#### 1. 使用 pip (所有平台)
```bash
pip install logseq-to-obsidian
```

#### 2. 使用 pipx (推荐，环境隔离)
```bash
pipx install logseq-to-obsidian
```

### 操作说明

#### 🐧 Linux / 🍎 macOS
打开终端并执行：
```bash
logseq-to-obsidian --input "~/LogseqVault" --output "~/ObsidianVault" --tag-all-properties --create-missing-pages
```

#### 🪟 Windows
建议使用 **PowerShell** 或 **CMD**。请确保 Python 的 `Scripts` 文件夹已添加到系统环境变量 PATH 中。

**PowerShell 示例**:
```powershell
logseq-to-obsidian --input "C:\Users\Name\Documents\Logseq" --output "C:\Users\Name\Documents\Obsidian" --tag-all-properties --create-missing-pages
```

**提示**: 如果路径中包含空格，请务必使用双引号包裹。

### 常用参数

- `--input`: Logseq 库根目录。
- `--output`: 目标 Obsidian 库路径。
- `--tag-all-properties`: 将所有自定义属性转换为标签（如 `类别:: [[书籍]]` -> `#类别/书籍`）。
- `--tag-property <key>`: 仅将特定属性转换为标签。
- `--create-missing-pages`: 自动创建丢失的链接文件。
- `--keep-pages`: 保留 `pages/` 顶级文件夹，不进行目录扁平化。
- `--daily-folder <name>`: 指定日记文件夹名称（如 "Daily Notes"）。
- `--dry-run`: 预览转换计划而不写入文件。

---

<h2 id="english">English</h2>

### Overview

A comprehensive tool to convert Logseq Markdown vaults into Obsidian-friendly formats, resolving discrepancies in metadata, link logic, and task management.

### Key Features

- **Property Promotion**:
    - Converts `key:: value` to YAML front matter.
    - **Unicode Support**: Correctly parses non-ASCII keys (e.g., Chinese `类别`, `作者`).
    - **Nested Tags**: Promotes properties to `#key/value` tags with automatic lowercasing for English values.
- **Global Alias Resolution**:
    - Scans the entire vault for aliases. Rewrites `[[Alias]]` to `[[RealTitle|Alias]]` based on a global index.
- **Task Management**:
    - Maps all 10 Logseq states and handles Priorities, SCHEDULED, and DEADLINE metadata for Obsidian compatibility.
- **Block Reference Resolution**:
    - Converts `((uuid))` references into precise cross-file `[[File#^id]]` links.
- **Hierarchy Mapping**:
    - Expands `___` in filenames into a real folder hierarchy (e.g., `A___B.md` -> `A/B.md`).
- **Minimal Placeholders**:
    - Automatically creates minimal (H1 only) pages for missing wikilink targets.

### Installation

Requires **Python 3.9+**.

#### 1. Using pip
```bash
pip install logseq-to-obsidian
```

#### 2. Using pipx (Recommended)
```bash
pipx install logseq-to-obsidian
```

### Running the Tool

#### 🐧 Linux / 🍎 macOS
Run in your terminal:
```bash
logseq-to-obsidian --input "~/LogseqVault" --output "~/ObsidianVault" --tag-all-properties --create-missing-pages
```

#### 🪟 Windows
Use **PowerShell** or **Command Prompt**. Ensure your Python `Scripts` directory is in your PATH.

**PowerShell Example**:
```powershell
logseq-to-obsidian --input "C:\Users\Name\Logseq" --output "C:\Users\Name\Obsidian" --tag-all-properties --create-missing-pages
```

### Options

- `--input`, `--output`: Source and destination directories.
- `--tag-all-properties`: Convert all custom properties to nested tags.
- `--tag-property <key>`: Convert specific properties to tags.
- `--create-missing-pages`: Create placeholder files for broken wikilinks.
- `--keep-pages`: Preserve the `pages/` directory structure.
- `--daily-folder <name>`: Specify the output directory for journals.
- `--dry-run`: Preview changes without writing to disk.

---

### Development

See [CONTRIBUTING.md](CONTRIBUTING.md).
