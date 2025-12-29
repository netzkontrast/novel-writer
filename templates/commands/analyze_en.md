---
description: Intelligent Analysis: Automatically selects framework analysis (before write) or content analysis (after write), supports manual specification with --type
argument-hint: [--type=framework|content]
allowed-tools: Bash(find:*), Bash(wc:*), Bash(grep:*), Read(//**), Read(//plugins/**), Read(plugins/**), Write(//stories/**/analysis-report.md), Bash(*)
model: claude-sonnet-4-5-20250929
scripts:
  sh: .specify/scripts/bash/check-analyze-stage.sh --json
  ps: .specify/scripts/powershell/check-analyze-stage.ps1 -Json
---

Performs intelligent, comprehensive analysis of the novel project. Automatically selects to execute **framework consistency analysis** (before writing) or **content quality analysis** (after writing) based on the current creation stage.
---

## Core Philosophy

**One Command, Dual Intelligence**:
- 📐 **Framework Analysis**: Verifies the consistency of specifications, plans, and tasks before writing (similar to spec-kit).
- 📝 **Content Analysis**: Verifies the quality and compliance of completed content after writing.

**Restrained but not Simplistic**:
- The user only needs to execute `/analyze`, and the system automatically determines which analysis to perform.
- Supports manual mode specification: `$ARGUMENTS --type=framework` or `--type=content`.

## Execution Flow

### 1. Intelligent Stage Detection

Run `{SCRIPT}` to get the current creation status:

```json
{
  "analyze_type": "framework|content",
  "chapter_count": 0,
  "has_spec": true,
  "has_plan": true,
  "has_tasks": true,
  "story_dir": "/path/to/story",
  "reason": "Reason description"
}
```

### 2. Decision Logic

Parse user arguments `$ARGUMENTS`:

**Manual Specification Mode** (highest priority):
- Contains `--type=framework` → Force framework analysis.
- Contains `--type=content` → Force content analysis.

**🆕 Specialized Analysis Mode** (new):
- Contains `--focus=opening` → Specialized opening analysis (focuses on the first 3 chapters).
- Contains `--focus=pacing` → Specialized pacing analysis (focuses on the distribution of high points/conflicts).
- Contains `--focus=character` → Specialized character analysis (focuses on character arcs).
- Contains `--focus=foreshadow` → Specialized foreshadowing analysis (focuses on planting and resolving foreshadowing).
- Contains `--focus=logic` → Specialized logic analysis (focuses on finding logical loopholes).
- Contains `--focus=style` → Specialized style analysis (focuses on checking writing style consistency).

**Automatic Judgment Mode**:
- Chapter count = 0 → **Framework Analysis**
- Chapter count < 3 → **Framework Analysis** (but suggests that writing can continue).
- Chapter count ≥ 3 → **Content Analysis**

### 3. Execute Corresponding Analysis

Based on the decision, execute one of the following two analyses.

---

## Mode A: Framework Consistency Analysis

**Objective**: To verify that preparations are sufficient before writing, ensuring there are no contradictions between specifications, plans, and tasks.

### A1. Load Benchmark Documents

- Constitution file: `.specify/memory/constitution.md`
- Specification file: `stories/*/specification.md`
- Plan file: `stories/*/creative-plan.md`
- Task file: `stories/*/tasks.md`

### A2. Coverage Analysis

Check if all specification requirements have corresponding plans and tasks:

```markdown
## Coverage Analysis Report

### P0 Requirement Coverage
- [Requirement 1: Protagonist's growth arc] → ✅ Plan Chapter 3, Tasks #5-8
- [Requirement 2: Villain's setting] → ⚠️ Mentioned in the plan, but no specific tasks
- [Requirement 3: Suspense setting] → ❌ Not covered in plan or tasks

### P1 Requirement Coverage
Coverage: 75% (3/4)

### P2 Requirement Coverage
Coverage: 50% (2/4)

### Task Completeness
- Do all planned chapters have corresponding tasks: ⚠️ Chapters 10-12 are missing tasks
- Do tasks cover all key scenes: ✅ Yes
```

### A3. Consistency Check

Verify if there are contradictions between documents:

```markdown
## Consistency Check Report

### Specification ↔ Plan
- ✅ Consistent theme expression
- ⚠️ Specification requires "fast pace," but the first 5 chapters in the plan have a slow pace
- ❌ Specification prohibits "too much romance," but chapters 6-8 in the plan have extensive romantic plotlines

### Plan ↔ Tasks
- ✅ All planned chapters have tasks
- ⚠️ Estimated total word count for tasks is 150K, but the plan's target is 100K
- ❌ Plan requires Chapter 5 to be a climax, but the task is marked as a "transitional chapter"

### Constitution Compliance
- ✅ The plan aligns with the values of the creative constitution
- ✅ Task breakdown meets quality standards
```

### A4. Logical Issue Warning

Analyze potential logical loopholes in the storyline design:

```markdown
## Logical Issue Warning

### Timeline Conflict
- ⚠️ Chapter 3 is "three years later," but a character in Chapter 5 mentions "an event from two years ago," the timeline doesn't match

### Character Ability Contradiction
- ❌ In Chapter 2, the protagonist "doesn't know martial arts," but the task description for Chapter 4 is "defeat the enemy using swordsmanship"

### Unplanned Foreshadowing
- ⚠️ A "mysterious token" is foreshadowed in Chapter 1, but there is no plan to resolve it in subsequent chapters
```

### A5. Readiness Assessment

Assess whether writing can begin:

```markdown
## Readiness Assessment

### Necessary Conditions (P0)
- [x] Specifications are complete and clear
- [x] The plan covers all P0 requirements
- [ ] Task breakdown is incomplete (missing tasks for 3 chapters)
- [ ] No fatal logical contradictions (2 found)

### Recommended Conditions (P1)
- [x] Character profiles are complete
- [ ] World-building document is not detailed enough
- [x] Timeline planning is clear

### Overall Score: 6/10

**Recommendations**:
1. 🔴 Must fix: Add tasks for chapters 10-12
2. 🔴 Must fix: Resolve timeline and character ability contradictions
3. 🟡 Recommended optimization: Supplement the world-building document
4. 🟢 Optional: Adjust the pacing design of the first 5 chapters

**Conclusion**: It is currently **not recommended to start writing**. Please resolve the P0 issues first.
```

---

## Mode B: Content Quality Analysis

**Objective**: To conduct a comprehensive quality verification of the completed content, ensuring it meets specifications and providing improvement suggestions.

### B1. Load Verification Benchmarks

- Constitution file: `.specify/memory/constitution.md`
- Specification file: `stories/*/specification.md`
- Plan file: `stories/*/creative-plan.md`
- Task list: `stories/*/tasks.md`
- **Completed Content**: `stories/*/content/*.md` or `stories/*/chapters/*.md`

### B2. Constitution Compliance Check

Verify if the work adheres to the principles of the creative constitution:

```markdown
## Constitution Compliance Report

### Core Value Check
- [x] Value Principle 1: Positive theme ✅
- [x] Value Principle 2: Avoid vulgar content ✅
- [ ] Value Principle 3: Respect cultural traditions ⚠️ Controversial description in Chapter 7

### Quality Standard Verification
- Logical consistency: 8/10 ⚠️ Minor contradiction between Chapter 3 and Chapter 6
- Character depth: 7/10 (Protagonist is well-developed, supporting characters are slightly flat)
- Writing quality: 8/10 (Good fluency, some descriptions could be enhanced)

### Style Consistency
- Narrative style: Consistent ✅
- Language style: Consistent ✅
- Pacing control: Slow at the beginning, then faster; overall reasonable ✅

**Overall Score: 8/10**
```

### B3. Specification Compliance Analysis

Check if the implementation meets the specification requirements:

```markdown
## Specification Compliance Analysis

### Core Requirement Coverage
#### P0 (Must include)
- [Requirement 1: Father-son conflict] → ✅ Fully demonstrated in Chapters 2-4
- [Requirement 2: Suspense setting] → ⚠️ Insufficient suspense in Chapter 5
- [Requirement 3: Three-dimensional villain] → ❌ Villain has not yet officially appeared

Coverage: 67% (2/3)

#### P1 (Should include)
Coverage: 75% (3/4)

#### P2 (Could include)
Coverage: 50% (2/4)

### Goal Achievement
- Target audience adaptation: 85% (Pacing and plot are in line with target audience preferences)
- Market positioning compliance: 80% (Differentiated selling points are clear but need to be strengthened)
- Success criteria achievement: 5/8 ⚠️ Some indicators not met

### Constraint Compliance
- Content red lines: ✅ No violations
- Creative constraints: ✅ Word count, update frequency meet requirements
- Technical constraints: ✅ Platform formatting standards

**Overall Score: 7/10**
```

### B4. Plan Execution Analysis

Evaluate the deviation between actual execution and the plan:

```markdown
## Plan Execution Analysis

### Chapter Structure Comparison
| Plan | Actual | Deviation Analysis |
|---|---|---|
| Chapter 1: Opening hook | ✅ Completed | Meets expectations, strong opening appeal |
| Chapter 2: Conflict development | ✅ Completed | Slightly adjusted, added foreshadowing |
| Chapter 3: Turning point | ⚠️ Completed | Turning point moved to the end of Chapter 2 |
| Chapter 4: Deepening conflict | ✅ Completed | Fully consistent with the plan |
| Chapter 5: Prelude to climax | ❌ Postponed | Became a transitional chapter in practice |

### Character Development Trajectory
- Protagonist's growth arc: 85% compliance (Growth rate is slightly faster than planned)
- Supporting character function: 70% realization (The role of supporting character B is not fully realized)
- Relationship evolution: 90% compliance (Father-son relationship evolution meets expectations)

### World-building Expansion
- First layer of setting (basic rules): ✅ Expanded as planned
- Second layer of setting (power structure): ⚠️ Revealed early (Planned for Chapter 8, actually in Chapter 5)
- Third layer of setting (ultimate secret): To be expanded

**Compliance Score: 8/10**
```

### B5. Content Quality Analysis

In-depth analysis of the work's quality:

```markdown
## Content Quality Analysis

### Text Statistics
- Total word count: 45,230 words
- Average chapter length: 6,461 words
- Completion progress: 35% (7/20 chapters)

### Structural Analysis
- Plot density: Medium (2-3 plot points per chapter)
- Conflict frequency: Moderate (Average of 1.5 conflicts per chapter)
- Pacing variation: Slow in the first 3 chapters, accelerating in chapters 4-7, as expected

### Technical Issues
#### Logical Issues
1. Chapter 3: A character mentions "an event from three years ago," but the timeline shows only two years have passed
2. Chapter 6: The protagonist uses an ability that was explicitly stated as "unknown" in Chapter 2

#### Cohesion Issues
1. The suspense at the end of Chapter 4 is not addressed at the beginning of Chapter 5

#### Character Consistency
1. The protagonist reacts inconsistently to similar events in Chapter 2 and Chapter 5 (impulsive in Chapter 2, calm in Chapter 5)

### Highlight Identification
1. Chapter 1: The opening hook is cleverly designed and introduced naturally
2. Chapter 4: The father-son dialogue is rich and emotionally sincere
3. Chapter 6: The action scenes are described fluently with a strong sense of imagery

**Quality Score: 7.5/10**
```

### 🆕 B5.1 Specialized Analysis (Optional)

**If the user specifies the `--focus` parameter, perform the corresponding in-depth specialized analysis**:

---

#### Specialization 1: Opening Analysis (--focus=opening)

**Objective**: To deeply analyze whether the first 1-3 chapters follow the golden opening rules.

**Analysis Dimensions**:

```markdown
## Specialized Opening Analysis Report

### Golden Rule Check

**If `spec/presets/golden-opening.md` exists, automatically read and apply the five golden rules.**

#### Rule 1: Dynamic Scene Entry
- ✅ Chapter 1 opening method: [Action/Dialogue/Conflict] direct entry
- ❌ Issue found: The opening has 200 words of static environmental description (violates the rule)
- Suggestion: Delete or shorten to under 50 words and go directly into the action

#### Rule 2: Front-load the Core Conflict
- ✅ Timing of core conflict introduction: Chapter 1, Section [X]
- ⚠️ Conflict intensity: Medium (suggest increasing to a level that "threatens the protagonist's survival/goals")
- Specifics: [Describe the conflict content]

#### Rule 3: Avoid Information Dumps
- ✅ Method of revealing world-building: Drip-feed, naturally integrated into the plot
- ❌ Issue found: Chapter 1, Section 3 has a 500-word setting explanation (violates the rule)
- Suggestion: Split it across the first 5 chapters, revealing 100 words per chapter

#### Rule 4: Limit the Number of Characters Introduced
- ✅ Named characters in Chapter 1: [X] people (meets the ≤3 requirement)
- ❌ Issue found: 5 characters were introduced in Chapter 1, which is too many (violates the rule)
- Suggestion: Delay the introduction of [Character D] and [Character E] to Chapters 2-3

#### Rule 5: Quickly Showcase the "Golden Finger" (Special Ability)
- ✅ Timing of "golden finger" reveal: Chapter [X]
- ⚠️ Method of reveal: Only mentioned, not actually used (suggest demonstrating its effect)
- Specifics: [Describe the method of reveal]

### Opening Hook Assessment
- **First sentence hook strength**: [Strong/Medium/Weak]
  - Current: [Quote the first sentence]
  - Analysis: [Does it attract the reader?]
  - Suggestion: [Direction for optimization]

- **Chapter 1 ending hook**: [Strong/Medium/Weak]
  - Current: [Quote the ending paragraph]
  - Analysis: [Does it create anticipation?]
  - Suggestion: [Direction for optimization]

### First Three Chapters Pacing Check
| Chapter | Objective | Actual Completion | Score |
|---|---|---|---|
| Chapter 1 | Hook the reader, build anticipation | [Describe actual effect] | [X]/10 |
| Chapter 2 | Showcase abilities, strengthen the hook | [Describe actual effect] | [X]/10 |
| Chapter 3 | Initial high point, confirm reader engagement | [Describe actual effect] | [X]/10 |

**Opening Score: [X]/10**
**Suggestion**: [Specific improvement directions]
```

---

#### Specialization 2: Pacing Analysis (--focus=pacing)

**Objective**: To analyze the pacing distribution throughout the text and evaluate the density of high points/conflicts.

**Analysis Dimensions**:

```markdown
## Specialized Pacing Analysis Report

### Pacing Parameters (if rhythm-config.json exists)
**Read `spec/presets/rhythm-config.json` (if it exists)**:
- Target chapter word count: [X] words
- Target interval for minor climaxes: [X] chapters
- Target interval for major climaxes: [X] chapters
- Target pacing style: [Fast/Moderate/Slow]

### Conflict Distribution Statistics
| Chapter | Conflict Count | Conflict Type | Conflict Intensity | Meets Expectations? |
|---|---|---|---|---|
| Chapter 1 | 2 | Interpersonal/Internal | Medium/High | ✅ |
| Chapter 2 | 1 | Interpersonal | Low | ⚠️ Too few |
| Chapter 3 | 3 | Interpersonal/External | High/High/Medium | ✅ |
| ... | ... | ... | ... | ... |

**Average Conflict Density**: [X] per chapter
**Suggested Density**: [Y] per chapter (based on genre and pacing configuration)

### High Point Distribution Statistics
| Chapter | High Point Type | High Point Intensity | Interval (chapters) |
|---|---|---|---|
| Chapter 1 | - | - | - |
| Chapter 3 | Face-slapping | High | 3 chapters |
| Chapter 7 | Level-up | Medium | 4 chapters |
| ... | ... | ... | ... |

**Average High Point Interval**: [X] chapters
**Suggested Interval**: [Y] chapters (based on rhythm-config or genre standards)

### Climax Distribution
- **Minor Climaxes**: Chapters [X], [Y], [Z]
  - Interval reasonableness: ✅ Meets the standard of one every 5 chapters
- **Major Climax**: Chapter [X]
  - Positional reasonableness: ⚠️ Suggested for Chapter 30, but actually in Chapter 25 (early)


**Pacing Evaluation**:
- ✅ Overall flow is reasonable
- ⚠️ Chapters 10-15 are slightly flat
- ❌ There is a pacing break in Chapter 20

**Improvement Suggestions**:
1. Add a medium-intensity conflict in Chapter 12
2. Add transitional content in Chapter 20 to avoid a sense of disconnection

**Pacing Score: [X]/10**
```

---

#### Specialization 3: Character Analysis (--focus=character)

**Objective**: To evaluate character arcs, consistency, and growth trajectories.

```markdown
## Specialized Character Analysis Report

### Protagonist Arc Tracking
**Read the planned character arc from specification.md and creative-plan.md**

| Node | Planned State | Actual State | Compliance |
|---|---|---|---|
| Start | [State A] | [Actual A] | ✅/⚠️/❌ |
| Trigger | [State B] | [Actual B] | ✅/⚠️/❌ |
| Growth | [State C] | [Actual C] | ✅/⚠️/❌ |
| Transformation | [State D] | [To be expanded] | - |

**Growth Reasonableness Assessment**:
- ✅ Growth is triggered by events
- ⚠️ Growth is slightly too fast (too much change between Chapter 3 and Chapter 7)
- ✅ Growth is consistent with the character's personality

### Protagonist Consistency Check
- **Personality Consistency**:
  - ✅ Chapters 1-5: Impulsive personality remains consistent
  - ❌ Chapter 6: Suddenly becomes calm in a similar situation (contradiction)

- **Ability Consistency**:
  - ✅ Martial ability progresses gradually, consistent with the setting
  - ❌ Uses an unlearned skill in Chapter 7

- **Motivation Consistency**:
  - ✅ Core goal is clear and consistent throughout

### Supporting Character Function Assessment
| Supporting Character | Planned Function | Actual Function | Realization |
|---|---|---|---|
| Supporting Character A | Mentor | Mentor | 90% ✅ |
| Supporting Character B | Rival | Not fully realized | 40% ⚠️ |
| Supporting Character C | Foil | Foil | 85% ✅ |

**Suggestions**:
- Add more confrontational scenes for Supporting Character B (Chapters 8-10)
- Clarify Supporting Character B's motivations and stance

### Relationship Network Evolution
```
Chapter 1: Protagonist ←Hostile← Villain A
              ↓
            Master-Apprentice
              ↓
            Supporting Character A

Chapter 7: Protagonist ←Complex Relationship← Villain A
              ↓          ↑
            Master-Apprentice   Misunderstanding
              ↓          ↓
            Supporting Character A → Supporting Character B
```

**Relationship Evolution Reasonableness**: ✅ Meets expectations

**Character Score: [X]/10**
```

---

#### Specialization 4: Foreshadowing Analysis (--focus=foreshadow)

**Objective**: To check the completeness of foreshadowing setup and resolution.

```markdown
## Specialized Foreshadowing Analysis Report

### Read the foreshadowing management table from specification.md section 5.4

### Foreshadowing Setup Check
| Foreshadowing ID | Planned Setup Chapter | Actual Setup Chapter | Setup Quality |
|---|---|---|---|
| F-001 | Chapter 1 | Chapter 1 | ✅ Natural, not abrupt |
| F-002 | Chapter 3 | Chapter 5 | ⚠️ Delayed by 2 chapters, need to confirm subsequent impact |
| F-003 | Chapter 5 | Not set up | ❌ Missing |

### Foreshadowing Resolution Check
| Foreshadowing ID | Planned Resolution Chapter | Actual Resolution Chapter | Resolution Completeness |
|---|---|---|---|
| F-001 | Chapter 10 | To be completed | - |
| F-002 | Chapter 15 | To be completed | - |

### Unplanned Foreshadowing
**New foreshadowing added during the actual writing process (not in the specification)**:
1. Chapter 2: Hint of a mysterious character → ⚠️ Need to add a resolution plan in the specification
2. Chapter 6: Mention of an ancient prophecy → ⚠️ Need to decide whether to resolve it

### Foreshadowing Density Assessment
- One foreshadowing element is set up every [X] chapters on average
- Suggested density: One every [Y] chapters (based on genre standards)
- Evaluation: ✅ Compliant / ⚠️ Too many / ❌ Too few

### Risk Alerts
- 🔴 Foreshadowing F-003 was not set up, which may affect the plot of Chapter 15
- 🟡 2 new foreshadowing elements have been added, resolution plans need to be supplemented

**Foreshadowing Management Score: [X]/10**
```

---

#### Specialization 5: Logic Analysis (--focus=logic)

**Objective**: To deeply search for logical loopholes and contradictions.

```markdown
## Specialized Logic Analysis Report

### Timeline Check
**Construct a complete timeline**:
```
Absolute Time      Story Time      Chapter     Key Event
2020-01-01         Day 0           -           [Background]
2020-01-05         Day 4           Chapter 1   Protagonist leaves home
2020-01-10         Day 9           Chapter 3   Meets mentor
2023-01-10         Three years later Chapter 5   ⚠️ Contradicts Chapter 7
2022-01-10         Two years later   Chapter 7   Character recalls "an event from three years ago"
```

**Timeline Contradictions**:
- ❌ The timelines in Chapter 5 and Chapter 7 do not match (1 found)
- Suggestion: Standardize to "two years later"

### Cause-and-Effect Logic Check
| Event A (Cause) | Event B (Effect) | Logical Reasonableness |
|---|---|---|
| Protagonist practices in Chapter 2 | Strength increases in Chapter 4 | ✅ Reasonable |
| Treasure is lost in Chapter 3 | Treasure appears in Chapter 6 | ❌ Fails to explain how it was recovered |
| Makes a vow in Chapter 5 | Breaks the vow in Chapter 7 | ⚠️ Lacks psychological buildup |

### Ability Consistency Check
| Chapter | Ability Setting | Contradiction? |
|---|---|---|
| Chapter 2 | Protagonist doesn't know martial arts | - |
| Chapter 4 | Protagonist learns basic swordsmanship | ✅ Reasonable transition |
| Chapter 6 | Protagonist uses advanced swordsmanship | ❌ Leap is too large, lacks learning process |

### Worldview Consistency
- ✅ Magic rules are consistent
- ❌ "Technology is forbidden" is mentioned in Chapter 3, but high-tech weapons appear in Chapter 8
- ⚠️ Slight discrepancies in the social class settings between Chapter 5 and Chapter 9

### Motivation Reasonableness
| Character | Action | Motivation Explanation | Reasonableness |
|---|---|---|---|
| Protagonist | Risks life to save someone in Chapter 5 | Sense of justice | ✅ Consistent with character |
| Supporting Character A | Betrays in Chapter 7 | Unexplained | ❌ Abrupt, lacks foreshadowing |
| Villain | Spares the protagonist in Chapter 9 | Admires their talent | ⚠️ A bit far-fetched |

**Logical Rigor Score: [X]/10**
```

---

#### Specialization 6: Style Analysis (--focus=style)

**Objective**: To check writing style consistency and compare with style-reference.md.

```markdown
## Specialized Style Analysis Report

### If style-reference.md exists (from /book-internalize)

**Read `memory/style-reference.md` and compare with the actual writing style.**

### Vocabulary Consistency Check
**Reference Style Vocabulary Preferences**:
- Target common modifiers: [List]
- Actual common modifiers: [List]
- Match rate: [X]%

**Banned Word Check (AI-like phrasing)**:
- ❌ Found the use of "filled with" [X] times (banned in style-reference)
- ❌ Found the use of "on the verge of collapse" [X] times (banned in style-reference)
- Suggestion: Replace with common words from the target works

### Sentence Structure Consistency Check
- Average sentence length: Actual [X] words vs. Target [Y] words
- Paragraph density: Actual [X] words/paragraph vs. Target [Y] words/paragraph
- Evaluation: ✅ Compliant / ⚠️ Significant deviation

### Description Ratio Check
| Type | Target Ratio | Actual Ratio | Deviation |
|---|---|---|---|
| Dialogue | 35% | 40% | +5% ⚠️ |
| Action | 40% | 30% | -10% ❌ |
| Description | 15% | 20% | +5% ⚠️ |
| Psychology | 10% | 10% | 0% ✅ |

**Suggestion**: Increase the proportion of action descriptions, reduce dialogue and description.

### Narrative Style Consistency
- Perspective: ✅ Third-person limited, remains consistent
- Language: ✅ Colloquial style, meets the target
- Pacing: ⚠️ First 3 chapters are "fast-paced," chapters 4-7 are slower
- Emotional tone: ✅ Exciting tone is consistent throughout

### Inter-Chapter Style Comparison
| Chapter | Style Characteristics | Similarity to Reference Works |
|---|---|---|
| Chapter 1 | Concise and powerful, verb-intensive | 85% ✅ |
| Chapter 2 | Slightly wordy, too many modifiers | 60% ⚠️ |
| Chapter 3 | Returns to a concise style | 80% ✅ |

**Style Consistency Score: [X]/10**
**Suggestion**: Revise Chapter 2 with reference to the style of Chapters 1 and 3.
```

---

### B6. Task Completion Audit

Check the task execution status:

```markdown
## Task Completion

### Overall Progress
- Total tasks: 28
- Completed: 12 (43%)
- In progress: 2 (7%)
- Not started: 14 (50%)

### Key Milestones
- [Milestone 1: First 5 chapters completed] → ✅ Achieved
- [Milestone 2: Main plot progressed to 50%] → ⚠️ Delayed (Planned for Chapter 10, but only 30% by Chapter 7)
- [Milestone 3: End of Volume 1] → TBD

### Blockers and Risks
1. The "climax scene" task for Chapter 5 was not executed as planned, affecting subsequent pacing.
2. The villain character has not yet appeared, which may affect the design of mid-term conflicts.
```

### B7. Generate Improvement Suggestions

Provide specific suggestions based on the analysis results:

```markdown
## Improvement Suggestions

### Urgent Fixes (P0)
1. **Timeline Contradiction**
   - Impact: Undermines reader trust, affects logical rigor.
   - Suggestion: Unify the time expressions in Chapter 3 and Chapter 6 to "two years ago."
   - Location: Chapter 3, Section 2; Chapter 6, Section 4.

2. **Character Ability Contradiction**
   - Impact: Seriously affects character credibility.
   - Suggestion: Add a transitional plot of "learning martial arts" between Chapters 4-5, or remove the martial arts description in Chapter 6.
   - Location: Chapter 2, Section 5; Chapter 6, Section 3.

### Optimization Suggestions (P1)
1. **Insufficient Suspense in Chapter 5**
   - Current: The ending of Chapter 5 is flat and lacks a hook.
   - Suggestion: Add an unexpected event or piece of information at the end to create anticipation.
   - Expected effect: Improve reader retention.

2. **Function of Supporting Character B Not Realized**
   - Current: Supporting Character B appears but their role is unclear.
   - Suggestion: Arrange a key role for Supporting Character B in Chapters 8-9 to follow up on earlier foreshadowing.
   - Expected effect: Enhance the presence of the supporting character and enrich the story.

### Long-term Improvements (P2)
1. **Early Reveal of World-building Settings**
   - Reason: May affect the creation of mystery later on.
   - Solution: Evaluate whether to adjust the subsequent reveal pace or add deeper settings.
   - Timing: Decide before Chapter 10.

**Priority Order**: P0-1 (Timeline) → P0-2 (Ability Contradiction) → P1-1 (Suspense) → P1-2 (Supporting Character)
```

### B8. Generate Verification Report

Create `stories/*/analysis-report.md`:

```markdown
# Work Analysis Report

## Summary
- Analysis Date: 2025-10-01
- Analysis Scope: Chapters 1-7
- Word Count Analyzed: 45,230 words
- Overall Score: 7.5/10
- Recommended Action: Continue writing, revise the first 7 chapters in bulk.

## Core Metrics
| Dimension | Score | Description |
|---|---|---|
| Constitution Compliance | 8/10 | Values are correct, style is consistent, 1 point to note |
| Specification Compliance | 7/10 | P0 requirement coverage is 67%, villain scenes need to be added |
| Plan Execution | 8/10 | Generally compliant, local adjustments are reasonable |
| Content Quality | 7.5/10 | 2 logical issues, 1 character contradiction to be fixed |
| Reader Experience | 8/10 | Pacing is reasonable, highlights are prominent, highly readable |

**Average Score: 7.7/10**

## Key Findings
1. ✅ Strong opening appeal, excellent hook design in Chapter 1.
2. ✅ Father-son relationship evolution meets expectations, emotionally rich.
3. ⚠️ 2 logical contradictions exist and need to be fixed.
4. ⚠️ Insufficient suspense in Chapter 5, affecting reader retention.
5. ❌ P0 requirement "Three-dimensional villain" has not yet been realized.

## Next Steps
1. 🔴 **Immediate Fix**: Timeline contradiction, character ability contradiction (Est. 2 hours).
2. 🟡 **Near-term Optimization**: Add suspense to Chapter 5, arrange scenes for Supporting Character B in Chapters 8-9 (Est. 1 day).
3. 🟢 **Continue Writing**: Proceed with Chapters 8-10 as planned, focusing on adding villain scenes.
4. 📅 **Planned Check**: Run `/analyze` again after completing Chapter 10 for a phase-gate review.
```

---

## Analysis Dimension Framework

### Seven Major Analysis Dimensions
1. **Compliance**: Conforms to constitution and specifications.
2. **Consistency**: Logic, characters, world-building.
3. **Completeness**: Requirement coverage, task completion.
4. **Quality**: Writing, structure, pacing.
5. **Innovation**: Highlights, features, breakthroughs.
6. **Readability**: Fluency, appeal, resonance.
7. **Feasibility**: Progress, resources, risks.

## Scoring Standard

```text
10 points: Perfect, exceeds expectations
8-9 points: Excellent, minor flaws
6-7 points: Good, needs optimization
4-5 points: Passable, many issues
1-3 points: Poor, needs rewriting
```

## Usage Scenarios

### Scenario 1: Ready to Start Writing

```bash
User: /tasks
AI: 28 writing tasks have been generated.

User: /analyze
AI: No chapter content detected, performing framework consistency analysis...

[Coverage Analysis]
⚠️ Specification requirement "Villain's setting" has no corresponding task.
❌ Chapters 10-12 are missing task breakdowns.

[Consistency Check]
❌ Specification requires "fast pace," but the first 5 chapters in the plan are slow.

[Readiness Score] 6/10
Suggestion: Fix the above issues before starting to write.
```

### Scenario 2: Completed Some Chapters

```bash
User: /write (Completed Chapters 1-7)

User: /analyze
AI: 7 chapters detected, performing content quality analysis...

[Constitution Compliance] 8/10 ✅
[Specification Compliance] 7/10 ⚠️ P0 requirement coverage 67%
[Content Quality] 7.5/10 ⚠️ 2 logical issues found

[Improvement Suggestions]
P0: Fix timeline and character ability contradictions.
P1: Add suspense to Chapter 5.

[Recommendation]: After fixing P0 issues, continue writing Chapters 8-10.
```

### Scenario 3: Manual Specification Mode

```bash
User: /analyze --type=framework
AI: Forcing framework analysis mode...

(Performs framework analysis even if there is chapter content)

User: /analyze --type=content
AI: Forcing content analysis mode...

(Performs content analysis even if there are fewer than 3 chapters)
```

### 🆕 Scenario 4: Specialized Analysis

```bash
# Specialized opening analysis (check if the first 3 chapters follow golden opening rules)
User: /analyze --focus=opening
AI: Performing specialized opening analysis...

[Golden Rule Check] (automatically reads golden-opening.md)
❌ Rule 1 violated: Chapter 1 has 200 words of static environmental description.
⚠️ Rule 5 delayed: The "golden finger" is only revealed in Chapter 4 (recommended for Chapters 2-3).

[Opening Hook Assessment]
First sentence hook: Medium strength
Chapter 1 ending hook: Strong

[Suggestion]: Remove the opening environmental description, move the "golden finger" reveal to Chapter 3.

---

# Specialized pacing analysis (check high point distribution and conflict density)
User: /analyze --focus=pacing
AI: Performing specialized pacing analysis...

[Pacing Parameters] (automatically reads rhythm-config.json)
Target high point interval: 5 chapters
Actual average interval: 7 chapters ⚠️ Too long

[Conflict Distribution]
Average conflict density: 1.2 per chapter
Suggested density: 2 per chapter ❌ Insufficient

[Suggestion]: Add high point designs in Chapters 5, 10, and 15.

---

# Specialized logic analysis (deep search for logical loopholes)
User: /analyze --focus=logic
AI: Performing specialized logic analysis...

[Timeline Contradiction]
❌ "Three years later" in Chapter 5 vs. "two years later" in Chapter 7

[Ability Contradiction]
❌ "Doesn't know martial arts" in Chapter 2 → "Uses advanced swordsmanship" in Chapter 6 (leap is too large)

[Cause-and-Effect Logic]
❌ Treasure lost in Chapter 3 → Treasure appears in Chapter 6 (fails to explain how it was recovered)

[Suggestion]: Fix the above 3 logical contradictions.

---

# Specialized style analysis (compare with style-reference.md)
User: /analyze --focus=style
AI: Performing specialized style analysis...

[Vocabulary Consistency] (automatically reads style-reference.md)
❌ Found banned word "filled with" 12 times
⚠️ Actual average sentence length is 35 words vs. target of 25 (too long)

[Description Ratio]
Dialogue: 40% (Target 35%) +5% ⚠️
Action: 30% (Target 40%) -10% ❌

[Suggestion]:
1. Replace banned words with common words from the target works.
2. Shorten sentences, increase the proportion of short sentences.
3. Increase action descriptions, reduce dialogue.
```

## Relationship with Other Commands

```text
/constitution → Defines creative principles
     ↓
/specify → Defines story specifications
     ↓
/clarify → Clarifies key decisions
     ↓
/plan → Creates a creative plan
     ↓
/tasks → Breaks down execution tasks
     ↓
/analyze (framework mode) → Verifies readiness
     ↓
/write → Executes chapter writing
     ↓
/analyze (content mode) → Verifies quality and consistency
     ↓
(Loop: Revise → Continue writing → Analyze again)
```

## Notes

### Intelligent but Controllable
- Automatic mode covers 90% of use cases.
- Manual mode for special requirements.
- Users do not need to memorize complex rules.

### 🆕 Usage Scenarios for Specialized Analysis

**When to use specialized analysis?**

1. **--focus=opening**: Use immediately after completing the first 3 chapters.
   - The opening is key to reader retention.
   - Golden opening rules have strict requirements.
   - The cost of fixing problems early is lower.

2. **--focus=pacing**: Use once every 10-15 chapters.
   - Check if the pacing meets expectations.
   - Evaluate the reasonableness of high point/conflict distribution.
   - Adjust pacing based on rhythm-config.

3. **--focus=character**: Use after major turning points.
   - After the protagonist experiences a major event.
   - When supporting characters enter or exit.
   - When character relationships change.

4. **--focus=foreshadow**: Use after completing each volume.
   - Check for missed foreshadowing.
   - Evaluate the quality of foreshadowing setup.
   - Plan resolution timing in advance.

5. **--focus=logic**: Use after adjusting the outline.
   - After modifying important settings.
   - After adjusting the timeline.
   - After adding or deleting chapter content.

6. **--focus=style**: Use before bulk revisions.
   - Check consistency against style-reference.
   - Find AI-like phrasing and banned words.
   - Ensure the style matches the target works.

**Relationship between Specialized and Comprehensive Analysis**:
- **Comprehensive Analysis** (default): Suitable for phase-gate reviews (every 5-10 chapters).
- **Specialized Analysis** (--focus): Suitable for targeted optimization (when a problem is found).

**Suggested Workflow**:
1. Every 5-10 chapters completed → `/analyze` (comprehensive analysis).
2. Found an opening issue → `/analyze --focus=opening`.
3. Pacing feels off → `/analyze --focus=pacing`.
4. Unsure about logic → `/analyze --focus=logic`.
5. Check before revising → `/analyze --focus=style`.

### Objective and Constructive
- Analysis is based on data and standards.
- Avoid subjective assumptions.
- Provide specific, actionable suggestions.

### Progressive Improvement
- Analysis is for improvement, not criticism.
- Record the results of each analysis.
- Track the effectiveness of improvements.

### 🆕 Synergy with Other Features

**Files automatically read by specialized analysis**:
- `spec/presets/golden-opening.md` → opening analysis
- `spec/presets/rhythm-config.json` → pacing analysis
- `memory/style-reference.md` → style analysis
- `stories/*/specification.md` → benchmark for all analyses

**Advantages**:
- No need to manually specify reference files.
- Automatically applies standards from target works.
- Maintains consistency in analysis standards.

---

**Remember**: **One command, three modes (framework/content/specialized), intelligent and precise. The purpose of `analyze` is to make the work better, whether before writing, after writing, or for specific dimensions.**
