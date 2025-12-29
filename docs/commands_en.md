# Novel Writer Slash Command Reference

This document provides a detailed explanation of all the slash commands available in Novel Writer, including their usage, parameters, and best practices.

## Namespace Explanation

Different AI platforms use different command formats:

| AI Platform | Command Format | Example |
|---|---|---|
| **Claude Code** | `/novel.command_name` | `/novel.write` |
| **Gemini CLI** | `/novel/command_name` | `/novel/write` |
| **Cursor, Windsurf, Roo Code**, etc. | `/command_name` | `/write` |

> 💡 **Tip**: This document uses the generic format `/command_name`. Please add the appropriate namespace prefix based on your AI platform when using them.

---

## Seven-Step Methodology Commands

A systematic creation process based on Specification-Driven Development (SDD).

### 1. `/constitution` - Writing Constitution

**Purpose**: Defines the highest-level creative principles and values.

**When to use**: At the beginning of a project, or when the core creative philosophy needs adjustment.

**Generated Content**: `.specify/memory/constitution.md`

**Includes**:
- Core creative philosophy
- Values and taboos
- Quality standards
- Style preferences

**Example Usage**:
```
/novel.constitution Create writing principles, emphasizing:
1. Character development takes precedence over plot progression.
2. Authenticity is more important than drama.
3. Details should be specific and vivid.
4. Avoid preaching and deliberate sentimentality.
```

**Best Practices**:
- The constitution is the "supreme law" for all subsequent decisions.
- Keep the principles clear and actionable.
- Don't be too verbose; 5-10 core principles are sufficient.
- Review and update periodically as needed.

---

### 2. `/specify` - Story Specification

**Purpose**: Defines the story with the precision of a Product Requirements Document (PRD).

**When to use**: After establishing the constitution, to define the specific story content.

**Generated Content**: `stories/001-Story-Name/specification.md`

**Includes**:
- Story concept and core conflict
- Target audience and market positioning
- Main characters and world-building
- Plotline management specifications (v0.12.0+)
- Success criteria and acceptance conditions

**Example Usage**:
```
/novel.specify Create a specification for an urban fantasy novel:
- Core concept: A society of superpowered individuals hidden in a modern city.
- Protagonist: A 25-year-old programmer who unexpectedly awakens the ability of precognition.
- Core conflict: The protagonist gets caught in a power struggle between superpowered organizations.
- Target audience: 18-35 years old, fans of superpowers and suspense.
```

**Plotline Management** (New in v0.12.0):
Chapter 5 of the specification document contains complete tables for plotline management:
- **5.1 Plotline Definition Table**: Defines all plotline IDs, types, and priorities.
- **5.2 Plotline Pacing Plan**: Plans the activity level of each plotline (⭐⭐⭐/⭐⭐/⭐).
- **5.3 Plotline Intersection Point Plan**: Pre-plans intersection timing to prevent the AI from improvising.
- **5.4 Foreshadowing Management Table**: Manages the setup and payoff of foreshadowing.
- **5.5 Plotline Modification Decision Matrix**: An impact assessment checklist for modifications.

**Best Practices**:
- Focus on "what" and "why," not "how."
- Use clear acceptance criteria.
- Write detailed profiles for key characters.
- Clearly define the core conflict and theme.

---

### 3. `/clarify` - Clarify Decisions

**Purpose**: Clarifies ambiguous points in the story specification through an interactive Q&A.

**When to use**: After completing the initial version of the specification, before creating the plan.

**How it works**:
1. The AI automatically identifies unclear parts of the specification.
2. It asks 5 precise questions to clarify them.
3. It records all decisions and their rationales.
4. It updates the specification document.

**Example Usage**:
```
/novel.clarify
```

The AI might ask:
- "What are the limitations of the protagonist's precognition? Range, accuracy, cost?"
- "What is the structure of the superpowered organizations? How many factions are there?"
- "What is the time span of the story? How many chapters are expected?"

**Best Practices**:
- Answer each question thoroughly; don't be vague.
- Record the reasons behind your decisions.
- If some details are genuinely uncertain, state that clearly.
- Check the updated specification document after completion.

---

### 4. `/plan` - Creative Plan

**Purpose**: Develops a technical implementation plan based on the clarified specifications.

**When to use**: After the specifications have been clarified.

