# Changelog

## [0.19.0] - 2025-10-25

### ✨ New Features

#### 🎉 Codex CLI Support
- **New Platform**: Full support for OpenAI Codex CLI
  - Command format: `/novel-commandname` (e.g., `/novel-write`)
  - Command directory: `.codex/prompts/`
  - Uses the `novel-` prefix to avoid naming conflicts
  - Pure Markdown format (no YAML frontmatter)
  - All 13 core commands are supported
- **Installation**: `novel init my-novel --ai codex`
- **Technical Implementation**: Based on the implementation from [Spec-Kit](https://github.com/github/spec-kit) v0.0.11+

#### 📚 AI Platform Command Reference Document
- **New Document**: `docs/ai-platform-commands.md` - A complete command reference guide for 13 AI platforms
  - Quick reference table: At-a-glance view of command format differences
  - Namespace rules: Detailed explanation of why different prefixes are used
  - Platform-specific details: Complete command lists for Gemini, Claude, Codex, etc.
  - Usage examples: Full workflow demonstrations for the three main platforms
  - FAQ: Solutions for common issues like commands not working, format differences, etc.

### 📝 Documentation Updates

#### Codex CLI Support Notes
- Updated `docs/why-codex-not-supported.md`:
  - Title changed to "Codex CLI Support in Novel Writer"
  - "Coming soon" changed to "Supported in v0.19.0"
  - Historical reasons are kept as a record of design decisions

#### README Update
- Mentioned Codex CLI in the core features
- Added the `--ai codex` option to the initialization example
- Added `/novel-constitution` (Codex format) to the command format examples
- Added Codex CLI to the namespace explanation table

#### GEMINI.md Template Update
- Clearly states that Gemini CLI uses the `novel:` namespace
- Added an explanation for the namespace reason
- Updated all example commands with the correct `novel:` prefix
- Added cross-reference links to the documentation

### 🔧 Build System Improvements

#### Namespace Support
- Modified `scripts/build/generate-commands.sh`:
  - Uses the `novel-` prefix for Codex CLI
  - Generates pure Markdown format (without frontmatter)
  - Command files are located in the `.codex/prompts/` directory

### 📊 Supported AI Platforms

Now supports **13 AI platforms**:

| Platform | Command Format | Namespace |
|------|---------|----------|
| Claude Code | `/novel.commandname` | `novel.` |
| Gemini CLI | `/novel:commandname` | `novel:` |
| **Codex CLI** ⭐ | **`/novel-commandname`** | **`novel-`** |
| Cursor | `/commandname` | None |
| Windsurf | `/commandname` | None |
| Roo Code | `/commandname` | None |
| GitHub Copilot | `/commandname` | None |
| Qwen Code | `/commandname` | None |
| OpenCode | `/commandname` | None |
| Kilo Code | `/commandname` | None |
| Auggie CLI | `/commandname` | None |
| CodeBuddy | `/commandname` | None |
| Amazon Q | `/commandname` | None |

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

#### Incorrect Constitution Save Path for Gemini (#6)
- **Issue**: After running the `/constitution` command in Gemini, the constitution file was incorrectly saved to `memory/constitution.md` (project root) instead of the correct `.specify/memory/constitution.md` path.
- **Cause**: Inconsistent path references in the source template file `templates/commands/constitution.md` and other command files, some of which used paths without the `.specify/` prefix.
- **Fix**: Unified all path references in the command template files to use the full path `.specify/memory/constitution.md`.
  - Modified 3 path references in `templates/commands/constitution.md`
  - Modified 1 path reference in `templates/commands/specify.md`
  - Modified 1 path reference in `templates/commands/plan.md`
  - Modified 3 path references in `templates/commands/write.md`
  - Rebuilt the command files for all platforms.
- **Impact**: Platforms using TOML format, such as Gemini and Qwen, will now correctly save the constitution file to `.specify/memory/constitution.md`.

### 📝 Scope of Impact
- `templates/commands/constitution.md` - Path references have been unified.
- `templates/commands/specify.md` - Path references have been unified.
- `templates/commands/plan.md` - Path references have been unified.
- `templates/commands/write.md` - Path references have been unified.
- `dist/gemini/.gemini/commands/novel/*.toml` - All TOML files have been regenerated.
- Build artifacts for all platforms have been updated.

### 🎯 User Experience Improvements
- After Gemini users run the `/constitution` command, the file will be correctly saved to `.specify/memory/constitution.md`.
- Unified paths prevent path confusion for the AI between different commands.
- The incorrect `memory/` directory will no longer appear in the project root.

---

## [0.18.4] - 2025-10-15

### 🐛 Bug Fixes

#### Unified Constitution File Naming
- **Issue**: The system had 3 different constitution file names (novel-constitution.md, writing-constitution.md, constitution.md), causing multiple constitution files to appear in user projects.
- **Fix**: Unified the constitution file name to `constitution.md`.
  - Renamed the source file: `memory/writing-constitution.md` → `memory/constitution.md`
  - Modified file path references in all Bash scripts (6 files).
  - Modified file path references in all PowerShell scripts (5+ files).
  - Modified file references in all command templates (constitution.md, specify.md, plan.md, analyze.md, write.md).
  - Updated path permissions in `allowed-tools`.

#### Script Path Duplication Issue
- **Issue**: The `rewrite_paths()` function in the build system was repeatedly adding the `.specify/` prefix, causing incorrect paths (`.specify.specify/scripts/`).
- **Fix**: Used a temporary marker to protect existing `.specify/` paths.
  - Modified the `rewrite_paths()` function in `scripts/build/generate-commands.sh`.
  - First, mark existing correct paths, then add the prefix, and finally restore the marker.
  - All paths in the generated command files are now correctly formatted as `.specify/scripts/...`.

### 📝 Scope of Impact
- `memory/constitution.md` - Unified constitution file name.
- `scripts/bash/*.sh` - All scripts referencing the constitution file have been updated.
- `scripts/powershell/*.ps1` - All scripts referencing the constitution file have been updated.
- `templates/commands/*.md` - All command templates have been updated.
- `scripts/build/generate-commands.sh` - The path rewriting function has been fixed.
- `dist/` - Rebuilt the command files for all platforms.

### 🎯 User Experience Improvements
- User projects will only have one `constitution.md` file in the `.specify/memory/` directory.
- All script command paths are correct, and the `.specify.specify/` error no longer occurs.
- The naming is more concise, clear, and easy to understand and use.

---

## [0.18.3] - 2025-10-15

### ✨ Feature Improvements

#### Standardized Plugin Installation System
- **Issue**: The `genre-knowledge` plugin used a manual installation method, which was inconsistent with other plugins.
- **Improvement**: Unified the installation method to use the `novel plugins:add` command.
  - Added a `plugins/genre-knowledge/config.yaml` configuration file.
  - The plugin metadata is fully defined (name, version, description, type, dependencies).
  - Detailed usage instructions and steps are displayed after installation.

#### Documentation Updates
- Updated `plugins/genre-knowledge/README.md`:
  - Changed the installation method to `novel plugins:add genre-knowledge`.
  - Changed the uninstallation method to `novel plugins:remove genre-knowledge`.
  - Added the command `novel plugins:list` to verify installation.
  - Simplified the document structure to highlight the installation process.

#### CLI Enhancements
- Updated the list of available plugins to include `genre-knowledge`:
  - `plugins:list` command prompt message.
  - `plugins:add` command error message.
- Maintained a consistent user experience with other plugins (translate, authentic-voice, book-analysis).

### 🧪 Test Validation
- ✅ Plugin installation process tested and passed.
- ✅ Plugin list displays correctly.
- ✅ Enhanced command files are copied successfully.
- ✅ Detailed usage instructions are displayed after installation.

### 📝 Scope of Impact
- `plugins/genre-knowledge/config.yaml` - New configuration file added.
- `plugins/genre-knowledge/README.md` - Installation instructions updated.
- `src/cli.ts` - Added `genre-knowledge` to the list of available plugins.

---

## [0.18.2] - 2025-10-15

### 🐛 Bug Fixes

#### Missing Plugin Command Files
- **Issue**: The `commands/` directory of the `genre-knowledge` plugin was empty, preventing users from getting the enhanced prompts.
- **Fix**: Added the 3 missing enhanced command files:
  - `commands/clarify-enhance.md` (18 lines) - Genre knowledge enhancement prompt for the `clarify` command.
  - `commands/plan-enhance.md` (62 lines) - Dynamic genre knowledge loading prompt for the `plan` command.
  - `commands/write-enhance.md` (15 lines) - Genre style application prompt for the `write` command.

#### User Experience Improvements
- Users can now directly copy the enhanced prompts from the `commands/*.md` files.
- Pasting them at the `PLUGIN_HOOK` marker in the core commands enables the plugin functionality.
- The structure is clearer and aligns with the plugin architecture design.

### 📝 Scope of Impact
- `plugins/genre-knowledge/commands/` - 3 new command files added.
- Plugin system - The installation experience has been improved.

---

## [0.18.1] - 2025-10-15

### 🏗️ Architectural Optimizations

#### Plugin-ization of Genre Knowledge
- **Migrated genre knowledge files**: Moved the 5 genre knowledge files from `spec/knowledge/genres/` to `plugins/genre-knowledge/knowledge/genres/`.
  - `fantasy.md` (669 lines) - Fantasy/Xuanhuan genre guide.
  - `scifi.md` (530 lines) - Sci-fi genre guide.
  - `romance.md` (378 lines) - Romance genre guide.
  - `mystery.md` (353 lines) - Mystery/Suspense genre guide.
  - `shuangwen.md` (236 lines) -爽文 (爽文) genre guide.

#### Truly Optional Plugin Architecture
- **Core command optimization**:
  - Removed hard-coded dependencies on plugins from the core commands.
  - Added `plugins/**` wildcard permission to support all plugins.
  - Kept the `<!-- PLUGIN_HOOK -->` marker for users to manually enable plugins.
- **User experience improvement**:
  - After installing a plugin, users only need to copy and paste the enhanced prompt to the `PLUGIN_HOOK` marker.
  - No need to modify `allowed-tools` (as `plugins/**` permission is already there).
  - Plugin functionality is completely optional and does not affect core features.

#### Design Philosophy
- ✅ **Clear responsibilities**: `spec/knowledge/` focuses on the knowledge created by the user for the project, while plugins focus on optional features provided by the system.
- ✅ **Optional installation**: Users who do not need genre knowledge do not need to load the plugin.
- ✅ **Single source of truth**: Genre knowledge only exists in the plugin, avoiding duplication and confusion.
- ✅ **Simple architecture**: No technical debt, no backward compatibility code.

### 📝 Scope of Impact
- `spec/knowledge/genres/` - Deleted.
- `plugins/genre-knowledge/` - Contains 7 knowledge files.
- `templates/commands/clarify.md` - Removed plugin hard-coding, added `plugins/**` permission.
- `templates/commands/plan.md` - Removed plugin hard-coding, added `plugins/**` permission.
- `templates/commands/analyze.md` - Removed plugin hard-coding, added `plugins/**` permission.

---

## [0.15.0] - 2025-10-11

### ✨ Major Improvement: Multi-platform Command Format Optimization

#### Background
The previous build system copied Claude-specific YAML frontmatter fields (`allowed-tools`, `model`, `disable-model-invocation`) to all 13 AI platforms, but these fields were not supported or needed on other platforms, causing compatibility issues.

#### Core Fix
- **Platform-specific format generation**: Generates the correct command file format based on the actual support of each AI platform.
- **Format classification system**:
  - **Pure Markdown (no frontmatter)**: Cursor, GitHub Copilot, Codex CLI, Auggie CLI, CodeBuddy, Amazon Q Developer
  - **Minimal frontmatter (description only)**: OpenCode
  - **Partial frontmatter (description + argument-hint)**: Roo Code, Windsurf, Kilo Code
  - **Full frontmatter (all fields)**: Claude Code
  - **TOML format (description + prompt)**: Gemini CLI, Qwen Code

#### Technical Implementation
- **Build script enhancement** (`scripts/build/generate-commands.sh`):
  - Added `frontmatter_type` parameter to the `generate_commands` function.
  - Implemented 4 frontmatter generation strategies (none/minimal/partial/full).
  - Specified the correct format type for all 13 platforms.
  - Extracted the `argument_hint` field to support partial frontmatter.

- **TOML format fix**:
  - Gemini and Qwen's TOML files only contain `description` and `prompt` fields.
  - The parameter placeholder correctly uses `{{args}}` instead of `$ARGUMENTS`.
  - Removed unsupported metadata fields.

#### Validation Results
✅ The command file formats for all 13 AI platforms have been validated.
✅ Each platform only contains the fields it supports, in compliance with the official documentation.
✅ Improved compatibility with various platforms and reduced file redundancy.
✅ Avoided potential parsing errors.

#### Scope of Impact
- 📦 **Build system**: `npm run build:commands` generates command files in the correct format.
- 🎯 **13 platforms**: Claude, Gemini, Cursor, Windsurf, Roo Code, GitHub Copilot, Qwen Code, OpenCode, Codex CLI, Kilo Code, Auggie CLI, CodeBuddy, Amazon Q Developer
- 📁 **182 files**: 14 command files for each platform, all in the correct format.

### 📚 Documentation
Thanks to the community for their feedback, which helped us discover and fix the multi-platform compatibility issues.

## [0.14.2] - 2025-10-10

### 🐛 Bug Fixes

- **Chinese word count issue**: Fixed the highly inaccurate word count of `wc -w` for Chinese characters.
  - Added a `count_chinese_words()` function, improving accuracy by 12+ times.
  - Excludes Markdown markup, code blocks, spaces, and punctuation.
  - Only counts the actual text content.
  - Excellent performance (processing 3000 words takes about 10ms).

### ✨ New Features

- **Word count function** (`scripts/bash/common.sh`)
  - `count_chinese_words()` - Accurate Chinese word count.
  - `show_word_count_info()` - Displays user-friendly word count validation information.

- **Script enhancements**
  - `analyze-story.sh` - Displays detailed word count statistics for each chapter.
  - `check-writing-state.sh` - Automatically validates if the chapter word count meets the requirements.
  - Reads word count requirements from the `validation-rules.json` configuration.

- **Command template updates**
  - `/write` command adds word count validation instructions.
  - Warns against using `wc -w` to count Chinese words.
  - Automatically displays the accurate word count after the AI finishes writing.

### 📚 New Documentation

- **Usage Guide**: `docs/word-count-guide.md` - Complete instructions for using the word count feature.
- **Test Script**: `scripts/bash/test-word-count.sh` - Verifies the accuracy of the count.
- **Fix Notes**: `WORD_COUNT_FIX.md` - Issue diagnosis and solution.

### 🎯 Problems Solved

- AI writing prompts with "not enough words," but the actual word count has exceeded the requirement.
- Using `wc -w` to count Chinese chapter words results in a severely low count (121/164 vs 2000+).
- Inconsistent results when counting the same file multiple times.

### ⚠️ Important Reminder

- ❌ Do not use `wc -w` to count Chinese words (highly inaccurate).
- ❌ Do not use `wc -m` to count words (includes too many irrelevant characters).
- ✅ Use the `count_chinese_words` function to get accurate results.

## [0.14.0] - 2025-10-09

### ✨ New Features

- **Roo Code Slash Command Support**: `novel init` and `novel upgrade` now support generating the `.roo/commands` directory and automatically outputting Roo Code compatible Markdown commands.
- **Plugin System Integration**: The plugin command injection process has been extended to Roo Code, ensuring that installed plugins can be used in Roo Code immediately.

### 📚 Documentation Updates

- README and CHANGELOG now include support for Roo Code, and the list of available AIs has been updated.

## [0.13.7] - 2025-10-06

### 🐛 Bug Fixes

- **Plugin command file naming optimization**: Fixed the issue of overly complex command file names after plugin installation.
  - Removed the unnecessary `plugin-{pluginName}-` prefix.
  - Simplified plugin command file names: `plugin-book-analysis-book-analyze.md` → `book-analyze.md`.
  - Maintained a consistent naming style with the core commands.
  - Applies to all AI platforms (Claude, Cursor, Windsurf, Gemini).

## [0.13.6] - 2025-10-06

### 🐛 Bug Fixes

- **CLI help text update**: Fixed the help text displayed after `novel init`.
  - Updated the core command list to the correct seven-step methodology commands (constitution, specify, clarify, plan, tasks, write, analyze).
  - Removed deprecated old commands (method, style, story, outline, chapters).
  - Updated the recommended flow to: `constitution → specify → clarify → plan → tasks → write → analyze`.

## [0.12.2] - 2025-10-04

### ✨ New Features: Claude Code Enhanced Layer

#### Core Improvement
Provides an exclusive enhanced version of commands for **Claude Code** users, while maintaining **full compatibility with other platforms (Gemini, Cursor, Windsurf)**.

#### 1. Build System Design (Upgraded to single source + build system in v0.15.0+)
- **Single source**: `templates/commands/` - Command source files (formerly `commands-claude/`).
- **Build system**: `scripts/build/generate-commands.sh` - Automatically generates commands for all platforms.
- **Namespace**: Claude uses the `novel.*` prefix, Gemini uses the `novel/` subdirectory, to avoid conflicts with spec-kit.
- **Release process**: The `dist/` directory is automatically generated during the build, and directly copied during user initialization.

#### 2. Claude Code Exclusive Features

**Enhanced Frontmatter Fields**:
- `argument-hint` - Command argument auto-completion hints.
- `allowed-tools` - Fine-grained tool permission control (e.g., `Bash(find:*)`, `Read(//**)`).
- `model` - Specifies the most suitable AI model for each command (default `claude-sonnet-4-5-20250929`).
- `disable-model-invocation` - Controls whether the SlashCommand tool can be automatically invoked.

**Dynamic Context Loading**:
- Supports inline bash execution: `!`command``.
- Real-time project status retrieval (number of chapters, word count, tracking files, etc.).
- Reduces manual user input and improves command intelligence.

#### 3. Enhanced Command List

**P0 Commands (3)**:
- `/analyze` - Added phase detection, chapter list, and word count dynamic context.
- `/write` - Added to-do tasks, latest chapter, and progress status dynamic loading.
- `/clarify` - Added story file path and spec detection dynamic context.

**P1 Commands (3)**:
- `/track` - Added tracking file status, progress statistics, chapter list, and word count.
- `/specify` - Added constitution check, spec file check, and path information.
- `/plan` - Added spec status, plan file check, and count of items to be clarified.

**P2 Commands (5)**:
- `/tasks` - Added plan/spec file check and clue management spec summary.
- `/plot-check` - Added tracking file status, progress check, and chapter statistics.
- `/timeline` - Added timeline status, time node statistics, and chapter mapping.
- `/relations` - Added relationship network status and role/faction statistics.
- `/world-check` - Added knowledge base check, setting statistics, and proper noun statistics.

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
- ✅ Does not modify the command directories of other platforms (`.claude`, `.cursor`, `.gemini`, etc.).
- ✅ The basic commands remain unchanged, ensuring that Gemini/Cursor/Windsurf can be used normally.
- ✅ The Claude enhancement layer is optional and does not affect existing users.
- ✅ All enhanced features are only effective in the Claude Code environment.

### 📚 Documentation Updates
- **README.md**: Added v0.12.2 Claude Code enhanced layer feature description.
- **CHANGELOG.md**: Detailed record of enhanced features and implementation details.

### 🎯 Design Philosophy
**Enhance without breaking compatibility**:
- ❌ Do not create new commands or new platform-specific commands.
- ✅ Layered architecture with priority selection.
- ✅ Claude users get the best experience.
- ✅ The experience of users on other platforms is not affected.

---

## [0.12.1] - 2025-10-01

### ✨ New Feature: Smart Dual-Mode Analyze

#### Core Improvement
The `/analyze` command has been upgraded to a **smart dual-mode**, automatically selecting the analysis type based on the creation stage, **without adding new commands**.

#### 1. Smart Stage Detection
- **Automatic judgment**: The system detects the number of chapters and automatically decides whether to perform a framework analysis or a content analysis.
- **Manual specification**: Supports `--type=framework` or `--type=content` to force a specific mode.
- **Script support**: Added `scripts/bash/check-analyze-stage.sh` and `scripts/powershell/check-analyze-stage.ps1`.

#### 2. Mode A: Framework Consistency Analysis (before writing)
- **Coverage analysis**: Checks if the spec requirements have corresponding plans and tasks.
- **Consistency check**: Verifies if there are contradictions between the spec, plan, and tasks.
- **Logical pre-warning**: Analyzes potential logical loopholes in the storyline design.
- **Readiness assessment**: Assesses whether writing can begin.

#### 3. Mode B: Content Quality Analysis (after writing)
- **Constitution compliance**: Verifies if the work complies with the creation principles.
- **Spec compliance**: Checks if the implementation meets the spec requirements.
- **Content quality**: Analyzes logic, characters, rhythm, etc.
- **Improvement suggestions**: Provides specific P0/P1/P2 fix suggestions.

#### 4. Decision Logic
```
Number of chapters = 0     → Framework analysis
Number of chapters < 3     → Framework analysis (suggest to continue writing)
Number of chapters ≥ 3     → Content analysis
User specifies --type → Force the specified mode
```

### 📚 Documentation Updates
- **README.md**: Updated the `/analyze` command description to showcase the smart dual-mode.
- **docs/writing/analyze-placement-rationale.md**: Added an "Appendix: Smart Dual-Mode Design" section.
- **Command template**: Completely rewrote the `analyze` command to detail the two analysis modes.

### 🎯 Design Philosophy
**Restrained but not simple**:
- ❌ Do not create two commands (`/framework-analyze`, `/content-analyze`).
- ✅ One `/analyze` command that intelligently judges the scenario.
- ✅ 90% automated, 10% manually controllable.
- ✅ Meets multiple needs while keeping the command simple.

### 💡 Community Feedback Driven
Thanks to @ZengXisheng Anson for the request for both "framework analysis before writing" and "content review after writing."
Through intelligent design, we have met both needs without adding more commands.

---

## [0.12.0] - 2025-09-30

### ✨ New Feature: Multi-Clue Management System

#### Core Improvement
**No new commands needed**. The complete multi-clue management capability is achieved by enhancing the existing command templates.

#### 1. specification.md Enhancement (`/specify` command)
Added **Chapter 5: Clue Management Specification**, which includes 5 management tables:
- **5.1 Clue Definition Table**: Defines the ID, type, priority, and conflicts of all clues.
- **5.2 Clue Pacing Plan**: Plans the activity level of each clue in different volumes (⭐⭐⭐/⭐⭐/⭐).
- **5.3 Clue Intersection Plan**: Pre-plans the timing of clue intersections to avoid random AI improvisation.
- **5.4 Foreshadowing Management Table**: Manages the placement and reveal of foreshadowing to ensure nothing is missed.
- **5.5 Clue Modification Decision Matrix**: An impact assessment checklist for when clues are modified.

#### 2. creative-plan.md Enhancement (`/plan` command)
The chapter section table now includes "Active Clues" and "Intersection Point" columns:
- Marks which clues are advanced in each chapter section.
- ⭐⭐⭐ Main advancement / ⭐⭐ Auxiliary / ⭐ Background.
- Clearly indicates the chapters where intersection points occur.

#### 3. tasks.md Enhancement (`/tasks` command)
Each writing task now includes clue-related fields:
- **Involved Clues**: Which clues are advanced in this chapter and their priority.
- **Intersection Point**: Whether this chapter is an intersection point.
- **Foreshadowing Placement/Reveal**: The foreshadowing operations involved in this chapter.

#### 4. plot-tracker.json Enhancement (`/track-init` command)
`/track --init` automatically reads from Chapter 5 of `specification.md`:
- All clue definitions (from section 5.1).
- All intersection points (from section 5.3).
- All foreshadowing (from section 5.4).
- Generates a complete tracking data structure.

#### 5. Practical Guide Update (`docs/writing/practical-guide.md`)
Added **Chapter 6: Multi-Clue Management Guide**, which includes:
- A real problem scenario (from user feedback).
- A 4-step solution.
- A complete usage example based on "Return to 1984."
- A comparison table of how the three main pain points are resolved.

### 🎯 Core Problems Solved
Real confusion from users:
> "It's hard to explain to the AI how to keep the main and side plots parallel, and how to intersect and reveal previous clues at the right time. It's a disaster, especially when the plot setting is constantly changing."

#### Three Main Pain Points and Their Solutions
| Pain Point | Solution | Specific File |
|------|---------|---------|
| **Parallel advancement** | `tasks.md` marks "Involved Clues" for each chapter. | W040 marked with PL-01⭐⭐⭐, PL-02⭐⭐. |
| **Intersection timing** | `specification.md` section 5.3 pre-plans. | X-001 is set for chapter 40, avoiding AI randomness. |
| **Modification consistency** | 5.5 modification decision matrix + `/track --check`. | Automatically prompts the scope of impact when modifying PL-02. |

### 📐 Design Principles
- ✅ **Follows the "if it's not necessary, don't add it" principle**: Completely uses the existing 7 commands.
- ✅ **Follows the SDD methodology**: Clue management is distributed across specify→plan→tasks→track.
- ✅ **Supported by writing theory**: Concepts from Story Grid's Grid Spreadsheet and Save the Cat's B Story.
- ✅ **Solves real pain points**: Based on actual user needs, not imagined features.

### 📝 Documentation Improvements
- Detailed usage example (based on the 5 clues in "Return to 1984").
- Complete input prompt templates.
- Impact assessment and consistency validation process.

## [0.11.0] - 2025-09-30

### ✨ New Features
- **SDD Methodology Practical Guide**: Added `docs/writing/practical-guide.md` (approx. 10,000 words).
  - A complete practical case of SDD based on the novel "Return to 1984."
  - Detailed explanation of the layered recursive application of SDD (whole book/a volume/chapter section/single chapter).
  - Provides actual input prompt examples for 4 complete scenarios.
  - Adds a comparison of good and bad prompts.
  - Adds a complete dialogue flow demonstration.
  - Answers practical questions like "What if the AI deviates from the outline while writing?"

- **Visual Diagrams**: Added 3 SVG diagrams to aid understanding.
  - `sdd-levels.svg` - SDD layered recursion diagram.
  - `sdd-flow.svg` - SDD complete cycle flow chart.
  - `prompt-structure.svg` - Good prompt structure diagram.

### 📝 Documentation Improvements
- Emphasizes the core of SDD: spec-driven + layered recursion + allowing deviation + frequent validation.
- Each scenario includes:
  - ❌ Bad prompt examples.
  - ✅ Good prompt examples.
  - 💬 Complete dialogue flow (User→AI→Confirm→Complete).
- Provides a prompt structure template (Situation/Modification Intent/Needs Update/Expected Output).

### 🎯 Problems Solved
- How to adjust the plot direction mid-writing.
- How to handle excellent deviations produced by the AI.
- What command combinations to use for modifications of different granularities.
- How to write prompts that the AI can understand.

## [0.10.5] - 2025-09-30

### 🐛 Bug Fixes
- **`common.sh` missing function**: Added the `get_active_story()` function.
  - Fixed the "get_active_story: command not found" error during script execution.
  - Synced to `.specify/scripts/bash/` and `scripts/bash/`.

### 📝 Scope of Impact
The following scripts can now be executed normally after the fix:
- `check-writing-state.sh`
- `plan-story.sh`
- `tasks-story.sh`
- `analyze-story.sh`

## [0.10.4] - 2025-09-30

### 🐛 Bug Fixes
- **Missing seven-step methodology scripts**: Completed Bash script support.
  - Created `plan-story.sh` - creation plan script.
  - Created `tasks-story.sh` - task breakdown script.
  - Copied `analyze-story.sh` - comprehensive validation script.
  - Copied `constitution.sh` - creation constitution script.
  - Copied `specify-story.sh` - story spec script.

### 📝 File Updates
- Updated the `/tasks` command template script reference from `generate-tasks.sh` to `tasks-story.sh`.
- Synced all scripts to `.specify/scripts/bash/` and `scripts/bash/`.
- Synced the command templates to `.claude/commands/`.

### 🔧 Scope of Impact
After the fix, all seven-step methodology commands (`/constitution`, `/specify`, `/clarify`, `/plan`, `/tasks`, `/write`, `/analyze`) can be executed normally in a Bash environment.

## [0.10.3] - 2025-09-30

### 🔧 Breaking Changes
- **Removed old format compatibility**: Completely removed support for the old `story.md` format.
  - All scripts now only support the new `specification.md` format.
  - The `/clarify` command only looks for `specification.md`.
  - The `/specify` command has removed the migration logic.
  - `/track-init` and related tracking scripts have been updated to the new format.
  - Updated prompt messages from `/story` to `/specify`.

### 📝 File Updates
- **Bash scripts**:
  - Updated `clarify-story.sh` to only support `specification.md`.
  - Updated `specify-story.sh` to remove `story.md` compatibility logic.
  - Updated `init-tracking.sh` to look for `specification.md`.
  - Updated `generate-tasks.sh` to check for `specification.md`.

- **PowerShell scripts**:
  - Updated `clarify-story.ps1` to only support `specification.md`.
  - Updated `specify-story.ps1` to remove `story.md` compatibility logic.

- **Configuration files**:
  - Updated `.gitignore` to add the `*.backup` rule.

### ⚠️ Migration Notice
If your project is still using `story.md`, please rename it to `specification.md` manually:
```bash
mv stories/your-story/story.md stories/your-story/specification.md
```

## [0.10.2] - 2025-09-30

### 🐛 Bug Fixes
- **Missing command templates**: Completed the seven-step methodology command templates.
  - Added `/constitution` - creation constitution command.
  - Added `/specify` - story spec command.
  - Added `/plan` - creation plan command.
  - Added `/tasks` - task breakdown command.
  - Added `/analyze` - comprehensive validation command.
- **Scope of impact**: After the fix, new projects created with `novel init` will include all command templates.

## [0.10.1] - 2025-09-30

### 🔧 System Improvements
- **Script system refactoring**: Unified management of Bash and PowerShell scripts to `.specify/scripts/`.
- **Command sync update**: Improved Claude Code and Gemini command templates.
- **Tracking system enhancement**:
  - Added `/track-init` command to initialize the tracking system.
  - Improved progress tracking and validation rules.
  - Added timeline, plot, and world-building consistency check scripts.
- **Command optimization**:
  - Updated `/clarify`, `/expert`, `/write`, `/relations`, etc.
  - Deleted redundant commands: `/story`, `/style`, `/outline`, `/chapters`.
- **Documentation improvement**: Updated the workflow and quick start guide.

### 📦 Project Structure
- Moved script files to the `.specify` directory for better organization.
- Added submodule support (BMAD-METHOD, spec-kit).
- Improved template files and configuration files.

## [0.10.0] - 2025-09-29

### 🎉 Major Update
- **Seven-step methodology system**: Introduced a complete Spec-Driven Development (SDD) creation flow.
  - `/constitution` - Creation constitution, defines the highest-level creation principles.
  - `/specify` - Story spec, defines story requirements like a PRD.
  - `/clarify` - Clarification and decisions, clarifies key points through interactive Q&A.
  - `/plan` - Creation plan, formulates the technical implementation plan.
  - `/tasks` - Task breakdown, generates an executable task list.
  - `/write` - Chapter writing (refactored to fit the new flow).
  - `/analyze` - Comprehensive validation, all-around quality check.

### 🔧 System Refactoring
- **Deleted redundant commands**: Removed old commands like story, style, outline, chapters, method.
- **Cross-platform sync**: PowerShell scripts and Gemini TOML commands are fully synced.
- **Documentation system upgrade**:
  - Created `METHODOLOGY.md` - complete methodology description.
  - Created `MIGRATION.md` - version migration guide.
  - Updated command support for all platforms.

### 📝 Philosophy Upgrade
- Upgraded from a "tool collection" to a "methodology framework."
- Shifted from "scattered commands" to a "systematic flow."
- Emphasized "spec-driven" over "inspiration-driven."
- Implemented a complete link from "requirements definition" to "content generation."

### ⚠️ Breaking Changes
- Deleted the following old commands (replaced by new ones):
  - `/story` → `/specify`
  - `/style` → `/constitution`
  - `/outline` → `/plan`
  - `/chapters` → `/tasks`
  - `/method` → became an optional helper.
- File structure adjustments:
  - `stories/*/chapters/` → `stories/*/content/`
  - Added several methodology-related files.

## [0.9.0] - 2025-09-29

### 🎯 Methodology Upgrade
- Introduced the spec-driven development concept from spec-kit.
- **`/clarify` command** - Interactively clarifies key decision points in the story outline.
- Structured creation flow: story → clarify → outline.
- Smart Q&A: The AI identifies ambiguous points and clarifies the creative direction with 5 precise questions.

## [0.8.4] - 2025-09-26

### 🎉 New Features
- Authentic Voice plugin (improves originality and naturalness).
  - `/authentic-voice` authentic voice creation mode (material cards + personal lexicon).
  - `/authenticity-audit` self-check for human-like quality and line-level rewrite suggestions.
  - `authentic-editor` expert: for more detailed voice editing.
- Offline text self-check script: `scripts/bash/text-audit.sh`.
  - Statistics for conjunction/filler word density, sentence length mean/variance, consecutive long/short sentences, abstract word density examples.
  - Supports project-level configuration: `spec/knowledge/audit-config.json`.

### 📚 Templates and Documentation
- New writing constitution template: `templates/writing-constitution-template.md`.
- New human-like quality self-check configuration template: `templates/knowledge/audit-config.json`.
- README adds an "Authentic Voice One-Click Example" and plugin recommended usage instructions.

### 🔧 Process Improvements
- `/style` initialization automatically references `.specify/memory/personal-voice.md`:
  - Appends "Personal Corpus Summary (auto-referenced)."
  - Syncs "Personal Expression Baseline (auto-synced)" fixed chapter (idempotent update).
- CLI help now shows `authentic-voice` as an available plugin item.

## [0.8.3] - 2025-09-25

### 🎉 New Features
- **Full plugin Gemini support**: All plugins support the Gemini CLI.
  - `translate` plugin: 3 TOML commands.
  - `book-analysis` plugin: 6 TOML commands.
  - Author style plugins: 13 TOML commands (Wang Yu, Shinianxueluo, Lu Yao).
  - `stardust-dreams` plugin: 4 TOML commands.

### 🔧 Technical Improvements
- Standardized plugin command format.
- Simplified complex commands into an AI-friendly format.
- Optimized the TOML command structure.

### 📝 Plugin Updates
- All 6 official plugins now support dual formats (Markdown + TOML).
- A total of 26 new TOML format command files have been added.
- The plugin system is fully compatible with the Gemini CLI.

## [0.8.2] - 2025-09-25

### 🎉 New Features
- **Google Gemini CLI Support**: Complete Gemini CLI slash command integration.
  - Added 13 TOML format command definitions.
  - Supports namespace commands (e.g., `/track:init`, `/plot:check`).
  - The plugin system supports both Markdown and TOML dual formats.
  - Smart format conversion and fallback mechanism.

### 📚 New Documentation
- **Gemini Development Guide**: `docs/gemini-command-guide.md` - Dual-format command development instructions.
- **Gemini User Document**: `templates/GEMINI.md` - Gemini CLI usage guide.
- **Gemini Configuration File**: `templates/gemini-settings.json` - CLI settings template.

### 🔧 Technical Improvements
- Refactored the plugin manager to support multiple AI platforms.
- The CLI initialization command intelligently detects and generates the corresponding format.
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
  - Automatically categorizes and processes suggestions (style/character/plot/world-building/dialogue).
  - Suggestion history tracking and version management.
  - Smart merging of multi-source suggestions.

### 📚 New Documentation
- **PRD Document**: `docs/PRD-external-suggestion-integration.md` - Feature design specification.
- **AI Prompt Template**: `docs/ai-suggestion-prompt-template.md` - Standardized suggestion format.
- **Gemini-specific Template**: `docs/ai-suggestion-prompt-for-gemini.md` - Optimized prompt.
- **Quick Guide**: `docs/quick-guide-external-ai-integration.md` - Three steps to complete integration.
- **Example Set**: `docs/suggestion-integration-examples.md` - Detailed usage examples.

### 🔧 Technical Improvements
- Added a `style-manager.sh` script to handle suggestion integration.
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
- **Dependency issue**: Fixed a runtime error caused by the missing `js-yaml` module.
  - Added `js-yaml` to the dependencies.
  - Resolved the error with the `novel -h` command.

## [0.6.0] - 2025-09-24

### New
- **Character Consistency Validation System**: Resolves character name errors in AI-generated content.
  - Added `validation-rules.json` validation rule file.
  - `/write` command enhancement: pre-writing reminders, post-writing validation.
  - `/track --check` deep validation mode: batch check for character consistency.
  - `/track --fix` auto-fix mode: automatically fix simple errors.
- **Program-driven validation**: Internally uses a task mechanism to perform validation, improving efficiency.
- **Validation script**: Added `track-progress.sh` to support the validation feature.

### Improvements
- **Writing flow optimization**: Proactively prevents character name errors during writing.
- **Batch validation**: Supports validating multiple chapters at once, saving tokens.
- **Auto-fix**: Can automatically fix character names and forms of address.

## [0.5.6] - 2025-09-23

### New
- **Writing style plugins**: Added three writing style plugins.
  - `luyao-style` - Lu Yao style writing plugin.
  - `shizhangyu-style` - Shi Zhangyu style writing plugin.
  - `wangyu-style` - Wang Yu style writing plugin.

## [0.4.3] - 2025-09-21

### Improvements
- **Default version number update**: Updated the default version number in `version.ts` from 0.4.1 to 0.4.2.
- **Version consistency**: Ensures all version references remain in sync.

## [0.4.2] - 2025-09-21

### Improvements
- **Unified version management**: Implemented a module that automatically reads the version number from `package.json`.
- **Knowledge base template system**: Changed the hard-coded knowledge base files to a template file system.
- **Code optimization**: Simplified the `cli.ts` code structure to improve maintainability.

### Fixes
- **Version number unification**: The `version.ts` module ensures version number consistency.

## [0.4.0] - 2025-09-21

### New
- **Plot Tracking System** (`/plot-check`): Tracks plot points, foreshadowing, and conflict development.
- **Timeline Management** (`/timeline`): Maintains the story timeline to ensure logical time consistency.
- **Relationship Matrix** (`/relations`): Manages character relationships and faction dynamics.
- **World-building Check** (`/world-check`): Verifies setting consistency to avoid contradictions.
- **Comprehensive Tracking** (`/track`): Provides a full view of the creation status.
- **spec directory structure**: Added `spec/tracking` and `spec/knowledge` directories.
- **Knowledge base templates**:
  - `world-setting.md` - World-building setting template.
  - `character-profiles.md` - Character profile template.
  - `character-voices.md` - Character language profile template.
  - `locations.md` - Scene location template.

### Improvements
- **Tracking file templates**: Provides complete JSON tracking file templates.
- **Consistency check scripts**: Implemented a comprehensive consistency validation system.
- **Workflow enhancement**: Added a quality assurance process.

## [0.3.7] - 2025-09-20

### New

- **Time retrieval guidance**: Added a prompt in the command template to guide the AI to use the `date` command to get the system date.
- **Automatic date generation**: The script will pre-generate the correct system date for the AI to reference.

### Improvements

- **Flexible volume management**: Chapters now automatically parse the volume structure from `outline.md`, no longer hard-coded to 4 volumes.
- **Dynamic chapter count**: Supports reading the total number of chapters from `outline.md`, no longer limited to 240 chapters.
- **Progress file timestamps**: `progress.json` now includes creation and update timestamps.

### Fixes

- **Date generation error**: Fixed an issue where the AI generated incorrect dates (e.g., 2025-01-20 instead of 2025-09-20).

## [0.3.6] - 2025-01-20

### Fixes

- **Directory naming issue**: Fixed an issue where the story directory was named `001-`.
  - Adopted the spec-kit way of handling directory names, only extracting English words.
  - Uses the default name `story` for purely Chinese descriptions.
- **Chapter organization structure**: Fixed the feature of generating chapters according to the volume structure.
  - Chapters are now automatically placed in the corresponding volume directory (volume-1 to volume-4) based on their number.
  - Chapters 1-60 are in volume-1, 61-120 are in volume-2, and so on.

## [0.3.5] - 2025-01-20

### Fixes

- Fixed a format issue with the `.claude/commands/` configuration file generated by the `novel init` command.
- Preserved the complete frontmatter and scripts sections in the command files to ensure that Claude can correctly recognize and execute the commands.
- Simplified the `generateMarkdownCommand` function to directly return the full template content.

## [0.3.4] - Previous Versions

### New

- Initial version release.
- Supports multiple AI assistants: Claude, Cursor, Gemini, Windsurf, Roo Code.
- Provides a complete set of workflow commands for novel writing.
