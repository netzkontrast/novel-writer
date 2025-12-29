---
description: Develop a technical implementation plan based on story specifications
argument-hint: [Technical preferences and choices]
allowed-tools: Read(//stories/**/specification.md), Read(stories/**/specification.md), Read(//stories/**/creative-plan.md), Read(stories/**/creative-plan.md), Read(//plugins/**), Read(plugins/**), Write(//stories/**/creative-plan.md), Write(stories/**/creative-plan.md), Read(//memory/constitution.md), Read(memory/constitution.md), Bash(find:*), Bash(grep:*), Bash(*)
model: claude-sonnet-4-5-20250929
scripts:
  sh: .specify/scripts/bash/plan-story.sh
  ps: .specify/scripts/powershell/plan-story.ps1
---

User Input: $ARGUMENTS

## Objective

To transform "what to create" (specifications) into "how to create" (plan). This is the key transition from requirements to implementation.

## Execution Steps

### 1. Load Prerequisite Documents

Run `{SCRIPT}` to check and load:
- Constitution file: `.specify/memory/constitution.md`
- Specification file: `stories/*/specification.md`
- Clarification log (if `/clarify` has been run)

<!-- PLUGIN_HOOK: genre-knowledge-plan -->
<!-- Plugin Enhancement Area: Knowledge Search
     If you have the genre-knowledge plugin installed, insert the knowledge search enhancement prompt here.
     Reference: "2.2 Enhance the /plan command" section in plugins/genre-knowledge/README.md
-->

**🆕 Conditional Loading: Golden Opening Rules**:

**Condition Check**:
1. Check "Target Word Count" or "Total Chapters" in specification.md.
2. Check if the current planning is for the opening phase.
3. Basis for judgment:
   - If total word count is < 10,000 words, OR
   - If the planned chapter range includes chapters 1-3.

**If the opening conditions are met, perform the following**:

```bash
# Check if the golden opening rules file exists
test -f spec/presets/golden-opening.md && echo "found" || echo "not-found"
```

- ✅ **If it exists**: Read `spec/presets/golden-opening.md`.
  - Automatically apply the five golden rules when planning chapters 1-3.
  - Specifically note the planning for the first three chapters in the subsequent "Chapter Architecture Design" section.

- ⚠️ **If it does not exist**: Continue with normal planning (does not affect the process).

**🆕 Conditional Loading: Pacing Configuration**:

If the user has used the `/book-internalize` command to analyze a reference work:

```bash
# Check if the pacing configuration file exists
test -f spec/presets/rhythm-config.json && echo "found" || echo "not-found"
```

- ✅ **If it exists**: Read `spec/presets/rhythm-config.json`.
  - Apply the pacing model of the reference work (chapter word count, thrill point intervals, etc.).
  - Apply content ratio suggestions (dialogue/action/description/psychology).
  - Reference this data in "2.2 Chapter Architecture Design".

- ⚠️ **If it does not exist**: Use default pacing planning.

**Verify Specification Clarification Status**:
- If there are unclarified key decisions, prompt to run `/clarify` first.
- Or accept the user's explicit instruction to skip.

### 2. Develop Creative Plan

Create `stories/*/creative-plan.md`, including the following content:

#### 2.1 Writing Methodology Selection

Based on specification analysis and story genre, select the most suitable writing method:
- **Three-Act Structure**: Suitable for linear narratives with clear setup, confrontation, and resolution.
- **Hero's Journey**: Suitable for growth-oriented, adventure-type stories.
- **Seven-Point Structure**: Suitable for suspense, twist-heavy stories.
- **Story Circle**: Suitable for character-driven stories with psychological depth.
- **Hybrid Method**: Using different methods for the main plot and subplots.
- **Genre-Specific Structures**: Such as "Thrill Point Distribution Structure" for爽文 (shuangwen, "cool" fiction) or "Clue Layout Structure" for mysteries (refer to genre knowledge base).

Record the reason for the choice and how it will be applied.

#### 2.2 Chapter Architecture Design

```markdown
## Chapter Architecture

### Overall Plan
- Total Chapters: [Based on target word count and chapter length]
- Chapter Length: [Based on pacing configuration or default 2000-3000 words/chapter]
- Volume Arrangement: [If applicable]

**🆕 Pacing Parameters (if rhythm-config.json exists)**:
- Average Chapter Word Count: [Read from config, e.g., 3200 words]
- Minor Climax Interval: [Read from config, e.g., 5 chapters]
- Major Climax Interval: [Read from config, e.g., 30 chapters]
- Pacing Style: [Fast/Moderate/Slow]
- Content Ratio: Dialogue [X]% / Action [X]% / Description [X]% / Psychology [X]%

### 🌟 Golden Opening Planning (if including Chapters 1-3)

**Important**: If this plan includes Chapters 1-3, special attention must be paid to the following points (based on golden-opening.md):

#### Chapter 1 Plan
- ✅ **Rule 1 - Dynamic Scene Entry**:
  - Prohibited: Static scenes, long environmental descriptions.
  - Required: Start directly with conflict/action/dialogue.
  - Specific Design: [Describe the opening method for Chapter 1].

- ✅ **Rule 2 - Front-load the Core Conflict**:
  - The protagonist's core conflict must be introduced within the first chapter.
  - Specific Design: [Describe how the core conflict will be presented].

- ✅ **Rule 3 - Avoid Information Dumps**:
  - Absolutely forbid large-scale introductions of the world-building at the beginning.
  - Use a "drip-feed" method for information reveal.
  - Specific Design: [List the information points to be revealed in Chapter 1].

- ✅ **Rule 4 - Limit the Number of Characters Introduced**:
  - No more than 3 named characters.
  - Specific Design: [List the characters appearing in Chapter 1].

#### Chapters 2-3 Plan
- ✅ **Rule 5 - Quickly Showcase the "Golden Finger" (Special Ability)**:
  - Show the effect of the "golden finger" in the second or third chapter.
  - Specific Design: [Describe how the golden finger will be showcased].

#### Opening Pacing Requirements
- Chapter 1 Goal: Hook the reader, build anticipation.
- Chapter 2 Goal: Showcase abilities, strengthen the hook.
- Chapter 3 Goal: Initial thrill point, confirm reader engagement.

### Emotional Curve Design ⭐ (Building an emotional feedback loop for the reading experience)

**Core Concept**: A good novel is not just a journey of story, but a **journey of emotion**. The essence of readers continuing to read is to chase the ups and downs and satisfaction of emotions.

**Emotional Type Definitions** (using novel terminology):

| Emotion Type | Definition | Reader Experience | Typical Scene |
|---|---|---|---|
| 😤 **Thrill Point** | Protagonist wins, a reversal, showcases power | Exhilarating, satisfying, look forward to the next | Face-slapping, comeback, showing off successfully |
| 😭 **Angst Point** | Protagonist fails, is suppressed, suffers setbacks | Worried, frustrated, hope for a comeback | Being bullied, failing, losing someone important |
| 🤔 **Suspense Point** | Unknowns, questions, foreshadowing | Curious, guessing, want to keep reading | A mysterious character appears, a clue is found, a puzzle is left |
| 💧 **Calm Point** | Daily life, transition, setup | Buffer, understanding, emotional preparation | Daily life, character interaction, world-building display |

**Emotional Design Principles**:
1. ✅ **Build Up Before Payoff**: Appropriately set up an angst point before a thrill point to make the thrill stronger.
2. ✅ **Vary the Tempo**: Avoid consecutive angst points or thrill points to maintain rhythm.
3. ✅ **Suspense-Driven**: Leave suspense at the end of each chapter to drive the desire to read on.
4. ✅ **Emotional Progression**: The emotional intensity at the climax should be significantly higher than at the opening.

**Chapter Section Emotional Planning**:

| Chapter Section | Emotion Type | Intensity | Target Effect | Key Scene |
|---|---|---|---|---|
| Ch 1-3 | Angst→Thrill→Suspense | Med→High→Med | Suppress then lift at the start, build desire to read on | [Specific description] |
| Ch 4-8 | Calm→Angst→Thrill | Low→Med→High | First minor climax | [Specific description] |
| Ch 9-15 | Suspense→Angst→Thrill | Med→High→High | Second climax, plant foreshadowing | [Specific description] |
| ... | ... | ... | ... | ... |

**Emotional Intensity Levels**:
- **Low**: Small emotional fluctuation, mainly for setup and transition.
- **Medium**: Clear emotional ups and downs, reader feels engaged.
- **High**: Emotional peak, reader is highly invested.
- **Extreme**: The absolute peak of the book, decisive climax (usually 1-3 instances).

**Emotional Curve Visualization** (optional, ASCII art):
```
Intensity
Extreme|                    ╱╲              ╱╲
High   |         ╱╲        ╱  ╲            ╱  ╲___
Medium |    ╱╲  ╱  ╲      ╱    ╲___    ___╱
Low    | __╱  ╲╱    ╲____╱         ╲__╱
       └─────────────────────────────────────> Chapters
          3   8   15   25   35   45   55
```

**Emotional Design Self-Checklist**:
- [ ] Is there a clear emotional hook in the first 3 chapters?
- [ ] Is there a calm period lasting more than 5 consecutive chapters? (Warning: high risk of reader drop-off)
- [ ] Is there a sufficient thrill point reward after an angst point?
- [ ] Does each volume/stage have a clear emotional climax?
- [ ] Is the highest emotional point of the book in the final 1/3?
- [ ] Is there suspense left at the end of the chapter to drive reading the next one?

**Relationship with Pacing Configuration**:
- If `rhythm-config.json` exists, refer to the "Thrill Point Interval" parameter.
- The emotional rhythm of a reference work can be a guide, but adjust according to your own story.
- Different genres have different emotional rhythms (Shuangwen: high-frequency thrill points; Suspense: high-frequency suspense; Angst-heavy: high thrill payoff late).

### Structure Mapping
[Map key nodes to specific chapters based on the chosen method]

### Plotline Distribution Plan

**Important**: Read the plotline management specifications from Chapter 5 of specification.md and mark the active plotlines in each volume/chapter section.

#### Volume 1: [Volume Name](Chapter Range)

| Chapter Section | Content | Key Events | **Active Plotlines** | **Intersection Point** |
|---|---|---|---|---|
| [X-Y] | [Section content] | [List of key events] | PL-01⭐⭐⭐, PL-02⭐⭐ | X-001 (Chapter X) |
| [X-Y] | [Section content] | [List of key events] | PL-01⭐⭐, PL-03⭐⭐⭐ | None |

**Plotline Annotation Guide**:
- PL-XX: Plotline ID, from specification.md section 5.1
- ⭐⭐⭐ Main Progression: This chapter section focuses on advancing this plotline, taking up the main portion of the text.
- ⭐⭐ Supporting: Normal progression, has some screen time.
- ⭐ Background: Mentioned occasionally to maintain its presence.
- X-XXX: Intersection Point ID, from specification.md section 5.3

#### Volume 2: [Volume Name](Chapter Range)

[Repeat the table structure above]

### Pacing Design
- Opening Hook: Chapter [X]
- First Climax: Chapter [X]
- Midpoint Turn: Chapter [X]
- Greatest Crisis: Chapter [X]
- Final Climax: Chapter [X]
```

#### 2.3 Character System Design

```markdown
## Character System

### Protagonist Design
- Initial State: [Starting point]
- Growth Arc: [Trajectory of change]
- Core Conflict: [Internal vs. External]
- Key Turning Points: [Specific chapters]

### Supporting Character Functions
[Functional role and appearance plan for each important supporting character]

### Relationship Network
[Character relationship map and evolution plan]
```

#### 2.4 World-building Construction

```markdown
## World-building System

### Core Settings
- World Rules: [Physics/Magic/Technology rules]
- Social Structure: [Politics/Economy/Culture]
- Historical Background: [Important historical events]

### Setting Reveal Plan
- First Layer (Opening): [Basic settings]
- Second Layer (Development): [In-depth settings]
- Third Layer (Climax): [Core secrets]
```

#### 2.5 Plot Technique Design

```markdown
## Plot Techniques

### Conflict Escalation Path
1. Primary Conflict: [Individual level]
2. Intermediate Conflict: [Group level]
3. Advanced Conflict: [World level]

### Suspense Setup
- Main Suspense: [Spans the entire story]
- Chapter Suspense: [Hooks for each chapter]
- Subplot Suspense: [Adds layers]

### Foreshadowing Layout
[List of foreshadowing and resolution plan]
```

#### 2.6 Narrative Technique Selection

```markdown
## Narrative Techniques

### POV Design
- Perspective Type: [First/Third person]
- Perspective Limitation: [Omniscient/Limited]
- Multi-POV Arrangement: [If applicable]

### Timeline Design
- Main Timeline: [Linear/Non-linear]
- Use of Flashbacks: [Strategy for use]
- Parallel Narratives: [If applicable]

### Narrative Pacing
- Fast-paced Sections: [Action/Conflict]
- Slow-paced Sections: [Emotion/Description]
- Pacing Variation: [Pattern of tension and release]
```

### 3. Technical Decision Log

Record all important technical decisions:
- **Decision**: What was chosen
- **Reason**: Why it was chosen
- **Risks**: Potential problems
- **Contingency**: Alternative plans

### 4. Quality Assurance Plan

```markdown
## Quality Assurance

### Self-Checklist
- [ ] Logical consistency checkpoints
- [ ] Plausibility of character actions
- [ ] Internal consistency of world-building
- [ ] Pacing fluency

### Verification Nodes
- Every 5 chapters: Minor loop verification
- Every volume: Major loop verification
- Final draft: Comprehensive verification
```

### 5. Risk Management

Identify and develop strategies for:
- **Creative Risks**: Inspiration, logic, pacing
- **Technical Risks**: Complexity, consistency
- **Time Risks**: Schedule, quality balance

### 6. Output and Validation

- Save the plan to `stories/*/creative-plan.md`
- Verify that the plan adheres to the constitution principles
- Verify that the plan meets the specification requirements
- Suggest the next step: run `/tasks` to generate tasks

## Relationship with Other Commands

- **Input**: Specifications from `/specify` + clarifications from `/clarify`
- **Output**: Provides the basis for task generation for `/tasks`
- **Validation**: Used by `/analyze` to check implementation compliance

## Notes

### 🌟 Application of the Golden Opening Rules (Important)

**When to Apply**:
- Automatically triggered when the plan includes Chapters 1-3.
- Or for short stories with a total word count < 10,000.

**Why It's Important**:
- The first three chapters determine 80% of reader retention.
- The opening is the key window where readers decide whether to continue reading.
- The golden opening rules have been validated by numerous blockbuster works.

**How to Apply**:
1. Create a separate "Golden Opening Planning" section within "Chapter Architecture Design".
2. Check each of the five rules one by one to see if they are reflected in the first three chapters.
3. Specifically design how each chapter will satisfy the rules.
4. If the specifications conflict with the rules, prioritize the rules (or consciously violate them).

**Common Pitfalls**:
- ❌ Long descriptions of world-building settings in Chapter 1.
- ❌ The protagonist is just living their daily life in Chapter 1 with no conflict.
- ❌ Too many characters introduced in Chapter 1 (>3).
- ❌ The "golden finger"/core ability is delayed until after Chapter 5.

### 🎵 Application of Pacing Configuration

**If `/book-internalize` was used**:
- The system will automatically read `spec/presets/rhythm-config.json`.
- Apply the pacing parameters of the reference work (chapter word count, thrill point interval, etc.).
- Apply content ratios (dialogue/action/description/psychology).

**Parameter Priority**:
1. **User's immediate instructions** (Highest)
2. **rhythm-config.json** (Reference work's pacing)
3. **Genre knowledge base** (Genre-common pacing)
4. **Default values** (2000-3000 words/chapter)

**Suggestions**:
- The pacing parameters of a reference work are for reference only.
- Adjust moderately based on your own writing habits.
- Do not copy mechanically; maintain flexibility.

### Technique Serves the Story
- All technical choices must serve the story's expression.
- Don't use techniques for the sake of using techniques.
- Maintain flexibility in the plan.

### Executability
- The plan must be specific and executable.
- Avoid being overly idealistic.
- Consider your actual creative capacity.

### Iterative Optimization
- The plan can be adjusted based on practice.
- Record the reasons for and impacts of adjustments.
- Maintain version tracking.

Remember: **A good plan is half the battle, but be ready to adjust. The golden opening is a hard rule; other plans can be flexible.**
