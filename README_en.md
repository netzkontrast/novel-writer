# Novel Writer - An AI-Powered Chinese Novel Writing Tool

[![npm version](https://badge.fury.io/js/novel-writer-cn.svg)](https://www.npmjs.com/package/novel-writer-cn)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> 🚀 An AI-powered intelligent novel writing assistant based on Specification-Driven Development (SDD)
>
> Systematically create high-quality novels using slash commands directly in AI assistants like Claude, Cursor, and Gemini.

## ✨ Core Features

- 📚 **Slash Commands** - Use directly in AI assistants like Claude, Gemini, Codex, Cursor, Windsurf, and Roo Code
- 🎯 **Seven-Step Methodology** - A systematic creation process based on Specification-Driven Development (SDD)
- 🤖 **Intelligent Assistance** - AI understands the context and provides targeted writing suggestions
- 📝 **Optimized for Chinese** - Designed specifically for Chinese novel writing, supporting word count statistics and multi-clue management
- 🔄 **Cross-Platform** - Supports 13 AI tools and runs on Windows/Mac/Linux
- 🔌 **Plugin System** - Extend functionality with plugins for authentic voice, translation, style imitation, and more
- ✅ **Quality Assurance** - Plot tracking, timeline management, and character consistency verification

> 📖 **Detailed Features**: Check out [CHANGELOG.md](CHANGELOG.md) for complete updates in each version.

## 🚀 Quick Start

### 1. Installation

```bash
npm install -g novel-writer-cn
```

### 2. Initialize a Project

```bash
# Basic usage
novel init my-novel

# Recommended: Pre-install the authentic voice plugin
novel init my-novel --plugins authentic-voice

# Specify an AI platform
novel init my-novel --ai claude    # Claude Code
novel init my-novel --ai gemini    # Gemini CLI
novel init my-novel --ai codex     # Codex CLI
novel init my-novel --ai cursor    # Cursor
```

### 3. Start Writing

Use slash commands in your AI assistant:

```
/novel.constitution    # Claude Code format
/novel:constitution    # Gemini CLI format
/novel-constitution    # Codex CLI format
/constitution          # Other platforms' format
```

**Seven-Step Methodology Workflow**:
1. `/constitution` → 2. `/specify` → 3. `/clarify` →
4. `/plan` → 5. `/tasks` → 6. `/write` → 7. `/analyze`

> 📚 **Detailed Installation Instructions**: [docs/installation.md](docs/installation.md)
> 📖 **Complete Workflow**: [docs/workflow.md](docs/workflow.md)
> 🎯 **AI Platform Command Comparison**: [docs/ai-platform-commands.md](docs/ai-platform-commands.md) ⭐ **Must-read**

## 📦 Upgrade an Existing Project

```bash
# Upgrade to the latest version
npm install -g novel-writer-cn@latest
cd my-novel
novel upgrade

# Or specify an AI platform
novel upgrade --ai claude
```

> 📚 **Complete Upgrade Guide**: [docs/upgrade-guide.md](docs/upgrade-guide.md) - Includes version compatibility, migration instructions, and rollback methods.

## 📚 Slash Commands

### Namespace Explanation

| AI Platform | Command Format | Example |
|---|---|---|
| **Claude Code** | `/novel.command_name` | `/novel.write` |
| **Gemini CLI** | `/novel:command_name` | `/novel:write` |
| **Codex CLI** | `/novel-command_name` | `/novel-write` |
| **Other Platforms** | `/command_name` | `/write` |

> 💡 The table below uses the generic format. Please add the appropriate prefix based on your AI platform.
> 📖 **Detailed Command Comparison**: [docs/ai-platform-commands.md](docs/ai-platform-commands.md)

### Seven-Step Methodology

| Command | Description | When to Use |
|---|---|---|
| `/constitution` | Create a constitution | At the beginning of a project to define core writing principles |
| `/specify` | Define story specifications | Define story requirements like a PRD |
| `/clarify` | Clarify decisions | Clarify ambiguous points with 5 questions |
| `/plan` | Create a writing plan | Develop the chapter structure and technical plan |
| `/tasks` | Decompose tasks | Generate an executable task list |
| `/write` | Write chapters | Write based on the task list |
| `/analyze` | Comprehensive validation | Intelligent dual-mode: framework analysis/content analysis |

### Tracking and Validation

| Command | Description | When to Use |
|---|---|---|
| `/track-init` | Initialize tracking | First-time use (only once) |
| `/checklist` | Quality checklist ⭐ | Specification validation (before writing) + content scanning (after writing) |
| `/track` | Comprehensive tracking | After completing each chapter |
| `/plot-check` | Plot check | Periodically, every 5-10 chapters |
| `/timeline` | Timeline management | After important events |
| `/relations` | Relationship tracking | When character relationships change |
| `/world-check` | Worldview check | After introducing new settings |

> 📖 **Detailed Command Descriptions**: [docs/commands.md](docs/commands.md) - Includes detailed usage, parameters, and best practices for each command.

<details>
<summary>📁 Project Structure (Click to expand)</summary>

```
my-novel/
├── .specify/          # Spec Kit configuration
│   ├── memory/        # Writing memory (constitution.md, etc.)
│   └── scripts/       # Supporting scripts
├── .claude/           # Claude commands (or .cursor/.gemini, etc.)
│   └── commands/      # Slash command files
├── spec/              # Novel specification data
│   ├── tracking/      # Tracking data (plot-tracker.json, etc.)
│   └── knowledge/     # Knowledge base (world-setting.md, etc.)
├── stories/           # Story content
│   └── 001-story-name/
│       ├── specification.md    # Story specification
│       ├── creative-plan.md    # Creative plan
│       ├── tasks.md            # Task list
│       └── content/            # Chapter content
└── scripts/           # Supporting scripts
    ├── bash/          # Unix/Linux/Mac
    └── powershell/    # Windows
```

</details>

## 🤖 Supported AI Assistants

| AI Tool | Description | Status |
|---|---|---|
| **Claude Code** | Anthropic's AI assistant | ✅ Recommended |
| **Cursor** | AI code editor | ✅ Full support |
| **Gemini CLI** | Google's AI assistant | ✅ TOML format |
| **Windsurf** | Codeium's AI editor | ✅ Full support |
| **Roo Code** | AI programming assistant | ✅ Full support |
| **GitHub Copilot** | GitHub's AI programming assistant | ✅ Full support |
| **Qwen Code** | Alibaba's Tongyi Qianwen code assistant | ✅ TOML format |
| **OpenCode** | Open-source AI programming tool | ✅ Full support |
| **Codex CLI** | AI programming assistant | ✅ Full support |
| **Kilo Code** | AI programming tool | ✅ Full support |
| **Auggie CLI** | AI development assistant | ✅ Full support |
| **CodeBuddy** | AI programming partner | ✅ Full support |
| **Amazon Q Developer** | AWS's AI development assistant | ✅ Full support |

> 💡 Use `novel init --all` to generate configurations for all AI tools simultaneously.

## 🛠️ CLI Commands

<details>
<summary>Detailed Options (Click to expand)</summary>

### `novel init [name]`

```bash
novel init my-novel [options]
```

**Common Options**:
- `--here` - Initialize in the current directory
- `--ai <type>` - Select an AI platform (claude/gemini/cursor, etc.)
- `--with-experts` - Include expert mode
- `--plugins <names>` - Pre-install plugins (comma-separated)
- `--all` - Generate configurations for all AI platforms

### `novel plugins`

```bash
novel plugins list                # List installed plugins
novel plugins add <name>          # Install a plugin
novel plugins remove <name>       # Remove a plugin
```

### `novel upgrade`

```bash
novel upgrade [--ai <type>]       # Upgrade the project to the latest version
```

### `novel check`

```bash
novel check                       # Check project configuration and status
```

</details>

## 📖 Docs Overview

This project contains a comprehensive set of documents to help you get the most out of Novel Writer. Here’s a quick overview of the most important ones:

- **[Core Concepts](docs/README.md):** An overview of the key concepts and philosophies behind Novel Writer.
- **[Installation Guide](docs/installation.md):** Step-by-step instructions on how to install and set up the tool.
- **[Quickstart](docs/quickstart.md):** A fast-paced guide to get you started with your first novel.
- **[Workflow](docs/workflow.md):** A detailed explanation of the seven-step writing process.
- **[Commands Reference](docs/commands.md):** A complete reference for all the available slash commands.
- **[Best Practices](docs/best-practices.md):** Tips and tricks for using Novel Writer effectively.
- **[Upgrade Guide](docs/upgrade-guide.md):** Instructions on how to upgrade your project to the latest version.

## 📖 Documentation Index

### Core Documentation
- **[Command Reference](docs/commands.md)** - Detailed usage, parameters, and best practices for all slash commands
- **[Workflow](docs/workflow.md)** - A complete guide to the writing process
- **[Writing Methods](docs/writing-methods.md)** - Detailed explanations of 6 classic writing methods
- **[Best Practices](docs/best-practices.md)** - Practical experience and advanced techniques

### Advanced Documentation
- **[Practical Guide](docs/writing/practical-guide.md)** - Applying SDD based on real-world examples
- **[Upgrade Guide](docs/upgrade-guide.md)** - Version upgrade instructions and migration guide
- **[Installation Guide](docs/installation.md)** - Detailed installation steps
- **[Word Count Guide](docs/word-count-guide.md)** - Best practices for Chinese word count statistics

### Plugins and Extensions
- **Authentic Voice Plugin** - `novel plugins add authentic-voice`
  - Edit `.specify/memory/personal-voice.md` to configure your personal corpus
  - Use `/authentic-voice` to write and `/authenticity-audit` to self-check
- **Translation Plugin** - `novel plugins add translate`
- **Style Imitation Plugin** - Imitate the styles of authors like Lu Yao and Wang Yu

> 💡 Use `novel plugins list` to see all available plugins.

## 📈 Version History

Check out the complete changelog: **[CHANGELOG.md](CHANGELOG.md)**

**Latest Version Highlights**:
- v0.15.0 - Optimized command formats for multiple platforms
- v0.14.2 - Fixed Chinese word count statistics
- v0.12.2 - Enhanced layer for Claude Code
- v0.12.0 - Multi-clue management system
- v0.10.0 - Seven-step methodology system

## 🤝 Contributing

Issues and Pull Requests are welcome!

Project Address: [https://github.com/wordflowlab/novel-writer](https://github.com/wordflowlab/novel-writer)

## 📄 License

MIT License

## 🌐 Project Matrix

WordFlowLab explores multi-dimensional approaches to AI-assisted novel writing with a portfolio of open-source projects using different methodologies and tech stacks:

### Methodology Exploration Series

| Project | Methodology | Technical Features | Use Case |
|---|---|---|---|
| **[Novel-Writer](https://github.com/wordflowlab/novel-writer)** ⭐ | Spec-Kit | Parasitic slash commands, seven-step methodology | For users across multiple platforms, supports 13 AI tools |
| **[Article-Writer](https://github.com/wordflowlab/article-writer)** 🆕 | Spec-Kit | Nine-step writing process, workspace management | For writing articles for public accounts/social media, reducing AI tone |
| **[Novel-Writer-OpenSpec](https://github.com/wordflowlab/novel-writer-openspec)** | OpenSpec | Parasitic slash commands, separate spec management (specs/ + changes/) | Suitable for those who need OpenSpec standardized management |
| **[Novel-Writer-Skills](https://github.com/wordflowlab/novel-writer-skills)** | Spec-Kit + Agent Skills | Parasitic slash commands, supports Claude Code Agent Skills | Optimized for Claude Code |

### Tool Implementation Series

| Project | Type | Technical Foundation | Description |
|---|---|---|---|
| **[WriteFlow](https://github.com/wordflowlab/writeflow)** | CLI Tool | Mimics the Claude Code architecture | A standalone CLI designed for technical writers |
| **[NovelWeave](https://github.com/wordflowlab/novelweave)** | VSCode Extension | Fork: Cline → Roo Code → Kilo Code → NovelWeave | A visual novel editor, "Stardust Weaving" |

### Technical Evolution Path

```
Spec-Kit Methodology Branch:
  Novel-Writer (mainline) ──┬─→ Novel-Writer-Skills (Claude Code special edition)
                           └─→ WriteFlow (CLI standalone version)

OpenSpec Methodology Branch:
  Novel-Writer-OpenSpec (experimental version)

VSCode Extension Branch:
  Cline → Roo Code → Kilo Code → NovelWeave (novel-customized version)
```

### Recommendations

Choose the right tool based on your experience:

| User Type | Recommended Project | Reason |
|---|---|---|
| 🌟 **Newcomers** | [NovelWeave](https://github.com/wordflowlab/novelweave) | Visual editor, VSCode extension, easiest to get started |
| 💻 **Programming background<br>No novel writing experience** | [Novel-Writer](https://github.com/wordflowlab/novel-writer) <br> [Novel-Writer-Skills](https://github.com/wordflowlab/novel-writer-skills) | Seven-step methodology guides the writing process<br>Skills version is suitable for Claude Code users |
| 📚 **Programming background<br>Novel writing experience** | [Novel-Writer-OpenSpec](https://github.com/wordflowlab/novel-writer-openspec) | OpenSpec for standardized management<br>Suitable for systematic writing and team collaboration |
| 🚀 **Technical explorers<br>Can contribute PRs** | [WriteFlow](https://github.com/wordflowlab/writeflow) | Exploration in CLI tool development<br>Contributions and ideas are welcome |

**Quick Decision**:
- **Complete beginner** → NovelWeave (most visually friendly)
- **Using Claude Code** → Novel-Writer-Skills (deep integration with Agent Skills)
- **Across multiple AI tools** → Novel-Writer (supports 13 platforms)
- **Pursuing standardization** → Novel-Writer-OpenSpec (OpenSpec methodology)
- **Prefer the command line** → WriteFlow (pure CLI experience)

> 💡 **Open-source portfolio with multiple matrices and methodologies**: Exploring the different possibilities of AI writing. Feel free to choose the right tool for your needs!

## 🙏 Acknowledgements

This project is designed based on the [Spec Kit](https://github.com/sublayerapp/spec-kit) architecture. Special thanks for that!

---

**Novel Writer** - Let AI be your creative partner! ✨📚
