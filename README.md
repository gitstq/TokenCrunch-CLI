# 🦞 TokenCrunch-CLI

<p align="center">
  <b>轻量级终端AI Token智能压缩引擎 | Lightweight Terminal AI Token Smart Compression Engine | 輕量級終端AI Token智能壓縮引擎</b>
</p>

<p align="center">
  <a href="#simplified-chinese">Simplified Chinese</a> •
  <a href="#traditional-chinese">Traditional Chinese</a> •
  <a href="#english">English</a>
</p>

---

## Simplified Chinese

### 🎉 项目介绍

**TokenCrunch-CLI** 是一款零依赖、纯 Python 实现的轻量级终端 AI Token 智能压缩引擎。在将内容投喂给大语言模型（LLM）之前，智能压缩文本、代码、Markdown、日志和 RAG 文档，**最高可节省 50%~90% 的 Token 消耗**，显著降低 API 调用成本。

本项目灵感来源于 GitHub Trending 热门项目 [headroom](https://github.com/chopratejas/headroom)，但在其基础上进行了深度差异化优化：
- 🎯 **纯 CLI 工具形态**：无需安装库或代理，开箱即用
- 🇨🇳 **中文内容专项优化**：针对中文 Token 占比高的特点优化压缩策略
- 🔧 **5 种压缩模式**：文本、代码、Markdown、日志、RAG 文档预处理
- 📊 **多模型 Token 估算**：支持 GPT-4、Claude、GLM-5.1、DeepSeek、Qwen、Llama 等
- 🖥️ **交互式 TUI**：内置终端交互界面，零基础也能轻松上手

### ✨ 核心特性

| 特性 | 说明 |
|------|------|
| 🚀 **零依赖** | 纯 Python 标准库实现，无需安装任何第三方包 |
| 🧠 **智能压缩** | 保留语义的前提下，去除冗余内容 |
| 📁 **5 种模式** | text / code / markdown / log / rag |
| 🔢 **Token 估算** | 支持 8+ 主流 LLM 模型的 Token 计数 |
| 📈 **实时统计** | 压缩前后 Token 数、节省比例一目了然 |
| 🔄 **管道支持** | 支持 Unix 管道操作，无缝集成工作流 |
| 📂 **批量处理** | 支持递归目录批量压缩 |
| 🎨 **TUI 界面** | 交互式终端界面，操作更直观 |

### 🚀 快速开始

#### 环境要求
- Python 3.8+
- 纯标准库，无需额外依赖

#### 安装

```bash
# 克隆仓库
git clone https://github.com/gitstq/TokenCrunch-CLI.git
cd TokenCrunch-CLI

# 安装（可选）
pip install -e .

# 或直接运行
python -m tokensqueeze_cli.cli.main --help
```

#### 基础用法

```bash
# 压缩单个文件（自动识别模式）
tokensqueeze input.txt

# 代码压缩（去除注释、精简空白）
tokensqueeze code.py --mode code --stats

# 激进模式压缩（更大压缩率）
tokensqueeze doc.md --mode markdown --aggressive --stats

# 日志智能摘要
tokensqueeze app.log --mode log --aggressive

# 管道输入
cat article.txt | tokensqueeze - --stats

# 批量处理目录
tokensqueeze data/ --recursive --output compressed/

# 交互式 TUI 模式
tokensqueeze --tui
```

### 📖 详细使用指南

#### 压缩模式说明

| 模式 | 适用场景 | 压缩策略 |
|------|---------|---------|
| `text` | 普通文本 | 去除冗余空格、简化标点、去除填充词 |
| `code` | 源代码 | 去除注释、精简空白、可选去除文档字符串 |
| `markdown` | Markdown 文档 | 去除 HTML 标签、简化表格、压缩图片标记 |
| `log` | 日志文件 | 按模式去重、提取关键事件、智能摘要 |
| `rag` | RAG 文档 | 语义段落压缩、关键句提取、去除低价值内容 |

#### 命令行参数

```
positional arguments:
  input                 输入文件、目录或 "-" 表示标准输入

options:
  -h, --help            显示帮助信息
  -o, --output          输出文件或目录
  -m, --mode            压缩模式: text/code/markdown/log/rag
  -a, --aggressive      启用激进压缩（可能丢失部分格式）
  --model               LLM 模型: gpt-4/claude/glm/deepseek/qwen/llama
  -r, --recursive       递归处理目录
  -s, --stats           显示压缩统计信息
  --dry-run             仅显示统计，不写入输出
  --min-ratio           最小压缩比例阈值
  -v, --version         显示版本
  --tui                 启动交互式 TUI 模式
```

#### 典型使用场景

**场景 1：压缩代码文件节省 Token**
```bash
$ tokensqueeze src/main.py --mode code -a --stats

==================================================
📊  Compression Stats: src/main.py
==================================================
   Mode:       code
   Original:   1,245 tokens
   Compressed: 678 tokens
   Saved:      567 tokens
   Ratio:      45.5%
==================================================
```

**场景 2：日志去重摘要**
```bash
$ tokensqueeze server.log --mode log -a --stats

=== Log Summary: 4 unique patterns from 1,247 entries ===
[INFO] (x892) [T] INFO Request processed...
[WARN] (x45) [T] WARN High latency detected...
[ERROR] (x12) [T] ERROR Database connection failed...
```

**场景 3：RAG 文档预处理**
```bash
$ tokensqueeze knowledge_base.md --mode rag -a -o kb_compressed.md
```

### 💡 设计思路与迭代规划

#### 技术选型原因
- **纯 Python 标准库**：确保零依赖，任何有 Python 的环境都能运行
- **字符级 Token 估算**：无需下载大体积 tokenizer 模型，轻量快速
- **语言感知压缩**：针对中英文不同特点采用差异化策略

#### 后续迭代计划
- [ ] 支持更多压缩模式（JSON、YAML、XML）
- [ ] 集成真实 tokenizer（tiktoken、transformers 可选）
- [ ] 配置文件支持（.tokensqueezerc）
- [ ] 插件系统支持自定义压缩规则
- [ ] 并行批量处理优化

### 📦 打包与部署

```bash
# 构建发布包
make build

# 运行测试
make test

# 清理构建产物
make clean
```

### 🤝 贡献指南

欢迎提交 Issue 和 PR！请阅读 [CONTRIBUTING.md](CONTRIBUTING.md) 了解详情。

### 📄 开源协议

本项目采用 [MIT License](LICENSE) 开源协议。

---

## Traditional Chinese

### 🎉 項目介紹

**TokenCrunch-CLI** 是一款零依賴、純 Python 實現的輕量級終端 AI Token 智能壓縮引擎。在將內容投餵給大語言模型（LLM）之前，智能壓縮文本、代碼、Markdown、日誌和 RAG 文檔，**最高可節省 50%~90% 的 Token 消耗**。

### ✨ 核心特性

- 🚀 **零依賴** - 純 Python 標準庫，無需第三方包
- 🧠 **智能壓縮** - 保留語義的同時去除冗餘內容
- 📁 **5 種模式** - text / code / markdown / log / rag
- 🔢 **Token 估算** - 支援 8+ 主流 LLM 模型
- 📈 **實時統計** - 壓縮前後 Token 數、節省比例一目瞭然
- 🎨 **TUI 界面** - 交互式終端界面

### 🚀 快速開始

```bash
git clone https://github.com/gitstq/TokenCrunch-CLI.git
cd TokenCrunch-CLI
pip install -e .

# 基礎用法
tokensqueeze input.txt --stats
tokensqueeze code.py --mode code -a
tokensqueeze app.log --mode log -a
```

### 📖 使用指南

詳細參數與使用場景請參考上方簡體中文版本。

### 📄 開源協議

[MIT License](LICENSE)

---

## English

### 🎉 Introduction

**TokenCrunch-CLI** is a zero-dependency, pure Python lightweight terminal AI Token smart compression engine. Intelligently compress text, code, Markdown, logs, and RAG documents before feeding them to LLMs, **saving up to 50%~90% of Token consumption**.

### ✨ Key Features

- 🚀 **Zero Dependencies** - Pure Python standard library, no third-party packages
- 🧠 **Smart Compression** - Remove redundancy while preserving semantics
- 📁 **5 Modes** - text / code / markdown / log / rag
- 🔢 **Token Estimation** - Support 8+ major LLM models
- 📈 **Real-time Stats** - Before/after token counts and savings ratio
- 🎨 **TUI Interface** - Interactive terminal UI

### 🚀 Quick Start

```bash
git clone https://github.com/gitstq/TokenCrunch-CLI.git
cd TokenCrunch-CLI
pip install -e .

# Basic usage
tokensqueeze input.txt --stats
tokensqueeze code.py --mode code -a
tokensqueeze app.log --mode log -a
```

### 📖 Usage

For detailed parameters and usage scenarios, please refer to the Simplified Chinese version above.

### 📄 License

[MIT License](LICENSE)