**Generated Content**: `stories/001-Story-Name/creative-plan.md`

**Includes**:
- Chapter structure design (with active plotline markers)
- Choice of narrative techniques
- Pacing control plan
- Foreshadowing setup plan

**Example Usage**:
```
/novel.plan Technical solution:
- Use a three-act structure.
- Act One: Chapters 1-20, establish the world and the protagonist's abilities.
- Act Two: Chapters 21-70, the protagonist gets involved in the power struggle.
- Act Three: Chapters 71-100, climax and resolution.
- Use a first-person perspective to enhance immersion.
- Set up a minor climax every 5 chapters.
```

**Enhanced Chapter Section Table** (v0.12.0):
- **Active Plotlines** column: Marks which plotlines are advanced in each chapter section (⭐⭐⭐ main progression / ⭐⭐ auxiliary / ⭐ background).
- **Intersection Point** column: Specifies the chapter where an intersection point occurs.

**Best Practices**:
- Plan the implementation like a software architecture design.
- Consider pacing and the balance of tension and release.
- Mark key turning points.
- Plan the setup and payoff for foreshadowing.

---

### 5. `/tasks` - Task Decomposition

**Purpose**: Breaks down the creative plan into an executable task list.

**When to use**: After the creative plan is complete.

**Generated Content**: `stories/001-Story-Name/tasks.md`

**Includes**:
- Chapter writing tasks (priority, estimated word count, involved plotlines)
- Character development tasks
- World-building expansion tasks
- Quality check tasks

**Example Usage**:
```
/novel.tasks
```

**Enhanced Task Format** (v0.12.0):
Each writing task includes:
- **Involved Plotlines**: Which plotlines this chapter advances and their priority (PL-01⭐⭐⭐, PL-02⭐⭐).
- **Intersection Point**: Whether this chapter is an intersection point (e.g., X-001).
- **Foreshadowing Setup/Payoff**: Foreshadowing actions involved in this chapter.

**Best Practices**:
- Tasks should be specific and actionable.
- Set reasonable priorities.
- Estimate word counts to help control pacing.
- Mark dependencies between tasks.

---

### 6. `/write` - Chapter Writing

**Purpose**: AI-assisted creation based on the task list.

**When to use**: After the task list is complete, to start the actual writing.

**Generated Content**: `stories/001-Story-Name/content/Chapter-File.md`

**How it works**:
- Creates content based on the specification and plan.
- Follows the principles of the writing constitution.
- Maintains style and character consistency.
- Automatically references personal corpus (if configured).
- Validates character names after writing (v0.6.0+).

**Example Usage**:
```
/novel.write Chapter 3: Awakening
Content: The protagonist awakens his precognitive ability during an accident and foresees an impending car crash.
Requirements:
- Focus on the protagonist's psychological changes.
- The awakening process should be tense.
- Word count: 2000-2500 words.
```

**Character Consistency Validation** (v0.6.0):
- Before writing: The AI will remind you to pay attention to character names and titles.
- After writing: It automatically validates the character names in the chapter.
- If errors are found, it will suggest corrections.

**Authentic Voice Support** (v0.8.4):
If `.specify/memory/personal-voice.md` exists, it will automatically reference the personal corpus to ensure natural expression. Recommended to be used with the `/authentic-voice` plugin.

**Best Practices**:
- Write one chapter at a time to maintain focus.
- Clearly define the core task of the chapter.
- Pay attention to continuity with previous content.
- Immediately validate the quality after writing.

---

### 7. `/analyze` - Intelligent Comprehensive Validation

**Purpose**: Automatically selects the analysis type based on the creation stage.

**When to use**:
- **Framework Analysis**: Before `write`, to validate preparation.
- **Content Analysis**: After `write` (≥3 chapters), to validate the quality of the work.

**Intelligent Mode Switching** (v0.12.1):

#### Mode A: Framework Analysis (Chapter count < 3)
**Validation Content**:
- Coverage analysis: Do all spec requirements have corresponding plans and tasks?
- Consistency check: Are there any contradictions between the spec, plan, and tasks?
- Logical warnings: Potential loopholes in the story design.
- Readiness assessment: Is it ready to start writing?

**Example Usage**:
```
/novel.analyze
```
Or force a specific type:
```
/novel.analyze --type=framework
```

