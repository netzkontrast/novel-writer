# Spec Directory Guide

> Novel Writer's Specification and Data Organization Hub

## 📁 Directory Structure

```
spec/
├── config.json                 # Main configuration file
├── README.md                   # This file
├── presets/                    # Writing method presets
│   ├── anti-ai-detection.md    # Anti-AI detection guidelines
│   ├── golden-opening.md       # Golden opening formula
│   ├── three-act/              # Three-act structure
│   ├── hero-journey/           # Hero's journey
│   └── ...                     # Other writing methods
├── checklists/                 # Quality checklists
│   ├── specification-quality.md
│   ├── plot-logic.md
│   └── ...
├── knowledge/                  # Knowledge base (accumulated content)
│   ├── world/                  # World-building knowledge
│   ├── rules/                  # Rule-based knowledge
│   ├── characters/             # Character profiles
│   └── research/               # Research materials
└── tracking/                   # Tracking data (runtime data)
    ├── plot-tracker.json       # Plot tracking
    ├── character-state.json    # Character states
    ├── relationships.json      # Relationship network
    └── timeline.json           # Timeline
```

---

## 🎯 Directory Responsibilities

### Specification Layer (Immutable Content)

**`presets/`** - Writing method presets
- **Responsibility**: Stores templates and guidelines for different writing methods.
- **Characteristics**: These files are "read-only references" and should not be modified.
- **Examples**: `anti-ai-detection.md` (Anti-AI detection guidelines), `golden-opening.md` (Golden opening formula).
- **Upgrade Strategy**: Can be safely overwritten by `novel upgrade`.

**`checklists/`** - Quality checklists
- **Responsibility**: Stores various quality check standards.
- **Characteristics**: Standardized checklist items, read by the AI when the `/checklist` command is used.
- **Examples**: `specification-quality.md` (Specification integrity check), `plot-logic.md` (Plot logic check).
- **Upgrade Strategy**: Can be safely overwritten by `novel upgrade`.

**`config.json`** - Main configuration file
- **Responsibility**: Defines project-level configurations.
- **Characteristics**: Can be modified by the user, but has default values.
- **Example**: `{"method": "three-act", "version": "0.5.2"}`
- **Upgrade Strategy**: Merged upgrade (preserves user-defined configurations).

### Knowledge Layer (Accumulated Content)

**`knowledge/`** - Knowledge base
- **Responsibility**: Stores knowledge and materials accumulated during the creative process.
- **Characteristics**: Completely created and managed by the user; the system will not overwrite it.
- **Subdirectory Descriptions**:
  - `world/` - World-building settings (geography, history, culture).
  - `rules/` - Rule-based knowledge (power systems, laws).
  - `characters/` - Character profiles (in-depth character settings).
  - `research/` - Research materials (references, sources of inspiration).
- **Upgrade Strategy**: **Never overwritten**, user content is fully preserved.

### Tracking Layer (Runtime Data)

**`tracking/`** - Tracking data
- **Responsibility**: Stores dynamic tracking data from the creative process.
- **Characteristics**: Continuously updated as the creation progresses.
- **Examples**:
  - `plot-tracker.json` - Records plot progression.
  - `character-state.json` - Records the current state of characters.
  - `relationships.json` - Records the character relationship network.
  - `timeline.json` - Records the timeline of events.
- **Upgrade Strategy**: **Never overwritten**, user data is fully preserved.

---

## 🔍 Query Protocol (Recommended)

When executing different commands, the AI should query the relevant files in the following order to ensure a complete context and correct priority.

### Creative Preparation Phase

**Applicable Commands**: `/constitution`, `/specify`, `/clarify`

**Query Order**:
1. **First, check**: `memory/novel-constitution.md` (Creative constitution - highest principle).
2. **Then, check**: `memory/style-reference.md` (Style reference - if it exists).
3. **Finally, check**: `spec/presets/` (Writing method presets).

**Purpose**: To ensure that the creative principles and style guidelines are followed when defining the story specifications.

---

### Planning Phase

**Applicable Commands**: `/plan`, `/tasks`

**Query Order**:
1. **First, check**: `memory/novel-constitution.md` (Creative principles).
2. **Then, check**: `stories/*/specification.md` (Story specifications).
3. **Then, check**: `spec/presets/golden-opening.md` (If in the early planning stage).
4. **Finally, check**: `spec/knowledge/` (Knowledge base).

**Purpose**: To create a writing plan that conforms to the specifications and principles.

---

