---
description: Decomposes the creative plan into an executable task list
allowed-tools: Read(//stories/**/creative-plan.md), Read(stories/**/creative-plan.md), Read(//stories/**/specification.md), Read(stories/**/specification.md), Write(//stories/**/tasks.md), Write(stories/**/tasks.md), Bash(find:*), Bash(*)
model: claude-sonnet-4-5-20250929
scripts:
  sh: .specify/scripts/bash/tasks-story.sh
  ps: .specify/scripts/powershell/generate-tasks.ps1
---

Generates a specific, executable task list based on the creative plan.

## Objective

To transform a macro-level plan into micro-level tasks, making the creative process manageable and trackable.

## Execution Steps

### 1. Load Plan Documents

Run `{SCRIPT}` to load:
- Creative Plan: `stories/*/creative-plan.md`
- Chapter structure information
- Timelines and dependencies

### 2. Generate Task List

Create `stories/*/tasks.md`, including:

#### Core Writing Tasks

**Important**: From Chapter 5 of specification.md and the plotline distribution in creative-plan.md, tag each writing task with the relevant plotlines.

```markdown
## Writing Tasks

### High Priority [Must be completed first]

- [ ] [P0] **T001** - Chapter 1: [Chapter Title] (Target word count)
  - **Core Task**: [Main task for this chapter]
  - **Key Plot Points**: [Specific plot points]
  - **Involved Plotlines**:
    - PL-XX([Plotline Name]) ⭐⭐⭐ Main Progression
    - PL-YY([Plotline Name]) ⭐ Background
  - **Intersection Point**: [If applicable] X-001([Intersection Point Description])
  - **Foreshadowing (Setup/Reveal)**: [If applicable] F-001 Setup / F-002 Reveal
  - **Must Include**: [List of key elements]
  - **End-of-Chapter Hook**: [Suspense setup]
  - **Dependencies**: None
  - **Output**: `content/volume1/chapter-001.md`

- [ ] [P0] **T002** - Character Profile: Detailed protagonist setting
  - **Core Task**: Complete the protagonist's profile
  - **Must Include**: Personality, background, abilities, desires, fears, growth arc
  - **Dependencies**: None
  - **Output**: `characters/protagonist.md`

### Medium Priority [Normal progression]

- [ ] [P1] **T005** - Chapter 5: [Chapter Title] (Target word count)
  - **Core Task**: [Main task for this chapter]
  - **Key Plot Points**: [Specific plot points]
  - **Involved Plotlines**:
    - PL-01([Plotline Name]) ⭐⭐⭐ Main Progression
    - PL-02([Plotline Name]) ⭐⭐ Supporting
  - **Intersection Point**: None
  - **Foreshadowing (Setup/Reveal)**: F-001 Setup ([Foreshadowing Content])
  - **Must Include**: [Key elements]
  - **End-of-Chapter Hook**: [Suspense]
  - **Dependencies**: T004 (Chapter 4)
  - **Output**: `content/volume1/chapter-005.md`

### Low Priority [Optional enhancements]

- [ ] [P2] Extra: Character prequel
- [ ] [P2] Setting Collection: Detailed world-building
```

**Task Field Descriptions**:
- **Involved Plotlines**: Read from the "Active Plotlines" column in creative-plan.md, indicating which plotlines are advanced in this chapter.
- **Intersection Point**: Read from section 5.3 of specification.md, indicating if this chapter is an intersection point.
- **Foreshadowing (Setup/Reveal)**: Read from section 5.4 of specification.md, indicating the foreshadowing operations in this chapter.

#### Task Marker Descriptions
- `[P]` - Can be executed in parallel
- `[Depends on:X]` - Requires task X to be completed first
- `[P0/P1/P2]` - Priority marker

### 3. Task Sorting and Grouping

- Group by priority
- Identify dependencies
- Mark parallelizable tasks
- Estimate completion time

### 4. Generate Execution Plan

```markdown
## Execution Plan

### Phase 1 (Week 1)
Parallel Task Group 1:
- Protagonist setting [P]
- World-building basics [P]
- Chapter 1 draft [P]

### Phase 2 (Weeks 2-3)
Sequential Tasks:
- Chapter 2 [Depends on:Chapter 1]
- Chapter 3 [Depends on:Chapter 2]

Parallel Task Group 2:
- Supporting character setting [P]
- Scene design [P]
```

### 5. Output Task Statistics

- Total number of tasks
- Estimated total word count
- Estimated completion time
- Key milestones

Suggests the next step: start executing `/write` or view specific task details.