#### Mode B: Content Analysis (Chapter count ≥ 3)
**Validation Content**:
- Constitution compliance: Does it follow the creative principles?
- Specification alignment: Does it meet the specification requirements?
- Content quality: Analysis of logic, characters, and pacing.
- Improvement suggestions: Specific P0/P1/P2 fix suggestions.

**Example Usage**:
```
/novel.analyze
```
Or force a specific type:
```
/novel.analyze --type=content
```

**Best Practices**:
- In 90% of cases, let the AI judge automatically.
- Rerun the analysis after major revisions.
- Take P0-level issues seriously.
- Record and implement the improvement suggestions.

---

## Tracking and Validation Commands

### `/track-init` - Initialize Tracking System

**Purpose**: Initializes the tracking function before its first use.

**When to use**: After completing the story outline, before starting to write.

**Generated Content**:
- `spec/tracking/plot-tracker.json` - Plot tracking
- `spec/tracking/timeline.json` - Timeline
- `spec/tracking/relationships.json` - Relationship matrix
- `spec/tracking/character-state.json` - Character state

**Example Usage**:
```
/novel.track-init
```

**Best Practices**:
- Only needs to be run once.
- Subsequent use of other tracking commands will update automatically.
- Periodically back up the tracking files.

---

### `/checklist` - Quality Checklist ⭐New

**Purpose**: Generates a quality checklist, similar to "unit tests for requirements."

**When to use**:
- **Specification Quality Check**: Before writing, to validate the quality of planning documents.
- **Content Validation Check**: After writing, to scan chapter content.

**Dual-Insurance Mechanism**:
- Type 1: Specification Quality Check (before writing) - Validates the quality of the documents themselves.
- Type 2: Content Validation Check (after writing) - Scans the actual written content.

#### Type 1: Specification Quality Check (Before Writing)

Similar to "unit tests for requirements," validates the quality of planning documents:

| Type | Command | Check Dimensions |
|---|---|---|
| Outline Quality | `/checklist Outline Quality` | Completeness, Clarity, Consistency, Measurability, Coverage |
| Character Setting | `/checklist Character Setting` | Profile completeness, motivation clarity, consistency |
| World-building | `/checklist World-building` | Rule completeness, setting clarity, internal consistency |
| Creative Plan | `/checklist Creative Plan` | Goal clarity, task clarity, plan reasonableness |
| Foreshadowing Mgt | `/checklist Foreshadowing Mgt` | Foreshadowing completeness, payoff clarity, density reasonableness |

**Example Usage**:
```
/novel.checklist Outline Quality
```

**Example Output**:
```markdown
# Outline Quality Checklist

**Check Time**: 2025-10-11 14:30
**Check Object**: outline.md
**Check Dimensions**: Completeness, Clarity, Consistency, Measurability, Coverage

## Completeness

- [ ] CHK001 Are trigger conditions and outcomes defined for each major plot point? [Spec §3.2]
- [x] CHK002 Is the story goal for each volume/chapter clear? [Spec §4.1] ✓
- [!] CHK003 Does it cover the growth arcs of all major characters? [Gap] ⚠️

## Clarity

- [ ] CHK004 Are the trigger conditions for plot points specific and verifiable? [Ambiguity]
- [ ] CHK005 Is there a clear basis for chapter allocation? [Clarity]

## Consistency

- [ ] CHK006 Are there any contradictions in the plot threads? [Consistency]
- [x] CHK007 Are the world-building rules consistent with the outline description? [vs §World-building] ✓

## Issues Found

### CHK003
**Issue**: The growth arc for the supporting character Zhang San is not reflected in the outline.
**Location**: outline.md:45
**Suggestion**: Add a brief growth path for the supporting character in Chapter 3.

## Follow-up Actions

- [ ] Add the growth arc design for the supporting character Zhang San.
- [ ] Check if other supporting characters have similar omissions.
```

#### Type 2: Content Validation Check (After Writing)

Scans written chapters to validate the actual content:

| Type | Command | Check Content |
|---|---|---|
| Worldview Consistency | `/checklist Worldview Consistency` | Scans chapters for setting contradictions |
| Plot Alignment | `/checklist Plot Alignment` | Compares progress with the outline |
| Data Sync | `/checklist Data Sync` | Validates tracking data consistency |
| Timeline | `/checklist Timeline` | Checks for logical continuity in time |
| Writing Status | `/checklist Writing Status` | Checks writing readiness and task status |

