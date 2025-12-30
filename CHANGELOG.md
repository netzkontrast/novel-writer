# Changelog

## [0.19.0] - 2025-10-25

### ✨ New Features

#### 🎉 Codex CLI Support
- **New Platform**: Full support for OpenAI Codex CLI
  - Command format: `/novel-command-name` (e.g., `/novel-write`)
  - Command directory: `.codex/prompts/`
  - Use `novel-` prefix to avoid naming conflicts
  - Pure Markdown format (no YAML frontmatter)
  - All 13 core commands supported
- **Installation**: `novel init my-novel --ai codex`
- **Technical Implementation**: Based on [Spec-Kit](https://github.com/github/spec-kit) v0.0.11+ implementation

#### 📚 AI Platform Command Comparison Document
- **New Document**: `docs/ai-platform-commands.md` - A complete command comparison guide for 13 AI platforms
  - Quick reference table: At-a-glance differences in command formats
  - Namespace rules: Detailed explanation of why different prefixes are used
  - Platform details: Complete command lists for Gemini, Claude, Codex, etc.
  - Usage examples: Full workflow demonstrations for the three main platforms
  - FAQ: Solutions for common issues like commands not working, format differences, etc.

### 📝 Documentation Updates

#### Codex CLI Support Notes
- Updated `docs/why-codex-not-supported.md`:
  - Title changed to "Codex CLI Support in Novel Writer"
  - Changed "Coming soon" to "Supported in v0.19.0"
  - Kept historical reasons as a record of design decisions

#### README Update
- Mentioned Codex CLI in core features
- Added `--ai codex` option to initialization examples
- Added `/novel-constitution` (Codex format) to command format examples
- Added Codex CLI to the namespace explanation table

#### GEMINI.md Template Update
- Clearly stated that Gemini CLI uses the `novel:` namespace
- Added an explanation for the namespace reason
- Updated all example commands with the correct `novel:` prefix
- Added cross-reference links to documentation

### 🔧 Build System Improvements

#### Namespace Support
- Modified `scripts/build/generate-commands.sh`:
  - Codex CLI uses `novel-` prefix
  - Generates pure Markdown format (no frontmatter)
  - Command files are located in the `.codex/prompts/` directory

### 📊 Supported AI Platforms

Now supporting **13 AI platforms**:

| Platform | Command Format | Namespace |
|---|---|---|
| Claude Code | `/novel.command-name` | `novel.` |
| Gemini CLI | `/novel:command-name` | `novel:` |
| **Codex CLI** ⭐ | **`/novel-command-name`** | **`novel-`** |
| Cursor | `/command-name` | None |
| Windsurf | `/command-name` | None |
| Roo Code | `/command-name` | None |
| GitHub Copilot | `/command-name` | None |
| Qwen Code | `/command-name` | None |
| OpenCode | `/command-name` | None |
| Kilo Code | `/command-name` | None |
| Auggie CLI | `/command-name` | None |
| CodeBuddy | `/command-name` | None |
| Amazon Q | `/command-name` | None |

### 🎯 User Experience Improvements

- Users can easily look up the corresponding command format for their AI platform
- Detailed documentation avoids confusion about commands not working
- Codex CLI users can now use the full functionality of Novel Writer

### 📖 Related Documents

- [AI Platform Command Usage Guide](docs/ai-platform-commands.md) ⭐ Must-read
- [Codex CLI Support Notes](docs/why-codex-not-supported.md)
- [Gemini Command Development Guide](docs/gemini-command-guide.md)

---

## [0.18.5] - 2025-10-24

### 🐛 Bug Fixes

#### Gemini Constitution Save Path Error (#6)
- **Issue**: After running the `/constitution` command in Gemini, the constitution file was incorrectly saved to `memory/constitution.md` (project root) instead of the correct `.specify/memory/constitution.md` path.
- **Reason**: Inconsistent path references in the source template file `templates/commands/constitution.md` and other command files, some of which used paths without the `.specify/` prefix.
- **Fix**: Standardized all path references in command template files to use the full path `.specify/memory/constitution.md`.
  - Modified 3 path references in `templates/commands/constitution.md`
  - Modified 1 path reference in `templates/commands/specify.md`
  - Modified 1 path reference in `templates/commands/plan.md`
  - Modified 3 path references in `templates/commands/write.md`
  - Rebuilt command files for all platforms.
- **Impact**: Platforms using TOML format, like Gemini and Qwen, will now correctly save the constitution file to `.specify/memory/constitution.md`.

### 📝 Scope of Impact
- `templates/commands/constitution.md` - Path references have been standardized.
- `templates/commands/specify.md` - Path references have been standardized.
- `templates/commands/plan.md` - Path references have been standardized.
- `templates/commands/write.md` - Path references have been standardized.
- `dist/gemini/.gemini/commands/novel/*.toml` - All TOML files have been regenerated.
- Build artifacts for all platforms have been updated.

### 🎯 User Experience Improvements
- For Gemini users, after running the `/constitution` command, the file will be correctly saved to `.specify/memory/constitution.md`.
- Path standardization avoids path confusion for the AI between different commands.
- The incorrect `memory/` directory will no longer appear in the project root.

---

## [0.18.4] - 2025-10-15

### 🐛 Bug Fixes

#### Constitution File Naming Standardization
- **Issue**: The system had 3 different constitution file names (novel-constitution.md, writing-constitution.md, constitution.md), causing multiple constitution files in user projects.
- **Fix**: Standardized the constitution file name to `constitution.md`.
  - Renamed source file: `memory/writing-constitution.md` → `memory/constitution.md`
  - Modified file path references in all Bash scripts (6 files)
  - Modified file path references in all PowerShell scripts (5+ files)
  - Modified file references in all command templates (constitution.md, specify.md, plan.md, analyze.md, write.md)
  - Updated path permissions in allowed-tools.

#### Script Path Duplication Issue
- **Issue**: The `rewrite_paths()` function in the build system repeatedly added the `.specify/` prefix, causing incorrect paths (`.specify.specify/scripts/`).
- **Fix**: Used a temporary marker to protect existing `.specify/` paths.
  - Modified the `rewrite_paths()` function in `scripts/build/generate-commands.sh`.
  - First, mark existing correct paths, then add the prefix, and finally restore the markers.
  - All paths in the generated command files are now correctly formatted as `.specify/scripts/...`.

### 📝 Scope of Impact
- `memory/constitution.md` - Standardized constitution file name.
- `scripts/bash/*.sh` - All scripts referencing the constitution file have been updated.
- `scripts/powershell/*.ps1` - All scripts referencing the constitution file have been updated.
- `templates/commands/*.md` - All command templates have been updated.
- `scripts/build/generate-commands.sh` - The path rewriting function has been fixed.
- `dist/` - Rebuilt command files for all platforms.

### 🎯 User Experience Improvements
- User projects' `.specify/memory/` directory will now only have one `constitution.md` file.
- All script command paths are correct, and the `.specify.specify/` error no longer occurs.
- The naming is more concise, clear, and easier to understand and use.

---

## [0.18.3] - 2025-10-15

### ✨ Feature Improvements

#### Plugin Installation System Standardization
- **Issue**: The genre-knowledge plugin used a manual installation method, inconsistent with other plugins.
- **Improvement**: Standardized to use the `novel plugins:add` command for installation.
  - Added `plugins/genre-knowledge/config.yaml` configuration file.
  - Complete plugin metadata definition (name, version, description, type, dependencies).
  - Detailed usage instructions and steps are displayed after installation.

#### Documentation Updates
- Updated `plugins/genre-knowledge/README.md`:
  - Changed installation method to `novel plugins:add genre-knowledge`.
  - Changed uninstallation method to `novel plugins:remove genre-knowledge`.
  - Added `novel plugins:list` command to verify installation.
  - Simplified document structure to highlight the installation process.

#### CLI Enhancements
- Updated the list of available plugins to include genre-knowledge:
  - `plugins:list` command prompt message.
  - `plugins:add` command error message.
- Maintained a consistent user experience with other plugins (translate, authentic-voice, book-analysis).

### 🧪 Test Verification
- ✅ Plugin installation process tested successfully.
- ✅ Plugin list displays correctly.
- ✅ Enhanced command files are copied successfully.
- ✅ Detailed usage instructions are displayed after installation.

### 📝 Scope of Impact
- `plugins/genre-knowledge/config.yaml` - New configuration file added.
- `plugins/genre-knowledge/README.md` - Installation instructions updated.
- `src/cli.ts` - Added genre-knowledge to the list of available plugins.

---

## [0.18.2] - 2025-10-15

### 🐛 Bug Fixes

#### Missing Plugin Command Files
- **Issue**: The `commands/` directory of the genre-knowledge plugin was empty, preventing users from getting enhanced prompts.
- **Fix**: Added 3 missing enhancement command files:
  - `commands/clarify-enhance.md` (18 lines) - Genre knowledge enhancement prompt for the clarify command.
  - `commands/plan-enhance.md` (62 lines) - Dynamic genre knowledge loading prompt for the plan command.
  - `commands/write-enhance.md` (15 lines) - Genre style application prompt for the write command.

#### User Experience Improvements
- Users can now directly copy enhancement prompts from the `commands/*.md` files.
- Pasting them at the `PLUGIN_HOOK` marker in the core commands enables the plugin functionality.
- The structure is clearer and aligns with the plugin architecture design.

### 📝 Scope of Impact
- `plugins/genre-knowledge/commands/` - 3 new command files added.
- Plugin system - The installation experience has been improved.

---

## [0.18.1] - 2025-10-15

### 🏗️ Architectural Optimization

#### Genre Knowledge Pluginization
- **Migrated Genre Knowledge Files**: Moved 5 genre knowledge files from `spec/knowledge/genres/` to `plugins/genre-knowledge/knowledge/genres/`.
  - `fantasy.md` (669 lines) - Fantasy/Xuanhuan genre guide.
  - `scifi.md` (530 lines) - Sci-Fi genre guide.
  - `romance.md` (378 lines) - Romance genre guide.
  - `mystery.md` (353 lines) - Mystery/Suspense genre guide.
  - `shuangwen.md` (236 lines) - Shuangwen (face-slapping) genre guide.

#### True Optional Plugin Architecture
- **Core Command Optimization**:
  - Removed hard-coded plugin dependencies from core commands.
  - Added `plugins/**` wildcard permission to support all plugins.
  - Retained the `<!-- PLUGIN_HOOK -->` marker for users to manually enable plugins.
- **User Experience Improvements**:
  - After installing a plugin, users only need to copy and paste the enhancement prompt to the PLUGIN_HOOK marker.
  - No need to modify allowed-tools (already has `plugins/**` permission).
  - Plugin functionality is completely optional and does not affect core functions.

#### Design Philosophy
- ✅ **Clear Responsibilities**: `spec/knowledge/` focuses on user-created project knowledge, while plugins focus on optional system-provided features.
- ✅ **Optional Installation**: Users who do not need genre knowledge do not need to load the plugin.
- ✅ **Single Source of Truth**: Genre knowledge exists only in the plugin, avoiding duplication and confusion.
- ✅ **Simple Architecture**: No technical debt, no backward compatibility code.

### 📝 Scope of Impact
- `spec/knowledge/genres/` - Deleted.
- `plugins/genre-knowledge/` - Contains 7 knowledge files.
- `templates/commands/clarify.md` - Removed plugin hard-coding, added `plugins/**` permission.
- `templates/commands/plan.md` - Removed plugin hard-coding, added `plugins/**` permission.
- `templates/commands/analyze.md` - Removed plugin hard-coding, added `plugins/**` permission.

---

## [0.15.0] - 2025-10-11

### ✨ Major Improvement: Multi-Platform Command Format Optimization

#### Background
The previous build system copied Claude-specific YAML frontmatter fields (`allowed-tools`, `model`, `disable-model-invocation`) to all 13 AI platforms, but these fields are not supported or needed on other platforms, causing compatibility issues.

#### Core Fix
- **Platform-Specific Format Generation**: Generate command files in the correct format based on the actual support of each AI platform.
- **Format Classification System**:
  - **Pure Markdown (no frontmatter)**: Cursor, GitHub Copilot, Codex CLI, Auggie CLI, CodeBuddy, Amazon Q Developer
  - **Minimal frontmatter (description only)**: OpenCode
  - **Partial frontmatter (description + argument-hint)**: Roo Code, Windsurf, Kilo Code
  - **Full frontmatter (all fields)**: Claude Code
  - **TOML format (description + prompt)**: Gemini CLI, Qwen Code

#### Technical Implementation
- **Build Script Enhancement** (`scripts/build/generate-commands.sh`):
  - Added `frontmatter_type` parameter to the `generate_commands` function.
  - Implemented 4 frontmatter generation strategies (none/minimal/partial/full).
  - Specified the correct format type for all 13 platforms.
  - Extracted the `argument_hint` field to support partial frontmatter.

- **TOML Format Fix**:
  - Gemini and Qwen's TOML files now only contain `description` and `prompt` fields.
  - Argument placeholders correctly use `{{args}}` instead of `$ARGUMENTS`.
  - Removed unsupported metadata fields.

#### Verification Results
✅ Command file formats for all 13 AI platforms have been verified.
✅ Each platform only includes the fields it supports, complying with official documentation.
✅ Improved compatibility across platforms and reduced file redundancy.
✅ Avoided potential parsing errors.

#### Scope of Impact
- 📦 **Build System**: `npm run build:commands` now generates command files in the correct format.
- 🎯 **13 Platforms**: Claude, Gemini, Cursor, Windsurf, Roo Code, GitHub Copilot, Qwen Code, OpenCode, Codex CLI, Kilo Code, Auggie CLI, CodeBuddy, Amazon Q Developer
- 📁 **182 Files**: 14 command files for each platform, all in the correct format.

### 📚 Documentation
Thanks to community feedback for helping us discover and fix multi-platform compatibility issues.

## [0.14.2] - 2025-10-10

### 🐛 Bug Fixes

- **Chinese Word Count Issue**: Fixed the issue where `wc -w` was highly inaccurate for counting Chinese words.
  - Added `count_chinese_words()` function, improving accuracy by 12+ times.
  - Excludes Markdown markers, code blocks, spaces, and punctuation.
  - Counts only the actual text content.
  - Excellent performance (processing 3000 words in about 10ms).

### ✨ New Features

- **Word Count Functions** (`scripts/bash/common.sh`)
  - `count_chinese_words()` - Accurate Chinese word count.
  - `show_word_count_info()` - Displays user-friendly word count verification information.

- **Script Enhancements**
  - `analyze-story.sh` - Displays detailed word count statistics for each chapter.
  - `check-writing-state.sh` - Automatically verifies if chapter word counts meet the requirements.
  - Reads word count requirements from `validation-rules.json`.

- **Command Template Updates**
  - `/write` command now includes word count verification instructions.
  - Warns against using `wc -w` for Chinese.
  - Automatically displays the accurate word count after AI writing is complete.

### 📚 New Documentation

- **User Guide**: `docs/word-count-guide.md` - Complete guide to using the word count feature.
- **Test Script**: `scripts/bash/test-word-count.sh` - Verifies the accuracy of the word count.
- **Fix Explanation**: `WORD_COUNT_FIX.md` - Issue diagnosis and solution.

### 🎯 Problems Solved

- AI writing prompts "word count not enough" when the actual word count has exceeded the requirement.
- Using `wc -w` to count Chinese chapters results in severely low counts (121/164 vs 2000+).
- Inconsistent results when counting the same file multiple times.

### ⚠️ Important Reminder

- ❌ Do not use `wc -w` to count Chinese words (highly inaccurate).
- ❌ Do not use `wc -m` to count words (includes too many irrelevant characters).
- ✅ Use the `count_chinese_words` function for accurate results.

## [0.14.0] - 2025-10-09

### ✨ New Features

- **Roo Code Slash Command Support**: `novel init` and `novel upgrade` now support generating the `.roo/commands` directory and automatically outputting Roo Code compatible Markdown commands.
- **Plugin System Integration**: The plugin command injection process has been extended to Roo Code, ensuring that installed plugins are immediately available in Roo Code.

### 📚 Documentation Updates

- README and CHANGELOG now include Roo Code support information and an updated list of available AI platforms.

## [0.13.7] - 2025-10-06

### 🐛 Bug Fixes

- **Plugin Command File Naming Optimization**: Fixed the issue of overly complex command file names after plugin installation.
  - Removed the unnecessary `plugin-{pluginName}-` prefix.
  - Simplified plugin command file names: `plugin-book-analysis-book-analyze.md` → `book-analyze.md`.
  - Maintained a naming style consistent with core commands.
  - Applies to all AI platforms (Claude, Cursor, Windsurf, Gemini).

## [0.13.6] - 2025-10-06

### 🐛 Bug Fixes

- **CLI Help Text Update**: Fixed the help text displayed after `novel init`.
  - Updated the core command list to the correct seven-step methodology commands (constitution, specify, clarify, plan, tasks, write, analyze).
  - Removed deprecated old commands (method, style, story, outline, chapters).
  - Updated the recommended workflow to: `constitution → specify → clarify → plan → tasks → write → analyze`.

## [0.12.2] - 2025-10-04

### ✨ New Feature: Claude Code Enhanced Layer

#### Core Improvement
Provide an exclusive enhanced version of commands for **Claude Code** users while **maintaining full compatibility with other platforms (Gemini, Cursor, Windsurf)**.

#### 1. Build System Design (Upgraded in v0.15.0+ to single-source + build system)
- **Single Source**: `templates/commands/` - Command source files (formerly `commands-claude/`).
- **Build System**: `scripts/build/generate-commands.sh` - Automatically generates commands for all platforms.
- **Namespace**: Claude uses `novel.*` prefix, Gemini uses `novel/` subdirectory to avoid conflicts with spec-kit.
- **Release Process**: The `dist/` directory is automatically generated during the build, and users can directly copy from it during initialization.

#### 2. Claude Code Exclusive Features

**Enhanced Frontmatter Fields**:
- `argument-hint` - Command argument auto-completion hints.
- `allowed-tools` - Fine-grained tool permission control (e.g., `Bash(find:*)`, `Read(//**)`).
- `model` - Specify the most suitable AI model for each command (default `claude-sonnet-4-5-20250929`).
- `disable-model-invocation` - Controls whether the SlashCommand tool can be automatically invoked.

**Dynamic Context Loading**:
- Supports inline bash execution: `!`command``.
- Real-time project status retrieval (chapter count, word count, tracking files, etc.).
- Reduces manual user input and enhances command intelligence.

#### 3. Enhanced Command List

**P0 Commands (3)**:
- `/analyze` - Added phase detection, chapter list, and word count dynamic context.
- `/write` - Added to-do tasks, latest chapter, and progress status dynamic loading.
- `/clarify` - Added story file path and spec detection dynamic context.

**P1 Commands (3)**:
- `/track` - Added tracking file status, progress statistics, chapter list, and word count.
- `/specify` - Added constitution detection, spec file detection, and path information.
- `/plan` - Added spec status, plan file detection, and items to be clarified statistics.

**P2 Commands (5)**:
- `/tasks` - Added plan/spec file detection and clue management spec summary.
- `/plot-check` - Added tracking file status, progress detection, and chapter statistics.
- `/timeline` - Added timeline status, time node statistics, and chapter mapping.
- `/relations` - Added relationship network status and character/faction statistics.
- `/world-check` - Added knowledge base detection, setting statistics, and proper noun statistics.

#### 4. CLI Logic Optimization

Modified `src/cli.ts` to support priority selection:
```typescript
// When generating commands for Claude, prioritize the enhanced version
if (await fs.pathExists(claudeEnhancedPath)) {
  commandContent = await fs.readFile(claudeEnhancedPath, 'utf-8');
  console.log(chalk.gray(`    💎 Claude Enhanced: ${file}`));
}
```

#### 5. Compatibility Guarantee
- ✅ Does not modify command directories of other platforms (`.claude`, `.cursor`, `.gemini`, etc.).
- ✅ Basic commands remain unchanged, ensuring normal use for Gemini/Cursor/Windsurf.
- ✅ The Claude enhancement layer is optional and does not affect existing users.
- ✅ All enhancement features are only effective in the Claude Code environment.

### 📚 Documentation Updates
- **README.md**: Added v0.12.2 Claude Code enhancement layer feature description.
- **CHANGELOG.md**: Detailed record of enhancement features and implementation details.

### 🎯 Design Philosophy
**Enhance without breaking compatibility**:
- ❌ Do not create new commands or new platform-specific commands.
- ✅ Layered architecture with priority selection.
- ✅ Claude users get the best experience.
- ✅ The user experience of other platforms is not affected.

---

## [0.12.1] - 2025-10-01

### ✨ New Feature: Smart Dual-Mode Analyze

#### Core Improvement
The `/analyze` command has been upgraded to a **smart dual-mode** that automatically selects the analysis type based on the creation stage, **without adding new commands**.

#### 1. Smart Stage Detection
- **Automatic Judgment**: The system detects the number of chapters and automatically decides whether to perform a framework analysis or a content analysis.
- **Manual Specification**: Supports `--type=framework` or `--type=content` to force a specific mode.
- **Script Support**: Added `scripts/bash/check-analyze-stage.sh` and `scripts/powershell/check-analyze-stage.ps1`.

#### 2. Mode A: Framework Consistency Analysis (before writing)
- **Coverage Analysis**: Checks if all specification requirements have corresponding plans and tasks.
- **Consistency Check**: Verifies if there are contradictions between specifications, plans, and tasks.
- **Logic Warning**: Analyzes potential logical loopholes in the storyline design.
- **Readiness Assessment**: Evaluates if writing can begin.

#### 3. Mode B: Content Quality Analysis (after writing)
- **Constitution Compliance**: Verifies if the work complies with the creation principles.
- **Specification Compliance**: Checks if the implementation meets the specification requirements.
- **Content Quality**: Analyzes logic, characters, rhythm, etc.
- **Improvement Suggestions**: Provides specific P0/P1/P2 fix suggestions.

#### 4. Decision Logic
```
Chapter count = 0     → Framework analysis
Chapter count < 3     → Framework analysis (suggests continuing to write)
Chapter count ≥ 3     → Content analysis
User specifies --type → Forces the specified mode
```

### 📚 Documentation Updates
- **README.md**: Updated the `/analyze` command description to showcase the smart dual-mode.
- **docs/writing/analyze-placement-rationale.md**: Added "Appendix: Smart Dual-Mode Design" section.
- **Command Template**: Completely rewrote the analyze command to detail the two analysis modes.

### 🎯 Design Philosophy
**Restrained but not simple**:
- ❌ Do not create two commands (`/framework-analyze`, `/content-analyze`).
- ✅ One `/analyze` command that intelligently judges the scenario.
- ✅ 90% automatic processing, 10% manual control.
- ✅ Meets multiple needs while keeping the command simple.

### 💡 Community Feedback Driven
Thanks to @ZengXishengAnson for the request for both "framework analysis before writing" and "content review after writing."
Through intelligent design, we have met both needs without increasing the number of commands.

---

## [0.12.0] - 2025-09-30

### ✨ New Feature: Multi-Clue Management System

#### Core Improvement
Achieved complete multi-clue management capabilities by enhancing existing command templates, **without adding new commands**.

#### 1. specification.md Enhancement (/specify command)
Added **Chapter 5: Clue Management Specification**, which includes 5 management tables:
- **5.1 Clue Definition Table**: Defines all clues' ID, type, priority, and conflicts.
- **5.2 Clue Pacing Plan**: Plans the activity level of each clue in different volumes (⭐⭐⭐/⭐⭐/⭐).
- **5.3 Clue Intersection Plan**: Pre-plans clue intersection moments to avoid random AI improvisation.
- **5.4 Foreshadowing Management Table**: Manages the placement and reveal of foreshadowing to ensure nothing is missed.
- **5.5 Clue Modification Decision Matrix**: An impact assessment checklist for modifying clues.

#### 2. creative-plan.md Enhancement (/plan command)
Added "Active Clues" and "Intersection Point" columns to the chapter section table:
- Marks which clues are advanced in each chapter section.
- ⭐⭐⭐ Main advancement / ⭐⭐ Auxiliary / ⭐ Background.
- Clearly indicates the chapter where an intersection point occurs.

#### 3. tasks.md Enhancement (/tasks command)
Added clue-related fields to each writing task:
- **Involved Clues**: Which clues are advanced in this chapter and their priority.
- **Intersection Point**: Whether this chapter is an intersection point.
- **Foreshadowing Placement/Reveal**: Foreshadowing operations involved in this chapter.

#### 4. plot-tracker.json Enhancement (/track-init command)
`/track --init` automatically reads from Chapter 5 of specification.md:
- All clue definitions (from section 5.1).
- All intersection points (from section 5.3).
- All foreshadowing (from section 5.4).
- Generates a complete tracking data structure.

#### 5. Practical Guide Update (docs/writing/practical-guide.md)
Added **Chapter 6: Multi-Clue Management Guide**, which includes:
- Real problem scenarios (from user feedback).
- A 4-step solution.
- A complete usage example based on "Return to 1984."
- A comparison table of how the three major pain points are solved.

### 🎯 Core Problems Solved
Real confusion from users:
> "It's hard to explain to the AI how to keep the main and subplots parallel, and how to intersect and reveal previous clues at the right time. It's a disaster, especially when the plot setting is constantly changing."

#### Three Major Pain Points and Their Solutions
| Pain Point | Solution | Specific File |
|---|---|---|
| **Parallel Advancement** | Mark "Involved Clues" in each chapter of tasks.md | W040 marked with PL-01⭐⭐⭐, PL-02⭐⭐ |
| **Intersection Timing** | Pre-plan in section 5.3 of specification.md | X-001 set for chapter 40 to avoid AI randomness |
| **Modification Consistency** | 5.5 Modification Decision Matrix + `/track --check` | Automatically prompts the scope of impact when modifying PL-02 |

### 📐 Design Principles
- ✅ **Follows the "if not necessary, do not add" principle**: Completely uses the existing 7 commands.
- ✅ **Complies with the SDD methodology**: Clue management is distributed across specify→plan→tasks→track.
- ✅ **Supported by writing theory**: Concepts from Story Grid's Grid Spreadsheet and Save the Cat's B Story.
- ✅ **Solves real pain points**: Based on actual user needs, not imagined features.

### 📝 Documentation Improvements
- Detailed usage examples (based on 5 clues from "Return to 1984").
- Complete input prompt templates.
- Impact assessment and consistency verification process.

## [0.11.0] - 2025-09-30

### ✨ New Features
- **SDD Methodology Practical Guide**: Added `docs/writing/practical-guide.md` (approx. 10,000 words).
  - A complete practical case of SDD based on the novel "Return to 1984."
  - Detailed explanation of the layered recursive application of SDD (whole book/a volume/chapter section/single chapter).
  - Provides actual input prompt examples for 4 complete scenarios.
  - Added comparison of good and bad prompts.
  - Added a complete dialogue flow demonstration.
  - Answers practical questions like "how to update the outline if the AI deviates."

- **Visual Diagrams**: Added 3 SVG diagrams to aid understanding.
  - `sdd-levels.svg` - SDD layered recursion diagram.
  - `sdd-flow.svg` - SDD complete cycle flow chart.
  - `prompt-structure.svg` - Structure of a good prompt.

### 📝 Documentation Improvements
- Emphasizes the core of SDD: specification-driven + layered recursion + allowing deviation + frequent verification.
- Each scenario includes:
  - ❌ Bad prompt examples.
  - ✅ Good prompt examples.
  - 💬 Complete dialogue flow (User→AI→Confirm→Complete).
- Provides a prompt structure template (Situation description/Modification intent/What needs updating/Expected output).

### 🎯 Problems Solved
- How to adjust the plot direction mid-writing.
- How to handle excellent deviations produced by the AI.
- What command combinations to use for different granularities of modification.
- How to write prompts that the AI can understand.

## [0.10.5] - 2025-09-30

### 🐛 Bug Fixes
- **Missing function in common.sh**: Added the `get_active_story()` function.
  - Fixed the "get_active_story: command not found" error during script execution.
  - Synced to `.specify/scripts/bash/` and `scripts/bash/`.

### 📝 Scope of Impact
The following scripts can now run correctly after the fix:
- `check-writing-state.sh`
- `plan-story.sh`
- `tasks-story.sh`
- `analyze-story.sh`

## [0.10.4] - 2025-09-30

### 🐛 Bug Fixes
- **Missing scripts for the seven-step methodology**: Completed Bash script support.
  - Created `plan-story.sh` - creative plan script.
  - Created `tasks-story.sh` - task decomposition script.
  - Copied `analyze-story.sh` - comprehensive validation script.
  - Copied `constitution.sh` - creation constitution script.
  - Copied `specify-story.sh` - story specification script.

### 📝 File Updates
- Updated the script reference in the `/tasks` command template from `generate-tasks.sh` to `tasks-story.sh`.
- Synced all scripts to `.specify/scripts/bash/` and `scripts/bash/`.
- Synced command templates to `.claude/commands/`.

### 🔧 Scope of Impact
After the fix, all seven-step methodology commands (`/constitution`, `/specify`, `/clarify`, `/plan`, `/tasks`, `/write`, `/analyze`) can run correctly in a Bash environment.

## [0.10.3] - 2025-09-30

### 🔧 Breaking Changes
- **Removed old format compatibility**: Completely removed support for the old `story.md` format.
  - All scripts now only support the new `specification.md` format.
  - The `/clarify` command only looks for `specification.md`.
  - The `/specify` command has removed the migration logic.
  - `/track-init` and related tracking scripts have been updated to the new format.
  - Updated prompt messages from `/story` to `/specify`.

### 📝 File Updates
- **Bash Scripts**:
  - Updated `clarify-story.sh` to only support `specification.md`.
  - Updated `specify-story.sh` to remove `story.md` compatibility logic.
  - Updated `init-tracking.sh` to look for `specification.md`.
  - Updated `generate-tasks.sh` to check for `specification.md`.

- **PowerShell Scripts**:
  - Updated `clarify-story.ps1` to only support `specification.md`.
  - Updated `specify-story.ps1` to remove `story.md` compatibility logic.

- **Configuration Files**:
  - Updated `.gitignore` to add the `*.backup` rule.

### ⚠️ Migration Notice
If your project is still using `story.md`, please manually rename it to `specification.md`:
```bash
mv stories/your-story/story.md stories/your-story/specification.md
```

## [0.10.2] - 2025-09-30

### 🐛 Bug Fixes
- **Missing command templates**: Completed the command templates for the seven-step methodology.
  - Added `/constitution` - creation constitution command.
  - Added `/specify` - story specification command.
  - Added `/plan` - creative plan command.
  - Added `/tasks` - task decomposition command.
  - Added `/analyze` - comprehensive validation command.
- **Scope of Impact**: After the fix, new projects created with `novel init` will include all command templates.

## [0.10.1] - 2025-09-30

### 🔧 System Improvements
- **Script System Refactoring**: Centralized management of Bash and PowerShell scripts to `.specify/scripts/`.
- **Command Sync Update**: Improved Claude Code and Gemini command templates.
- **Tracking System Enhancement**:
  - Added `/track-init` command to initialize the tracking system.
  - Improved progress tracking and validation rules.
  - Added scripts for timeline, plot, and world consistency checks.
- **Command Optimization**:
  - Updated `/clarify`, `/expert`, `/write`, `/relations`, etc. commands.
  - Removed redundant commands: `/story`, `/style`, `/outline`, `/chapters`.
- **Documentation Improvement**: Updated workflow and quick start guide.

### 📦 Project Structure
- Moved script files to the `.specify` directory for better organization.
- Added submodule support (BMAD-METHOD, spec-kit).
- Improved template files and configuration files.

## [0.10.0] - 2025-09-29

### 🎉 Major Update
- **Seven-Step Methodology System**: Introduced a complete Specification-Driven Development (SDD) creation process.
  - `/constitution` - Creation constitution, defines the highest-level creation principles.
  - `/specify` - Story specification, defines story requirements like a PRD.
  - `/clarify` - Clarify decisions, clarifies key points through interactive Q&A.
  - `/plan` - Creative plan, formulates a technical implementation plan.
  - `/tasks` - Task decomposition, generates an executable task list.
  - `/write` - Chapter writing (refactored to fit the new process).
  - `/analyze` - Comprehensive validation, all-around quality check.

### 🔧 System Refactoring
- **Removed Redundant Commands**: Removed old commands like story, style, outline, chapters, method.
- **Cross-Platform Sync**: Fully synchronized PowerShell scripts and Gemini TOML commands.
- **Documentation System Upgrade**:
  - Created `METHODOLOGY.md` - complete methodology explanation.
  - Created `MIGRATION.md` - version migration guide.
  - Updated command support for all platforms.

### 📝 Philosophy Upgrade
- Upgraded from a "tool collection" to a "methodology framework."
- Shifted from "scattered commands" to a "systematic process."
- Emphasizes "specification-driven" over "inspiration-driven."
- Achieves a complete link from "requirements definition" to "content generation."

### ⚠️ Breaking Changes
- The following old commands have been deleted (replaced by new commands):
  - `/story` → `/specify`
  - `/style` → `/constitution`
  - `/outline` → `/plan`
  - `/chapters` → `/tasks`
  - `/method` → Becomes an optional helper.
- File structure adjustments:
  - `stories/*/chapters/` → `stories/*/content/`
  - Added multiple methodology-related files.

## [0.9.0] - 2025-09-29

### 🎯 Methodology Upgrade
- Introduced the specification-driven development concept from spec-kit.
- **`/clarify` command** - Interactively clarifies key decision points in the story outline.
- Structured creation process: story → clarify → outline.
- Smart Q&A: AI identifies ambiguous points and clarifies the creative direction through 5 precise questions.

## [0.8.4] - 2025-09-26

### 🎉 New Features
- Authentic Voice plugin (improves originality and naturalness).
  - `/authentic-voice` authentic voice creation mode (material cards + personal lexicon).
  - `/authenticity-audit` self-check for "humanness" and line-level rewrite suggestions.
  - `authentic-editor` expert: more detailed voice editing.
- Offline text self-check script: `scripts/bash/text-audit.sh`.
  - Statistics on connective/filler word density, sentence length mean/variance, consecutive long/short sentences, abstract word density examples.
  - Supports project-level configuration: `spec/knowledge/audit-config.json`.

### 📚 Templates and Documentation
- Added writing constitution template: `templates/writing-constitution-template.md`.
- Added "humanness" self-check configuration template: `templates/knowledge/audit-config.json`.
- README added "one-click authentic voice example" and plugin recommended usage instructions.

### 🔧 Process Improvements
- `/style` initialization now automatically references `.specify/memory/personal-voice.md`:
  - Appends "Personal Corpus Summary (automatic reference)."
  - Syncs "Personal Expression Baseline (automatic sync)" to a fixed chapter (idempotent update).
- CLI help now shows `authentic-voice` as an available plugin.

## [0.8.3] - 2025-09-25

### 🎉 New Features
- **Full Gemini Plugin Support**: All plugins now support Gemini CLI.
  - translate plugin: 3 TOML commands.
  - book-analysis plugin: 6 TOML commands.
  - Author style plugins: 13 TOML commands (Wang Yu, Shinianxueluo, Lu Yao).
  - stardust-dreams plugin: 4 TOML commands.

### 🔧 Technical Improvements
- Standardized plugin command format.
- Simplified complex commands into an AI-friendly format.
- Optimized TOML command structure.

### 📝 Plugin Updates
- All 6 official plugins now support dual formats (Markdown + TOML).
- A total of 26 new TOML format command files have been added.
- The plugin system is fully compatible with Gemini CLI.

## [0.8.2] - 2025-09-25

### 🎉 New Features
- **Google Gemini CLI Support**: Full Gemini CLI slash command integration.
  - Added 13 new TOML format command definitions.
  - Supports namespace commands (e.g., `/track:init`, `/plot:check`).
  - The plugin system now supports both Markdown and TOML formats.
  - Smart format conversion and fallback mechanism.

### 📚 New Documentation
- **Gemini Development Guide**: `docs/gemini-command-guide.md` - dual-format command development instructions.
- **Gemini User Document**: `templates/GEMINI.md` - Gemini CLI usage guide.
- **Gemini Configuration File**: `templates/gemini-settings.json` - CLI settings template.

### 🔧 Technical Improvements
- Refactored the plugin manager to support multiple AI platforms.
- The CLI initialization command now intelligently detects and generates the corresponding format.
- Enhanced the command injection mechanism to support automatic format conversion.
- Optimized directory structure management.

### 📝 Compatibility
- Fully backward compatible with existing Claude, Cursor, and Windsurf users.
- Supports the `--ai gemini` parameter to specifically generate Gemini format.
- Plugins can optionally provide TOML format support.

## [0.7.0] - 2025-01-24

### 🎉 New Features
- **External AI Suggestion Integration**: Supports integrating analysis suggestions from AI tools like Gemini and ChatGPT.
  - Extended the `/style` command with a new `refine` mode.
  - Supports both JSON and Markdown suggestion formats.
  - Automatically categorizes and processes suggestions (style/character/plot/worldview/dialogue).
  - Suggestion history tracking and version management.
  - Smart merging of suggestions from multiple sources.

### 📚 New Documentation
- **PRD Document**: `docs/PRD-external-suggestion-integration.md` - feature design specification.
- **AI Prompt Template**: `docs/ai-suggestion-prompt-template.md` - standardized suggestion format.
- **Gemini-Specific Template**: `docs/ai-suggestion-prompt-for-gemini.md` - optimized prompt.
- **Quick Guide**: `docs/quick-guide-external-ai-integration.md` - three steps to complete integration.
- **Example Set**: `docs/suggestion-integration-examples.md` - detailed usage examples.

### 🔧 Technical Improvements
- Added `style-manager.sh` script to handle suggestion integration.
- Optimized format recognition logic to support pipe input.
- Improved Markdown parsing and handling.
- Enhanced error handling mechanism.

### 📝 File Updates
- Updated the `/style` command template to support the new feature.
- Added `improvement-log.md` to track suggestion history.
- Extended `character-voices.md` to add a vocabulary replacement table.

## [0.6.2] - 2025-09-24

### Improvements
- **ESM Module Support**: The project has been fully migrated to ESM (ECMAScript Modules).
  - Added `"type": "module"` configuration.
  - Updated all import statements to ESM format.
  - Used `import.meta.url` instead of `__dirname`.
  - Full support for all versions of Node.js 18+ (including 21, 22, 23).
  - Truly achieved upward compatibility, embracing modern JavaScript standards.

## [0.6.1] - 2025-09-24

### Fixes
- **Dependency Issue**: Fixed a runtime error caused by the missing `js-yaml` module.
  - Added `js-yaml` to the dependencies.
  - Resolved the error with the `novel -h` command.

## [0.6.0] - 2025-09-24

### Added
- **Character Consistency Verification System**: Solved the issue of character name errors in AI-generated content.
  - Added `validation-rules.json` validation rules file.
  - `/write` command enhancement: pre-writing reminder, post-writing validation.
  - `/track --check` deep validation mode: batch check for character consistency.
  - `/track --fix` auto-fix mode: automatically fix simple errors.
- **Program-Driven Validation**: Internally uses a task mechanism to perform validation, improving efficiency.
- **Validation Script**: Added `track-progress.sh` to support validation functions.

### Improvements
- **Writing Process Optimization**: Proactively prevents character name errors during writing.
- **Batch Validation**: Supports validating multiple chapters at once, saving tokens.
- **Auto-Fix**: Can automatically fix character names and titles.

## [0.5.6] - 2025-09-23

### Added
- **Writing Style Plugins**: Added three new writing style plugins.
  - `luyao-style` - Lu Yao style writing plugin.
  - `shizhangyu-style` - Shi Zhangyu style writing plugin.
  - `wangyu-style` - Wang Yu style writing plugin.

## [0.4.3] - 2025-09-21

### Improvements
- **Default Version Number Update**: Updated the default version number in version.ts from 0.4.1 to 0.4.2.
- **Version Consistency**: Ensured all version references remain synchronized.

## [0.4.2] - 2025-09-21

### Improvements
- **Unified Version Management**: Implemented a module that automatically reads the version number from package.json.
- **Knowledge Base Template System**: Changed hard-coded knowledge base files to a template file system.
- **Code Optimization**: Simplified the cli.ts code structure, improving maintainability.

### Fixes
- **Version Number Unification**: Ensured version number consistency through the version.ts module.

## [0.4.0] - 2025-09-21

### Added
- **Plot Tracking System** (`/plot-check`): Tracks plot points, foreshadowing, and conflict development.
- **Timeline Management** (`/timeline`): Maintains the story timeline to ensure logical time consistency.
- **Relationship Matrix** (`/relations`): Manages character relationships and faction dynamics.
- **Worldview Check** (`/world-check`): Verifies setting consistency to avoid contradictions.
- **Comprehensive Tracking** (`/track`): Provides a full view of the creation status.
- **spec Directory Structure**: Added `spec/tracking` and `spec/knowledge` directories.
- **Knowledge Base Templates**:
  - `world-setting.md` - Worldview setting template.
  - `character-profiles.md` - Character profile template.
  - `character-voices.md` - Character voice profile template.
  - `locations.md` - Location template.

### Improvements
- **Tracking File Templates**: Provided complete JSON tracking file templates.
- **Consistency Check Scripts**: Implemented a comprehensive consistency validation system.
- **Workflow Enhancement**: Added a quality assurance process.

## [0.3.7] - 2025-09-20

### Added

- **Time Acquisition Guidance**: Added prompts in command templates to guide the AI to use the `date` command to get the system date.
- **Automatic Date Generation**: Scripts will pre-generate the correct system date for the AI to reference.

### Improvements

- **Flexible Volume Management**: Chapters now automatically parse the volume structure from outline.md, no longer hard-coded to 4 volumes.
- **Dynamic Chapter Count**: Supports reading the total chapter count from outline.md, no longer limited to 240 chapters.
- **Progress File Timestamps**: progress.json now includes creation and update timestamps.

### Fixes

- **Date Generation Error**: Fixed the issue of the AI generating incorrect dates (e.g., 2025-01-20 instead of 2025-09-20).

## [0.3.6] - 2025-01-20

### Fixes

- **Directory Naming Issue**: Fixed the issue where the story directory was named `001-`.
  - Adopted the spec-kit way of handling directory names, extracting only English words.
  - Used the default name `story` for purely Chinese descriptions.
- **Chapter Organization Structure**: Fixed the functionality of generating chapters according to the volume structure.
  - Chapters are now automatically placed in the corresponding volume directory (volume-1 to volume-4) based on their number.
  - Chapters 1-60 are in volume-1, 61-120 in volume-2, and so on.

## [0.3.5] - 2025-01-20

### Fixes

- Fixed the format issue of the `.claude/commands/` configuration files generated by the `novel init` command.
- Retained the complete frontmatter and scripts sections in the command files to ensure Claude can correctly recognize and execute commands.
- Simplified the `generateMarkdownCommand` function to directly return the full template content.

## [0.3.4] - Previous Versions

### Added

- Initial version release.
- Support for multiple AI assistants including Claude, Cursor, Gemini, Windsurf, and Roo Code.
- Provided a complete set of workflow commands for novel writing.
