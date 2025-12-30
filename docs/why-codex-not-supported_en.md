# Novel Writer's Codex CLI Support

## 🎉 Latest Update (2025-10-25)

**Good news**: Novel Writer will **soon support** the OpenAI Codex CLI!

### Explanation of Changes

After researching the Codex implementation in the [Spec-Kit](https://github.com/github/spec-kit) project (v0.0.11+), we have found that **the Codex CLI can fully support Novel Writer's command system**, as long as a pure Markdown format (without YAML frontmatter) is used.

**Implementation Progress**:
- ✅ Technical solution confirmed
- 🚧 **In progress** (expected in v0.19.0)
- ⏳ Coming soon

**Recommended Solution**:
- For now, use [Claude Code](https://claude.ai), [Cursor](https://cursor.sh), or [Gemini CLI](https://ai.google.dev/gemini-api/docs/cli).
- Wait for the v0.19.0 release and upgrade to use the Codex CLI.

---

## ⚠️ Historical Reasons (v0.18.x and earlier)

The following content is preserved to document the history of design decisions:

Novel Writer v0.18.x and earlier **did not support** the OpenAI Codex CLI, mainly for the following reasons:

1.  **Limited Functionality of Codex Custom Prompts** - It only supports plain text prompts and cannot execute scripts or perform file operations.
2.  **High Complexity of Novel Writer Commands** - 93% of the commands (13/14) rely on scripts for state management and automated validation.
3.  **Waiting for Official Slash Commands** - We will adapt immediately once Codex supports full slash command functionality.

---

## 📊 Technical Comparison Analysis

### Codex Custom Prompts vs Novel Writer Commands

| Dimension | Codex Custom Prompts | Novel Writer Commands | Compatibility |
|---|---|---|---|
| **File Format** | Pure Markdown | Markdown + YAML Frontmatter | ⚠️ Partial |
| **Script Execution** | ❌ Not supported | ✅ Bash + PowerShell | ❌ Conflict |
| **File Operations** | ❌ Manual execution by AI | ✅ Automated checks and creation | ❌ Conflict |
| **State Management** | ❌ No structured return | ✅ JSON format state data | ❌ Conflict |
| **Parameter System** | ✅ `$1..$9`, `$ARGUMENTS` | ✅ `$ARGUMENTS`, `{SCRIPT}` | ✅ Compatible |
| **Location** | Global `~/.codex/prompts/` | Project-level `.claude/commands/` | ⚠️ Different |
| **Complexity** | Simple prompts | Complete workflow engine | ❌ Conflict |

### Key Difference: Script Execution Capability

The core limitation of **Codex Custom Prompts**:

```markdown
<!-- Example Codex prompt file -->
Help me check the story directory, count the number of chapters, and then decide whether to use framework analysis or content analysis.

Steps:
1. Find the stories/*/content/ directory.
2. Count the number of .md files.
3. If the number of chapters is < 3, use framework analysis.
4. Otherwise, use content analysis.
```

❌ **Problem**:
-   Completely relies on the AI's understanding and manual execution.
-   Cannot guarantee the accuracy and consistency of the execution.
-   Cannot return structured data for subsequent commands to use.

The implementation in **Novel Writer Commands**:

```yaml
---
description: Intelligent dual-mode analysis
scripts:
  sh: .specify/scripts/bash/check-analyze-stage.sh --json
  ps: .specify/scripts/powershell/check-analyze-stage.ps1 -Json
---
```

```bash
#!/bin/bash
# check-analyze-stage.sh - Returns structured analysis stage data

# Count the number of chapters
CHAPTER_COUNT=$(find "$CONTENT_DIR" -maxdepth 1 -type f -name "*.md" | wc -l)

# Check for files
HAS_SPEC=$([ -f "$STORY_DIR/specification.md" ] && echo true || echo false)

# Return JSON
echo "{\"type\": \"$ANALYZE_TYPE\", \"chapters\": $CHAPTER_COUNT, \"hasSpec\": $HAS_SPEC}"
```

✅ **Advantages**:
-   Automated execution, 100% accurate.
-   Returns structured data for the AI to use.
-   Automatic validation of preconditions.
-   Cross-platform compatibility (Bash + PowerShell).

---

## 🔍 Command System Dependency Analysis

### Statistics

Novel Writer has **14 core commands**:

-   ✅ **Pure prompt-based**: 1 (7%) - `expert`
-   ❌ **Script-dependent**: 13 (93%) - All other commands

### List of Script-Dependent Commands

| Command | Script Function | Can it be replaced with a pure prompt? |
|---|---|---|
| `/analyze` | Detects stage, counts chapters, returns JSON | ❌ Needs accurate counting |
| `/clarify` | Finds story files, extracts paths, JSON output | ❌ Needs path accuracy |
| `/constitution` | Checks file versions, migrates old files, initializes | ❌ Needs file operations |
| `/plan` | Validates specs, checks dependencies, creates directories | ❌ Needs state validation |
| `/plot-check` | Loads tracking data, validates nodes, compares outlines | ❌ Needs data parsing |
| `/relations` | Manages relationship matrix, updates JSON, validates consistency | ❌ Needs structured data |
| `/specify` | Creates spec files, validates templates, returns status | ❌ Needs file management |
| `/tasks` | Generates task lists, assigns priorities, JSON format | ❌ Needs structured output |
| `/timeline` | Parses time nodes, validates logic, visualizes data | ❌ Needs time calculation |
| `/track-init` | Initializes tracking system, creates JSON files | ❌ Needs data initialization |
| `/track` | Tracks progress, calculates completion rate, aggregates data | ❌ Needs multi-source data |
| `/world-check` | Validates setting consistency, compares with knowledge base | ❌ Needs multi-file comparison |
| `/write` | Checks writing state, loads context, validates dependencies | ❌ Needs state checking |

**Conclusion**: Only the `/expert` command (pure AI conversation) can be implemented with Codex prompts.

---

## 🏗️ Architectural Design Philosophy Differences

### Codex Custom Prompts: Lightweight Quick Prompts

**Design Goal**:
-   Quickly save frequently used prompts.
-   Reduce repetitive input.
-   A personal efficiency tool.

**Use Case**:
```markdown
<!-- ~/.codex/prompts/explain.md -->
Please explain in detail how the following code works:

$ARGUMENTS
```

**Characteristics**:
-   🎯 Simple and direct
-   ⚡ Quick to start
-   👤 Personalized

### Novel Writer Commands: Workflow Engine

**Design Goal**:
-   Systematic creation process.
-   Automated state management.
-   Cross-project consistency.

**Use Case**:
```yaml
# Complete workflow of the seven-step methodology
constitution → specify → clarify → plan → tasks → write → analyze
     ↓            ↓         ↓        ↓       ↓       ↓        ↓
  [Check]     [Validate]  [Q&A]   [Deps]  [Decompose] [State] [Multi-mode]
```

**Characteristics**:
-   🏛️ Structured process
-   🤖 Automated validation
-   📊 Data-driven

---

## 🚀 Possibility of Future Support

### Condition 1: Official Codex Support for Slash Commands

If OpenAI Codex introduces a slash command feature similar to Claude Code, supporting:

-   ✅ Script execution (like the `scripts` frontmatter)
-   ✅ File system operation tools
-   ✅ Structured data return (JSON)
-   ✅ Project-level command directories

**Commitment**: Novel Writer will adapt to it at the earliest opportunity.

### Condition 2: Codex Prompts Enhanced to a Command System

If the prompts feature in Codex is upgraded to support:

```toml
# TOML format similar to Gemini CLI
description = "Intelligent analysis command"

[scripts]
sh = ".specify/scripts/bash/check-analyze-stage.sh"

[tools]
file_operations = true
json_output = true
```

**Commitment**: We will immediately provide command files in the Codex format.

### Condition 3: Downgraded Support (Not Recommended)

**Solution**: Convert script logic into detailed AI instructions.

**Example**:
```markdown
<!-- analyze.md - Downgraded version for Codex -->
Please perform the following steps:

1. Use the tool to check the stories/*/content/ directory.
2. Count the number of .md files, ignoring README.md and index.md.
3. Check if specification.md, creative-plan.md, and tasks.md exist.
4. Decide the analysis type based on the following logic:
   - Number of chapters = 0 → Framework analysis
   - Number of chapters < 3 → Framework analysis (suggest continuing to write)
   - Number of chapters ≥ 3 → Content analysis
...
```

**Why it's not recommended**:
-   ❌ Reliability is greatly reduced (depends on AI understanding).
-   ❌ Consistency cannot be guaranteed (each execution may differ).
-   ❌ Cannot handle complex edge cases.
-   ❌ Loses automated validation capabilities.
-   ❌ User experience is severely degraded.

---

## ✅ Recommended AI Tools

Novel Writer **fully supports** the following AI tools, all of which support the complete command system:

### 1. Claude Code (Recommended)

**Features**:
-   ✅ Native support for Markdown + YAML frontmatter
-   ✅ Strong context understanding
-   ✅ Excellent Chinese processing
-   ✅ Project-level command directory `.claude/commands/`

**Installation**:
```bash
novel init my-novel --ai claude
```

**Use Case**: All users, especially for Chinese novel writing.

---

### 2. Cursor

**Features**:
-   ✅ AI code editor, integrating writing and editing
-   ✅ Full support for slash commands
-   ✅ Real-time preview and editing
-   ✅ Command directory `.cursor/commands/`

**Installation**:
```bash
novel init my-novel --ai cursor
```

**Use Case**: Users who prefer an integrated development environment.

---

### 3. Google Gemini CLI

**Features**:
-   ✅ Google's latest AI model
-   ✅ TOML format command system
-   ✅ Novel Writer provides a complete TOML conversion
-   ✅ Command directory `.gemini/commands/` (with namespaces)

**Installation**:
```bash
novel init my-novel --ai gemini
```

**Use Case**: Users in the Google ecosystem who prefer the TOML format.

---

### 4. Multi-platform Support (Recommended)

**Initialize once, support all platforms**:
```bash
novel init my-novel --all
```

Generates command configurations for all AI tools:
-   `.claude/commands/` - Claude Code
-   `.cursor/commands/` - Cursor
-   `.gemini/commands/` - Gemini CLI
-   `.windsurf/workflows/` - Windsurf

**Use Case**: Team collaboration, switching between multiple tools.

---

## ❓ FAQ

### Q1: Are Codex prompts completely unusable?

**A**: They can be used for **simple prompts**, for example:

```markdown
<!-- ~/.codex/prompts/brainstorm.md -->
Help me quickly brainstorm 5 creative angles on "$ARGUMENTS".
Each angle should be no more than 2 sentences.
```

But they **cannot be used for Novel Writer's core workflow commands** (constitution, specify, write, etc.).

---

### Q2: Why do other tools work, but not Codex?

**A**: The feature support is different:

| Feature | Codex Prompts | Claude/Cursor | Gemini CLI |
|---|---|---|---|
| Plain text prompts | ✅ | ✅ | ✅ |
| YAML Frontmatter | ❌ | ✅ | ❌ (uses TOML) |
| Script execution | ❌ | ✅ | ✅ |
| Tool calls | Limited | Powerful | Powerful |
| Project-level config | ❌ | ✅ | ✅ |

Codex's Custom Prompts are positioned as **personal quick prompts**, not a **project-level command system**.

---

### Q3: Will it be supported in the future?

**A**: Yes, provided that:

1.  **Official support**: Codex introduces slash commands or enhances the prompts feature.
2.  **Community demand**: There are enough users who need Codex support.
3.  **Technical feasibility**: It does not compromise the experience and functionality of existing users.

We will continue to monitor updates to Codex and will **adapt to it at the earliest opportunity** once the conditions are met.

---

### Q4: I really want to use Codex, what should I do?

**A**: You can try the following solutions:

**Solution 1**: Manually copy pure prompt commands.
```bash
# Copy the expert command to Codex prompts
cp .claude/commands/expert.md ~/.codex/prompts/novel-expert.md
# Use /novel-expert to call it
```

**Solution 2**: Write your own lightweight prompts.
```markdown
<!-- ~/.codex/prompts/novel-brainstorm.md -->
Based on a novel writing scenario, help me brainstorm: $ARGUMENTS
Reference file: .specify/memory/writing-constitution.md
```

**Solution 3**: Use a hybrid approach.
-   **Codex**: For quick conversations, brainstorming, and exploratory writing.
-   **Claude/Cursor**: For the core workflow (constitution → write → analyze).

---

### Q5: What are the advantages of Codex?

**A**: The advantages of Codex are:

-   ⚡ Lightweight and quick to start
-   🌍 Globally available (not project-specific)
-   👤 Personalized prompt management
-   🆓 Potential cost advantages

However, these advantages **do not align with Novel Writer's design goals**:

-   Novel Writer focuses on a **project-level creation process**.
-   It requires **state persistence across sessions**.
-   It emphasizes **automated validation and data management**.

---

## 📚 Related Resources

-   [OpenAI Codex Documentation](https://developers.openai.com/codex/)
-   [Codex Custom Prompts Guide](https://github.com/openai/codex/blob/main/docs/prompts.md)
-   [Novel Writer Installation Guide](installation.md)
-   [Gemini Command Development Guide](gemini-command-guide.md)
-   [Novel Writer Quick Start](quickstart.md)

---

## 💬 Feedback and Discussion

If you have a strong need for Codex support or other suggestions, feel free to:

-   📧 Submit a [GitHub Issue](https://github.com/wordflowlab/novel-writer/issues)
-   💬 Join the community discussion
-   🤝 Contribute code or documentation

We will adjust our development priorities based on community feedback.

---

**Last updated**: 2025-10-04
**Version**: v0.12.1