**Example Usage**:
```
/novel.checklist Worldview Consistency
```

**Output Location**:
- Specification Quality: `spec/checklists/outline-quality.md` (overwritten)
- Content Validation: `spec/checklists/world-consistency-20251011.md` (with date)

**Checkbox Legend**:
- `[ ]` - Not checked
- `[x]` - Checked, passed
- `[!]` - Checked, issue found

**Workflow**:

**Planning Phase (Before Writing)**:
1. Complete planning documents for outline, characters, world-building, etc.
2. Run specification quality checklists.
3. Improve documents based on the check results.
4. Ensure planning quality is adequate before starting to write.

**Writing Phase (During/After Writing)**:
1. Complete chapter writing.
2. Periodically run content validation checklists (every 5-10 chapters).
3. Find and fix consistency issues.
4. Maintain a history to track improvements.

**Best Practices**:
- **Dual-Insurance**: Validate documents in the planning stage, and content in the writing stage.
- **Early Detection**: Find logical loopholes in the outline stage to avoid discovering them late in the writing process.
- **Regular Checks**: Run content validation every 5-10 chapters.
- **Keep History**: Content validation checklists are saved by date, allowing you to track the discovery and resolution of issues.
- **Clean Up**: Periodically clean up old checklists for resolved issues, keeping records of key milestone checks.

**Backward Compatibility**:
Old commands are still available, but using the unified `/checklist` is recommended:
- `/world-check` → `/checklist Worldview Consistency`
- `/plot-check` → `/checklist Plot Alignment`

**Key Advantages**:
1. **Analogous to Requirements Validation**: Like unit tests in software development, it establishes a quality gate for creative documents.
2. **Layered Validation**: Planning document quality + actual content quality, for double assurance.
3. **Documented Tracking**: All check results are saved in `spec/checklists/`, making it easy to trace back.
4. **Early Warning**: Logical loopholes can be found in the outline stage, preventing rework later.

---

### `/track` - Comprehensive Tracking

**Purpose**: Provides a comprehensive overview of the progress and status of the novel creation.

**When to use**: After completing each chapter.

**Displays**:
- Writing progress (word count, chapters, completion rate)
- Plot development (main plot progress, subplot status)
- Timeline (story time progression)
- Character status (character development and location)
- Foreshadowing management (setup and payoff status)

**Example Usage**:
```
/novel.track
```

**Deep Validation Mode** (v0.6.0):
```
/novel.track --check
```
Batch checks for character name consistency.

**Auto-Fix Mode** (v0.6.0):
```
/novel.track --fix
```
Automatically fixes simple character name errors.

**Best Practices**:
- Update after every chapter.
- Pay attention to deviations from the plan.
- Address warning messages promptly.

---

### `/plot-check` - Plot Check

**Purpose**: Checks the consistency and coherence of the plot development.

**When to use**: Periodically, every 5-10 chapters.

**Validates**:
- Is the current progress consistent with the outline plan?
- The foreshadowing that has been set up and its payoff plan.
- Is the conflict escalation in line with the expected pacing?
- Is character development consistent with the plan?

**Example Usage**:
```
/novel.plot-check
```

**Best Practices**:
- Check periodically, not after every chapter.
- A must-check after major plot twists.
- Record reasons for deviations and adjustment plans.

---

### `/timeline` - Timeline Management

**Purpose**: Maintains and validates the story's timeline.

**When to use**: When dealing with time-related plots.

**Manages**:
- The time point of each chapter.
- Multiple plotlines happening simultaneously.
- Comparison with real historical events (for historical novels).
- Check for the reasonableness of the time span.

**Example Usage**:
```
/novel.timeline
```

**Best Practices**:
- Pay special attention when multiple plotlines are advancing in parallel.
- Mark key time points.
- Check for logical reasonableness of the timeline.

---

### `/relations` - Relationship Tracking

**Purpose**: Manages and tracks changes in character relationships.

**When to use**: When important changes in character relationships occur.

**Tracks**:
- The relationship map between characters.
- The evolution of relationships.
- The opposition and cooperation between factions.
- The emotional development between characters.

**Example Usage**:
```
/novel.relations
```

**Best Practices**:
- Update promptly when important relationships change.
- Maintain the reasonableness of relationships.
- Pay attention to the impact of group relationships.

