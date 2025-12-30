---
name: checklist
description: Generate or execute quality checklists (specification validation + content scanning)
allowed-tools: Read, Bash, Write, Edit, Glob, Grep
model: claude-sonnet-4-5-20250929
scripts:
  sh: scripts/bash/common.sh
  ps: scripts/powershell/common.ps1
---

# Quality Checklist

Generates or executes a quality checklist, supporting two modes:

## 🎯 Supported Check Types

### Type 1: Specification Quality Check (Question-Generative)
Validates the quality of the planning documents themselves (like "unit tests for requirements"):

- `Outline Quality` - Checks the completeness, clarity, and consistency of outline.md.
- `Character Settings` - Checks spec/knowledge/characters.md.
- `World-building` - Checks spec/knowledge/world-setting.md and related documents.
- `Creative Plan` - Checks creative-plan.md / specification.md.
- `Foreshadowing Management` - Checks the foreshadowing definitions in spec/tracking/plot-tracker.json.

### Type 2: Content Validation Check (Report-Generative)
Scans written chapters to validate the actual content:

- `World-building Consistency` - Scans chapter content for contradictions in world-building descriptions.
- `Plot Alignment` - Compares progress with the outline to check plot development.
- `Data Sync` - Verifies the synchronization of all tracking JSON files.
- `Timeline` - Checks the logical continuity of time events.
- `Writing Status` - Checks writing readiness and task status.

## User Input

```text
$ARGUMENTS
```

## Execution Flow

### 1. Identify Check Type

Based on user input, determine the check type (Specification Quality vs. Content Validation):

**Keyword Mapping**:
- "outline", "quality" → Spec Quality: Outline Quality
- "character", "setting" → Spec Quality: Character Settings
- "world-building" + "quality/completeness/spec" → Spec Quality: World-building
- "world-building" + "consistency/check/scan" → Content Validation: World-building Consistency
- "creative plan", "planning" → Spec Quality: Creative Plan
- "foreshadowing" → Spec Quality: Foreshadowing Management
- "plot", "alignment", "progress" → Content Validation: Plot Alignment
- "data", "sync", "consistency" → Content Validation: Data Sync
- "timeline", "time" → Content Validation: Timeline
- "writing status", "readiness" → Content Validation: Writing Status

If the user input is unclear, ask for a selection.

### 2. Execute Corresponding Check Logic

#### Specification Quality Checks (Generates a question-based Checklist)

Executes a requirements quality validation logic similar to spec-kit:

##### 2.1 Outline Quality Check

**Objective**: To verify that outline.md has good completeness, clarity, and consistency.

**Read Files**:
- `outline.md` or `stories/*/outline.md`
- `spec/tracking/plot-tracker.json` (if it exists)

**Generate Check Item Dimensions**:

**Completeness**:
- Are trigger conditions and outcomes defined for each major plot node?
- Is the story goal for each volume/chapter clearly defined?
- Does it cover the growth arcs of all major characters?
- Is the escalation path of the main conflict defined?
- Are the story's climax and ending clearly defined?

**Clarity**:
- Are the trigger conditions for plot nodes specific and verifiable?
- Is there a clear basis for chapter allocation (e.g., word count, plot density)?
- Are character motivations quantified with specific events?
- Do scene descriptions avoid vague words ("someplace", "a period of time")?

**Consistency**:
- Are there contradictions in the plot threads?
- Is character behavior consistent with their settings?
- Is the time span reasonable?
- Are the world-building rules consistent with the outline's description?

**Measurability**:
- Is the chapter allocation reasonable and feasible (e.g., 2000-4000 words per chapter)?
- Is the timing for resolving foreshadowing clear (chapter number or range)?
- Does character growth have clear milestones?

**Coverage**:
- Have all major scene types been considered (conflict, daily life, turning points)?
- Is the screen time of all major characters covered?
- Does it include necessary foreshadowing setup and resolution?

**Example Output Format**:
```markdown
# Outline Quality Checklist
**Creation Date**: 2025-10-11
**Check Object**: outline.md
**Check Dimensions**: Completeness, Clarity, Consistency, Measurability, Coverage

## Completeness

- [ ] CHK001 Are trigger conditions and outcomes defined for each major plot node? [Spec §Outline 3.2]
- [ ] CHK002 Is the story goal for each volume/chapter clearly defined? [Gap]
- [ ] CHK003 Does it cover the growth arcs of all major characters? [Spec §Outline 5.1]

## Clarity

- [ ] CHK004 Are the trigger conditions for plot nodes specific and verifiable? [Ambiguity, Spec §Outline 3.2]
- [ ] CHK005 Is there a clear basis for chapter allocation? [Clarity]

## Consistency

- [ ] CHK006 Are there contradictions in the plot threads? [Consistency]
- [ ] CHK007 Are the world-building rules consistent with the outline's description? [Consistency, vs §World-building]

## Measurability

- [ ] CHK008 Is the chapter allocation reasonable and feasible (e.g., 2000-4000 words per chapter)? [Measurability]
- [ ] CHK009 Is the timing for resolving foreshadowing clear (chapter number or range)? [Gap]

## Coverage

- [ ] CHK010 Have all major scene types been considered? [Coverage]
- [ ] CHK011 Does it include necessary foreshadowing setup and resolution? [Coverage, Gap]

## Instructions for Use

Tick verified items: `[x]`
Mark items with issues: `[!]` and record the specific issue below.
```

