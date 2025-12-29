# Novel Writer - Gemini CLI Configuration

This project is configured to support the Google Gemini CLI, providing a complete novel writing system based on the seven-step methodology.

## 🎯 v0.10.0 Seven-Step Methodology

Novel Writer uses the Specification-Driven Development (SDD) philosophy to create novels through a systematic seven-step process.

## ⚠️ Important: Gemini CLI Command Format

**The Gemini CLI uses the namespace prefix** `novel:`, and all commands are formatted as:

```bash
/novel:command-name [arguments]
```

**Reason**:
- Novel Writer uses the `novel:` namespace to avoid command conflicts with other tools (like spec-kit, OpenSpec).
- Subdirectories in the Gemini CLI are automatically converted to a colon-separated namespace (path: `.gemini/commands/novel/write.toml` → command: `/novel:write`).

> 📖 **Detailed Command Reference**: See [docs/ai-platform-commands.md](../docs/ai-platform-commands.md) to understand the command format differences across all AI platforms.

### Seven-Step Methodology Commands

1.  **`/novel:constitution`** - Create a constitution (define core principles).
2.  **`/novel:specify`** - Define story specifications (clarify what to create).
3.  **`/novel:clarify`** - Clarify decisions (interactively resolve ambiguities).
4.  **`/novel:plan`** - Create a writing plan (develop a technical solution).
5.  **`/novel:tasks`** - Decompose into tasks (generate an executable list).
6.  **`/novel:write`** - Write chapters (execute content creation).
7.  **`/novel:analyze`** - Comprehensive validation (full quality check).

### Tracking Management Commands

- `/novel:plot-check` - Plot logic check.
- `/novel:world-check` - World-building consistency check.
- `/novel:timeline` - Timeline management.
- `/novel:relations` - Character relationship management.
- `/novel:track` - Comprehensive progress tracking.
- `/novel:track-init` - Initialize the tracking system.

### Expert Mode Command

- `/novel:expert` - Activate expert mode for in-depth guidance.

## How to Use

### Recommended Workflow (Seven-Step Methodology)

Use the commands in the Gemini CLI in the following order (**note the `novel:` prefix**):

```bash
# 1. Establish creative principles
> /novel:constitution

# 2. Define story specifications
> /novel:specify a fantasy story about an adventure

# 3. Clarify key decisions
> /novel:clarify

# 4. Formulate the creative plan
> /novel:plan

# 5. Generate a task list
> /novel:tasks

# 6. Start writing
> /novel:write chapter one

# 7. Validate quality
> /novel:analyze
```

## Tool Permissions

This project is configured with the following tool permissions:
- File read/write (read_file, write_file, edit_file)
- Shell command execution (limited scope)
- File search (glob_files)

## Project Structure

```
memory/           # Creative memory (new in v0.10.0)
└── novel-constitution.md  # Writing constitution

stories/          # Story content
├── [story-name]/
│   ├── specification.md   # Story specification (replaces story.md)
│   ├── clarification.md   # Clarification log (new in v0.10.0)
│   ├── creative-plan.md   # Creative plan (replaces outline.md)
│   ├── tasks.md           # Task list (new in v0.10.0)
│   ├── analysis-report.md # Analysis report (new in v0.10.0)
│   └── content/           # Chapter content (replaces chapters/)

spec/             # Configuration and knowledge base
├── tracking/     # Progress tracking
├── knowledge/    # World-building settings
└── presets/      # Writing method templates

.gemini/          # Gemini configuration
├── commands/     # Command definitions (TOML)
└── settings.json # Gemini settings
```

## Core Philosophy of the Methodology

**Specification-Driven Creation**: No longer relying on inspiration and randomness, but driving content generation through precise specification definitions.

- **Constitutional Constraints**: Creative principles are the supreme, inviolable guidelines.
- **Specification as Requirement**: Define the story like a product manager writes a PRD.
- **Plan as Path**: The technical solution determines how to implement the specification.
- **Task as Execution**: Executable units that are trackable and verifiable.
- **Continuous Validation**: Perform quality checks at each stage.

## Plugin Support

If you have plugins installed, additional commands will be available. Check the `plugins/` directory to see installed plugins.

## Notes

1.  **Follow the Seven-Step Process**: Execute the steps in order; do not skip them.
2.  **Define Before Executing**: Complete the specification definition before starting to write.
3.  **Continuously Validate**: Run `/analyze` every 5 chapters to check the quality.
4.  **Iterate and Optimize**: Adjust the specification and plan based on the analysis results.

## Getting Help

- Use `/expert` to activate expert mode for in-depth guidance.
- Check the `docs/` directory for detailed documentation.
- Visit the project repository: https://github.com/wordflowlab/novel-writer

## Known Issues and Solutions

### Chinese Character Encoding Issues
The Gemini CLI may occasionally output some Chinese characters as garbled text (displayed as � or other gibberish). This is a known encoding issue with Gemini.

#### Preventive Measures
1.  **Terminal Settings**
    - Windows: Use Windows Terminal or PowerShell (avoid `cmd`).
    - Mac/Linux: Ensure your terminal supports UTF-8.

2.  **Environment Variable Settings**
    ```bash
    # Windows PowerShell
    [Console]::OutputEncoding = [System.Text.Encoding]::UTF8

    # Mac/Linux
    export LANG=zh_CN.UTF-8
    export LC_ALL=zh_CN.UTF-8
    ```

#### Solutions When Garbled Text Appears
1.  **Regenerate**: Rerunning the same command usually works the second time.
2.  **Manual Fix**: Directly edit the generated file and replace the garbled characters with the correct Chinese ones.
3.  **Process in Sections**: If a particular section is garbled, regenerate only that section.

#### Common Garbled Patterns
- `�` → Often common characters like "的" or "了".
- `\u4e2d\u6587` → Unicode escapes, need to be converted back to Chinese.
- Some punctuation marks display abnormally → Manually replace with Chinese punctuation.

#### Temporary Solutions
If garbled text appears frequently, you can:
1.  Generate shorter paragraphs (reduce the output per request).
2.  Use standard mode instead of sectioned mode.
3.  Check and fix immediately after generation.

Note: Google is working on fixing this issue, and it should improve in future versions.

---
*This project is developed by the Novel Writer team, designed specifically for AI-driven Chinese novel writing.*