### Specific Writing Phase

**Applicable Command**: `/write`

**Query Order (Important!)**:
1. **First, check**: `memory/novel-constitution.md` (Creative constitution).
2. **Then, check**: `memory/style-reference.md` (Style reference - if generated via `/book-internalize`).
3. **Then, check**: `stories/*/specification.md` (Story specifications).
4. **Then, check**: `stories/*/creative-plan.md` (Creative plan).
5. **Then, check**: `stories/*/tasks.md` (Current tasks).
6. **Then, check**: `spec/tracking/` related files:
   - `character-state.json` (Character states).
   - `relationships.json` (Relationship network).
   - `plot-tracker.json` (Plot tracking).
7. **Then, check**: `spec/knowledge/` related files (world-building, character profiles).
8. **Then, check**: `spec/presets/anti-ai-detection.md` (Anti-AI detection guidelines).
9. **Conditional query**: If it's the first three chapters, additionally query `spec/presets/golden-opening.md`.

**Purpose**: To ensure a complete context during writing, conforming to all guidelines and existing settings.

---

### Quality Validation Phase

**Applicable Commands**: `/analyze`, `/checklist`, `/track`

**Query Order**:
1. **First, check**: `memory/novel-constitution.md` (Check for compliance against the constitution).
2. **Then, check**: `stories/*/specification.md` (Check for completeness against the specifications).
3. **Then, check**: `stories/*/creative-plan.md` (Check for execution against the plan).
4. **Then, check**: all files in `spec/tracking/` (Check for consistency).
5. **Then, check**: `spec/checklists/` (Use standardized checklists).
6. **Conditional query**: If it's the first three chapters, use the self-check list from `spec/presets/golden-opening.md`.

**Purpose**: To comprehensively validate content quality, identify issues, and provide suggestions for improvement.

---

## ⚙️ Rule Priority

When rules from different files conflict, handle them according to the following priority:

### Priority Order (from highest to lowest)

1. **User's Immediate Instructions** (Highest priority)
   - Specific requirements in the user's command.
   - **Example**: "Don't use the golden opening formula for this chapter; I want to build it up slowly."
   - **Effect**: Overrides all preset rules.

2. **Creative Constitution** (`memory/novel-constitution.md`)
   - The project's highest creative principles.
   - **Example**: "This work prohibits the depiction of violent scenes."
   - **Effect**: Overrides all preset guidelines.

3. **Style Reference** (`memory/style-reference.md`)
   - Style guidelines from a reference work.
   - **Example**: "Use short sentences and avoid ornate metaphors."
   - **Effect**: Influences the specific writing style.

4. **Story Specifications** (`stories/*/specification.md`)
   - Specific requirements for the current story.
   - **Example**: "The target audience is 15-25 year old males."
   - **Effect**: Influences content positioning and style.

5. **Writing Method Presets** (`spec/presets/`)
   - Standardized writing guidelines.
   - **Examples**: `anti-ai-detection.md`, `golden-opening.md`.
   - **Effect**: Provides basic guidelines and best practices.

6. **Knowledge Base and Tracking Data** (`spec/knowledge/`, `spec/tracking/`)
   - Existing settings and states.
   - **Example**: A character is already dead and cannot appear again.
   - **Effect**: Ensures consistency.

### Conflict Resolution Examples

**Scenario 1**: Golden Opening vs. Creative Constitution
- **Conflict**: `golden-opening.md` requires a direct conflict in the first chapter, but `constitution.md` calls for a "slow burn, with an emphasis on atmosphere."
- **Resolution**: Follow `constitution.md` and modify the opening strategy.
- **Basis**: The creative constitution has a higher priority than preset guidelines.

**Scenario 2**: Style Reference vs. Anti-AI Detection
- **Conflict**: `style-reference.md` says the reference work uses ornate metaphors, but `anti-ai-detection.md` says to avoid them.
- **Resolution**: Prioritize `style-reference.md`, but use ornate metaphors with moderate restraint.
- **Basis**: The style reference has a slightly higher priority but needs to be balanced.

**Scenario 3**: User's Immediate Instruction vs. All Presets
- **Conflict**: The user says, "Use a lot of environmental descriptions to build up the atmosphere in this chapter," but all guidelines say to limit descriptions.
- **Resolution**: Follow the user's instruction completely.
- **Basis**: The user's immediate instruction has the highest priority.

---

## 🚀 Best Practices

### 1. During Project Initialization