---

### `/world-check` - World-building Check

**Purpose**: Validates the consistency of the world-building settings.

**When to use**: When introducing new settings or discovering contradictions.

**Validates**:
- Consistency of rules, laws, and systems.
- Reasonableness of locations, distances, and directions.
- Uniformity of customs, languages, and traditions.
- The scope and limitations of abilities.

**Example Usage**:
```
/novel.world-check
```

**Best Practices**:
- Especially important for fantasy and sci-fi.
- Establish clear setting documents.
- Assess the impact of major setting changes.

---

## Expert Mode Commands

Requires initializing the project with `--with-experts`.

### `/expert` - List Available Experts

**Purpose**: Displays all available expert advisors.

**Example Usage**:
```
/novel.expert
```

---

### `/expert plot` - Plot Structure Expert

**Specialty**:
- Plot design
- Pacing control
- Conflict escalation
- Climax setup

**Use Case**:
- The plot feels slow or chaotic.
- Need to design a climax scene.
- Unsure how to advance the plot.

---

### `/expert character` - Character Development Expert

**Specialty**:
- Character arc design
- Motivation analysis
- Dialogue optimization
- Personality consistency

**Use Case**:
- Characters feel flat.
- Dialogue is not lively.
- Character actions are unreasonable.

---

### `/expert world` - World-building Design Expert

**Specialty**:
- World construction
- Setting consistency
- Cultural background
- Magic/technology systems

**Use Case**:
- Building a complex world.
- Settings have contradictions.
- Need to enrich the background.

---

### `/expert style` - Writing Style and Language Expert

**Specialty**:
- Narrative techniques
- Rhetorical devices
- Language style
- Text polishing

**Use Case**:
- The writing is bland.
- Want to improve the literary quality.
- Need a specific style.

---

## Command Usage Tips

### 1. Command Combination Strategies

**Creating a new story**:
```
/novel.constitution → /novel.specify → /novel.clarify →
/novel.plan → /novel.tasks → /novel.track-init → /novel.write
```

**Periodic checks**:
```
Every chapter: /novel.track
Every 5-10 chapters: /novel.plot-check
After key turning points: /novel.analyze
```

**Problem diagnosis**:
```
Plot issues: /novel.plot-check → /novel.expert plot
Character issues: /novel.relations → /novel.expert character
Setting issues: /novel.world-check → /novel.expert world
```

### 2. Prompting Techniques

**A good prompt**:
- Clearly explains the current situation.
- Clearly expresses the modification intent.
- Specifies what needs to be updated.
- Describes the desired output.

**Example**:
```
/novel.write Chapter 10

Situation: The first 9 chapters have completed the protagonist's ability awakening and initial exploration.
Intent: This chapter needs to introduce the villainous organization to create tension.
Needs to be updated: Create the content for Chapter 10, and set up two foreshadowing.
Expected output: 2500 words, first-person perspective, suspenseful atmosphere.
```

### 3. Version Control Suggestions

- Create a Git branch before major revisions.
- Commit regularly at key milestones.
- Keep backups of important versions.
- Record the reasons for important decisions.

---

## Frequently Asked Questions

### Q: Do I have to execute all commands in order?

A: The seven-step methodology suggests executing them in order, but you can skip or repeat steps as needed. For example, a small modification might only require `/write` and `/analyze`.

### Q: How to handle deviations when the AI is writing?

A:
1. Small deviation and good quality: Accept the deviation and update the specification and plan.
2. Large deviation: Use `/analyze` to diagnose, then update the corresponding documents.
3. Refer to the practical examples in `docs/writing/practical-guide.md`.

### Q: Are the tracking commands slow?

A: The tracking commands use programmatic validation, which is much faster than relying entirely on AI analysis. It is recommended to update after each chapter, and there should be no noticeable delay.

### Q: What's the difference between expert mode and normal commands?

A: Expert mode provides more in-depth professional guidance and is suitable for when you encounter specific problems. Normal commands focus more on process and consistency.

---

## Related Documents

- [Workflow](workflow.md) - A complete guide to the creation workflow
- [Writing Methods](writing-methods.md) - 6 classic writing methods
- [Best Practices](best-practices.md) - Practical experience and tips
- [Upgrade Guide](upgrade-guide.md) - Version upgrade instructions