##### 2.2 Character Setting Check

**Read Files**:
- `spec/knowledge/characters.md`
- `spec/tracking/character-state.json`
- `spec/tracking/relationships.json`

**Generate Check Item Dimensions**:

**Completeness**:
- Is basic information (name, age, identity, appearance) defined for major characters?
- Are the character's core motivations and goals defined?
- Are the character's personality traits and behavioral patterns defined?
- Is the character's backstory defined?
- Are the character's abilities and limitations defined?

**Clarity**:
- Are character motivations specific and verifiable (not "wants to succeed" but "wants to change the family's fate by passing the imperial exam")?
- Are personality traits demonstrated through specific actions?
- Are character goals quantifiable or have clear achievement standards?

**Consistency**:
- Is the character's setting consistent with their behavior in the outline?
- Are character descriptions consistent across different documents?
- Are relationship definitions symmetrical (A's relationship to B vs. B's relationship to A)?

**Measurability**:
- Does character growth have clearly defined stages?
- Are changes in character abilities trackable?

##### 2.3 World-building Check

**Read Files**:
- `spec/knowledge/world-setting.md`
- `spec/knowledge/locations.md`
- `spec/knowledge/culture.md`
- `spec/knowledge/rules.md`

**Generate Check Item Dimensions**:

**Completeness**:
- Are the core world-building rules (magic system, tech level, social structure) defined?
- Are the main locations and their features defined?
- Are cultural customs, languages, and traditions defined?
- Is the time period and historical context defined?

**Clarity**:
- Are the world-building rules clear and unambiguous?
- Are geographical locations, distances, and directions clear?
- Are special terms clearly defined?

**Consistency**:
- Are world-building settings consistent across different documents?
- Do the world-building rules have internal contradictions?
- Is it consistent with the outline's description?

**Coverage**:
- Does it cover all locations involved in the story?
- Are all special rules or abilities that appear defined?

##### 2.4 Creative Plan Check

**Read Files**:
- `creative-plan.md` or `specification.md`
- `tasks.md`

**Generate Check Item Dimensions**:

**Completeness**:
- Are creative goals and milestones defined?
- Is the creative process and its steps clearly defined?
- Are quality standards defined?

**Clarity**:
- Is the task breakdown clear and specific?
- Is the schedule reasonable?
- Are the acceptance criteria clear?

**Consistency**:
- Does the plan match the scale of the outline?
- Do the tasks cover all planned content?

##### 2.5 Foreshadowing Management Check

**Read Files**:
- `spec/tracking/plot-tracker.json`
- `outline.md`

**Generate Check Item Dimensions**:

**Completeness**:
- Are all planned foreshadowing events recorded?
- Is a setup chapter and a resolution chapter defined for each foreshadowing?
- Is the type and importance of the foreshadowing defined?

**Clarity**:
- Is the foreshadowing content clearly described?
- Is the resolution method clear?

**Measurability**:
- Is there a clear chapter number or range for the resolution timing?
- Is the setup density defined (to avoid too many unresolved foreshadowing events)?

**Consistency**:
- Does the foreshadowing match the plot of the outline?
- Are the `planted` and `resolved` fields consistent?

#### Content Validation Checks (Executes a script to generate a report)

These checks require scanning the actual written content and calling the corresponding bash scripts:

##### 2.5 World-building Consistency Check

Execute command:
```bash
bash scripts/bash/check-world.sh --checklist
```

If the script does not exist, inform the user that this feature is under development.

##### 2.6 Plot Alignment Check

Execute command:
```bash
bash scripts/bash/check-plot.sh --checklist
```

##### 2.7 Data Sync Check

Execute command:
```bash
bash scripts/bash/check-consistency.sh --checklist
```

##### 2.8 Timeline Check

Execute command:
```bash
bash scripts/bash/check-timeline.sh check --checklist
```

##### 2.9 Writing Status Check

Execute command:
```bash
bash scripts/bash/check-writing-state.sh --checklist
```

### 3. Output Checklist

**Save Location**: `spec/checklists/`

**File Naming Convention**:
- Spec Quality type: `[type]-quality.md` (e.g., `outline-quality.md`)
- Content Validation type: `[type]-[date].md` (e.g., `world-consistency-20251011.md`)

**Output Format**: Use `templates/checklist-template.md` as a template.

### 4. Report Results

Output:
- Path to the Checklist file
- Total number of check items
- Check type and scope
- Instructions on how to use the checklist

## Example Usage

```bash
# Specification Quality Checks
/checklist Outline Quality
/checklist Character Settings
/checklist World-building

# Content Validation Checks
/checklist World-building Consistency
/checklist Plot Alignment
/checklist Data Sync
```

## Notes

1.  **Specification Quality Checklist**: Used for pre-writing planning validation to find quality issues in the documents themselves.
2.  **Content Validation Checklist**: Used for post-writing content checks to find issues in the actual output.
3.  The two types of checklists are complementary. Recommendation: Use the first type during the planning phase and the second type during the writing phase.
4.  All checklists are saved in the `spec/checklists/` directory for easy tracking of historical check records.

## Backward Compatibility Note

The old commands `/world-check` and `/plot-check` are still available, but it is recommended to use the unified `/checklist` command.