Recommended creation order:
1. Run `/constitution` to create `memory/novel-constitution.md`.
2. (Optional) Run `/book-analyze` + `/book-internalize` to generate `memory/style-reference.md`.
3. Run `/specify` to create `stories/*/specification.md`.
4. Run `/plan` to create `stories/*/creative-plan.md`.
5. Before starting to write, manually create the relevant files in `spec/knowledge/` (world-building, characters, etc.).

### 2. During the Creative Process

**Before each writing session**:
- Ensure the data in `spec/tracking/` is up-to-date.
- Check if any new characters or settings need to be recorded in `spec/knowledge/`.

**After every 5 chapters**:
- Run `/analyze` for a quality check.
- Run `/track` to update tracking data.
- If necessary, run `/checklist` for a specific check.

### 3. During Version Upgrades

**Safe Upgrade**:
```bash
novel upgrade
```

**Upgrade Strategy**:
- `spec/presets/` - Will be updated (with new and better guidelines).
- `spec/checklists/` - Will be updated (with new checklist items).
- `spec/config.json` - Will be merged and updated (preserving your custom configurations).
- `spec/knowledge/` - **Never overwritten**.
- `spec/tracking/` - **Never overwritten**.

### 4. Managing Multiple Projects

If you are writing multiple works at the same time:
- Each work has its own independent `spec/` directory.
- However, guidelines in `presets/` can be shared (via symlinks or by copying).
- `knowledge/` and `tracking/` must be kept separate for each.

---

## 📝 File Naming Conventions

### `knowledge/` Directory

**Recommended Naming**:
- World-building: `world/geography.md`, `world/history.md`
- Rules: `rules/power-system.md`, `rules/magic-rules.md`
- Characters: `characters/protagonist.md`, `characters/villain.md`
- Research: `research/[work-title]-analysis.md`

### `tracking/` Directory

**Fixed Naming** (generated by the system, do not modify):
- `plot-tracker.json`
- `character-state.json`
- `relationships.json`
- `timeline.json`
- `validation-rules.json`

### `checklists/` Directory

**Recommended Naming**:
- Specification-related: `specification-*.md`
- Content-related: `plot-*.md`, `character-*.md`, `world-*.md`
- Style-related: `style-*.md`, `dialogue-*.md`

---

## 🔧 Advanced Tips

### Tip 1: Use Symlinks to Share Presets

If you have multiple projects and want to share the same writing method presets:

```bash
# In project A
cd project-a/spec
ln -s /path/to/shared-presets presets

# Project B can do the same
cd project-b/spec
ln -s /path/to/shared-presets presets
```

### Tip 2: Create a Personal Guideline Library

Create a `~/.novel-writer/presets/` directory to store your personally summarized writing guidelines, and then reference them in your projects.

### Tip 3: Use Git for Version Control

It is highly recommended to include the `spec/` directory in Git version control:

```bash
# .gitignore
spec/tracking/*.json  # Tracking data is not included in version control
spec/knowledge/      # Knowledge base can be selectively included

# But these should be included:
spec/presets/
spec/checklists/
spec/config.json
```

---

## ❓ Frequently Asked Questions

### Q1: What's the difference between `knowledge/` and `tracking/`?

**A**:
- `knowledge/` is for "static knowledge," such as basic character settings and world-building rules, which do not change frequently.
- `tracking/` is for "dynamic states," such as a character's current location, relationships, and emotions, which may change with each chapter.

### Q2: Can I delete unused methods from `presets/`?

**A**:
You can, but it is not recommended. Keeping them does not take up much space, and you might need them in the future. If you do decide to delete them, remember to back them up.

### Q3: What should I do if `presets/` is overwritten after an upgrade?

**A**:
This is normal. The `presets/` directory is designed to be safely overwritten. If you have made custom modifications to a preset, you should:
1. Copy it to the `memory/` or `knowledge/` directory.
2. Or rename it (e.g., `anti-ai-detection-custom.md`).

### Q4: Can I create my own presets?

**A**:
Of course! You can create any `.md` file you need in the `presets/` or `memory/` directories, and the AI will discover them when it reads the directories.

---

## 📚 Related Documents

- **Creative Workflow Guide**: `docs/workflow.md`
- **Command Details**: `docs/commands.md`
- **Best Practices**: `docs/best-practices.md`
- **Upgrade Guide**: `docs/upgrade-guide.md`

---

**Version**: v1.0.0
**Last Updated**: 2025-01-14
**Maintained by**: Novel Writer Team
