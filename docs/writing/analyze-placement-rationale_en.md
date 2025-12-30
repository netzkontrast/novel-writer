# Design Rationale for the Different Placements of the Analyze Command

## Background

Community users have pointed out that **the position of the `analyze` command is inconsistent between spec-kit and novel-write**.

-   spec-kit (Code Development): `analyze` comes **before** `implement`.
-   novel-write (Novel Writing): `analyze` comes **after** `write`.

Is this a design flaw or an intentional choice? This document will provide a detailed rationale for the validity of this difference.

## Process Comparison

### Standard Flow for spec-kit (Software Development)

```
constitution → specify → clarify → plan → tasks → analyze → implement
                                                      ↑
                                           Verification before implementation
```

Source: [spec-kit README.md L216](https://github.com/github/spec-kit)

```markdown
| `/analyze` | Cross-artifact consistency & coverage analysis
             (run after /tasks, before /implement) |
```

**The role of `analyze`**:
-   Cross-verify the consistency between the specification, plan, and tasks.
-   Check for coverage (do all requirements have corresponding tasks?).
-   Identify contradictions and omissions (to be fixed before coding).
-   Ensure thorough preparation before implementation.

### Standard Flow for novel-write (Novel Writing)

```
constitution → specify → clarify → plan → tasks → write → analyze
                                                             ↑
                                              Verification after writing
```

Source: [novel-write templates/commands/write.md L115](../../templates/commands/write.md)

```markdown
/constitution → Provides creative principles
     ↓
/specify → Defines the story requirements
     ↓
/clarify → Clarifies key decisions
     ↓
/plan → Creates a technical plan
     ↓
/tasks → Decomposes into executable tasks
     ↓
/write → 【Current】Executes the writing
     ↓
/analyze → Verifies quality and consistency
```

**The role of `analyze`**:
-   Verify if the completed content aligns with the specification.
-   Check for plot coherence and character consistency.
-   Assess the achievement of quality standards.
-   Generate suggestions for improvement.

## Core Difference Analysis

### Difference 1: The Object of Verification

| Dimension | spec-kit (Code) | novel-write (Novel) |
|---|---|---|
| **Verification Object** | Specification documents, plans, task lists | The created content |
| **Verification Content**| Consistency, completeness, feasibility | Quality, coherence, thematic expression |
| **Timing of Verification** | Before implementation (Preventive) | After creation (Evaluative) |

### Difference 2: The Cost of Errors

**Cost of errors in code development**:
-   ❌ If you don't `analyze` before implementation, you might write 50% of the code before discovering a design contradiction.
-   ❌ The cost of refactoring is high; the written code might be discarded.
-   ✅ Pre-verification can prevent rework.

**Cost of errors in novel writing**:
-   ✅ Over-analyzing before writing can interrupt creative inspiration.
-   ✅ Completing a first draft and then making bulk revisions is a more efficient process.
-   ✅ Literary works allow for perfection through revision.

### Difference 3: The Nature of the Feedback Loop

**Code Development (Preventive Feedback)**:
```
plan → tasks → analyze → Discover contradiction → Modify plan/tasks → implement
          ↑_______________|
        (No code is lost)
```

**Novel Writing (Iterative Feedback)**:
```
plan → tasks → write → analyze → Discover issues → Revise content
                  ↑_______________________|
              (Content already exists; it's refinement, not rewriting)
```

## Rationale for the Design

### Why Does Code Need "analyze before implement"?

#### 1. **Immediate Modifiability**
-   Specifications, plans, and tasks are documents, and the cost of modifying them is low.
-   If `analyze` finds a problem, you can simply adjust the documents.
-   Pre-implementation adjustments do not waste coding effort.

#### 2. **Binary Correctness**
-   Code either compiles or it doesn't.
-   Tests either pass or they fail.
-   `analyze` can identify these binary issues in advance.

#### 3. **Dependency Complexity**
-   Code modules have strong interdependencies.
-   Once implementation begins, the dependency tree is set.
-   Early `analyze` can help optimize the dependency structure.

#### 4. **Tool-Based Verification**
-   `analyze` can automatically check:
    -   If all spec requirements have a corresponding task.
    -   If tasks cover all components in the plan.
    -   If contracts are consistent with the data-model.
-   These checks do not require actual code.

**Example: `analyze` checks in spec-kit**
```markdown
## Coverage Analysis
- [ ] All P0 requirements have corresponding tasks
- [ ] All entities in data-model.md have CRUD tasks
- [ ] All API endpoints in contracts/ have implementation tasks

## Consistency Analysis
- [ ] Plan's tech stack matches tasks' tooling
- [ ] No contradictions between spec.md and plan.md
- [ ] Test tasks exist before implementation tasks (TDD)
```

### Why Does Novel Writing Need "write before analyze"?

#### 1. **Protection of Creative Flow**
-   Writing requires a continuous state of creativity (flow state).
-   Frequent interruptions can destroy narrative rhythm and inspiration.
-   Completing a first draft before analyzing in bulk is more aligned with the creative process.

#### 2. **The Continuity of Quality**
-   Literary quality is not binary (it's not "right/wrong").
-   It requires a rating scale and dimensions of analysis (1-10 points).
-   There must be actual content to judge its quality.

#### 3. **Context Dependency**
-   A single chapter cannot be used to verify theme, pacing, or character arcs.
-   A certain volume of content (5-10 chapters) is needed for a deep analysis.
-   Bulk verification is more effective than chapter-by-chapter verification.

#### 4. **Iterative Refinement Model**
-   Novel writing is essentially a process of "draft → revise → polish".
-   The first draft is allowed to be imperfect and is adjusted after `analyze`.
-   This model is fundamentally different from the "test-first" approach in coding.

**Example: `analyze` checks in novel-write**
```markdown
## Constitution Compliance Check
- [ ] Does it align with core values?
- [ ] Does it meet quality standards?

## Specification Alignment Analysis
- [ ] Have P0 required elements been implemented?
- [ ] Target reader suitability score.

## Content Quality Analysis
- [ ] Plot density score.
- [ ] Character consistency check.
- [ ] Pacing variation analysis.
```

## Engineering vs. Art: The Fundamental Difference

### Dimensional Comparison Table

| Dimension | Code Development (Engineering) | Novel Writing (Art) |
|---|---|---|
| **Standard of Correctness** | Binary (Pass/Fail) | Continuous Spectrum (1-10 points) |
| **Timing of Verification** | Preventive (Beforehand) | Evaluative (Afterward) |
| **Cost of Modification** | High after implementation, low before | Acceptable after the first draft |
| **Workflow Characteristic**| Interruptible, can be parallelized | Requires continuity |
| **Feedback Mechanism** | Automated tool verification | Human + AI comprehensive judgment |
| **Granularity of Verification**| Fine-grained (Function-level) | Coarse-grained (Chapter/Volume-level) |
| **Timing of Perfection** | Right the first time | Gradually perfected through iteration |

### Psychological Differences

**The Engineer's Working Model**:
-   Design the architecture first (can be deliberated repeatedly).
-   Start coding only after confirming it's correct.
-   A problem encountered during coding is a design flaw.

**The Writer's Working Model**:
-   Quickly capture inspiration first (don't let rationality interrupt).
-   Review the big picture after completing the first draft.
-   Revision is a necessary part of the creative process.

### Validation by Historical Experience

**Best Practices in Software Engineering**:
-   TDD (Test-Driven Development): Write tests before code.
-   Design Review: Review the design before implementation.
-   Architecture Review: Validate the architecture before coding.
-   **Conclusion**: Upfront verification has been proven to be a best practice in the industry.

**Best Practices in Literary Creation**:
-   Hemingway: "The first draft of anything is shit" (get it done, then make it perfect).
-   Stephen King: Write the complete story first, then put it away for 6 weeks before revising.
-   Standard workflow in writing workshops: Draft → Critique → Revise.
-   **Conclusion**: Post-writing revision is the standard process in creation.

## What if it Were the Other Way Around?

### Hypothesis 1: Code also adopts "implement before analyze"

```
plan → tasks → implement → analyze → Discover issues
                    ↓
                Wrote 5000 lines of code
                    ↓
            `analyze` finds an architectural contradiction
                    ↓
            Needs refactoring or rewriting ❌
```

**Problems**:
-   A significant amount of coding time is wasted.
-   Technical debt may have already accumulated.
-   Team morale suffers.

### Hypothesis 2: Novels also adopt "analyze before write"

```
plan → tasks → analyze (check the plan) → write
                ↑
        Check what? There's no content!
```

**Problems**:
-   `analyze` cannot check content that does not yet exist.
-   It can only check the logic of the plan, but this is already done in the `clarify` stage.
-   Over-analysis can lead to "perfectionism paralysis," preventing the first draft from ever being written.

## Practical Usage Scenarios

### `analyze` Usage Scenario in spec-kit

**When**: After completing `tasks.md`, before starting `implement`.

**Typical Dialogue**:
```
User: /tasks
AI: Task list generated, containing 23 tasks.

User: /analyze
AI:
Analysis complete!

【Coverage Analysis】
✅ All P0 requirements have corresponding tasks.
⚠️ The "user notification" feature in the spec does not have a corresponding task.
❌ The Comment entity in data-model.md is missing CRUD tasks.

【Consistency Analysis】
✅ The plan uses PostgreSQL, and the tasks are correctly configured for the database.
❌ The spec requires "real-time updates," but there is no WebSocket in the plan.

Suggestions:
1. Add 3 tasks for the notification feature.
2. Add 4 tasks for Comment CRUD.
3. Supplement the plan with a WebSocket solution.

Fix these issues before running /implement.
```

### `analyze` Usage Scenario in novel-write

**When**: After completing 5-10 chapters, or after finishing a volume.

**Typical Dialogue**:
```
User: /write (completes chapters 1-5)

User: /analyze
AI:
Comprehensive Analysis Report

【Constitution Compliance】8/10
✅ Aligns with core values.
⚠️ The pacing in Chapter 3 is slightly slow, which might affect reader retention.

【Specification Alignment】7/10
✅ P0 element "father-son conflict" is fully demonstrated.
❌ P0 element "suspense setting" is insufficient; there should be a turning point at the end of Chapter 5.

【Content Quality】
- Plot Density: Medium
- Character Consistency: The protagonist's reaction to the same event is contradictory in Chapters 2 and 4.
- Pacing: The first 3 chapters are slow; the acceleration in chapters 4-5 is reasonable.

【Improvement Suggestions】
P0 (Urgent):
1. Fix the protagonist's personality contradiction (Chapter 2 vs. Chapter 4).
2. Add a suspenseful element at the end of Chapter 5.

P1 (Optimization):
1. Chapter 3 can be condensed by 20% to improve pacing.
```

## Conclusion

### The Difference in `analyze` Placement is a Correct Design

| Project | Command Position | Reason |
|---|---|---|
| **spec-kit** | tasks → **analyze** → implement | Preventive verification to reduce implementation costs |
| **novel-write** | tasks → write → **analyze** | Evaluative verification to protect creative flow |

### Design Principles

**Engineering Activities** (Code):
-   Follow the principle of "Measure twice, cut once."
-   Upfront verification can prevent rework.
-   `analyze` comes before `implement`.

**Artistic Activities** (Novel):
-   Follow the principle of "Done is better than perfect."
-   The first draft is allowed to be imperfect; polishing comes later.
-   `analyze` comes after `write`.

### A Unified Methodological Philosophy

Although the placement of `analyze` is different, both adhere to the core philosophy of **Specification-Driven Development**:

```
Specification (what) → Plan (how) → Execute (do) → Verify (check)
```

The difference lies in the **timing of Verification**:
-   Code: Verify before Execute (Preventive)
-   Novel: Verify after Execute (Evaluative)

This difference demonstrates the **flexibility of the SDD methodology**: the same conceptual framework, with implementation details adjusted based on the characteristics of the domain.

### Advice for Users

**When developing software with spec-kit**:
-   ✅ Be sure to run `analyze` before `implement`.
-   ✅ Modify the plan/tasks based on the `analyze` report.
-   ✅ Start coding only after `analyze` passes all checks.

**When writing a novel with novel-write**:
-   ✅ Focus on writing; do not analyze frequently during the process.
-   ✅ Run `analyze` after accumulating a certain amount of content (5-10 chapters or a volume).
-   ✅ Make bulk revisions based on the `analyze` report.

---

**Final Summary**: This is not a design flaw but a **domain-driven, rational difference**. Code needs a "strategist beforehand," while a novel needs one "after the fact."

---

## Appendix: Smart Dual-Mode Design (New in v0.12.0)

### The Problem: Both Needs are Valid

After community discussion, we found that both types of `analyze` needs are valuable:

1.  **Framework analysis before `write`** (suggested by Zeng Xisheng)
    -   Verify the consistency of the specification, plan, and tasks.
    -   Ensure thorough preparation before starting to write.
    -   Avoid discovering design flaws halfway through writing.

2.  **Content analysis after `write`** (the original design)
    -   Verify the quality of the completed content.
    -   Check if it aligns with the specification.
    -   Provide suggestions for improvement.

### The Solution: One Command, Dual Intelligence

Adhering to the **principle of restraint** (avoiding command explosion), we have implemented a **smart dual-mode `analyze`**:

```bash
/analyze  # Automatically determines which analysis to perform
```

### Smart Detection Logic

The system automatically detects the current creation stage to decide which analysis to perform:

```
Number of chapters = 0     → Framework analysis (preparation stage)
Number of chapters < 3     → Framework analysis (suggests continuing to write)
Number of chapters ≥ 3     → Content analysis (quality verification)
```

### Manual Control Mode

Users can also explicitly specify the analysis type:

```bash
/analyze --type=framework  # Force framework analysis
/analyze --type=content    # Force content analysis
```

### Comparison of the Two Analysis Modes

| Dimension | Framework Analysis | Content Analysis |
|---|---|---|
| **Execution Time** | Before `write` | After `write` |
| **Analysis Object** | Spec/plan/task documents | Completed chapter content |
| **Core Goal** | Verify readiness | Verify quality standards |
| **Check Items** | Coverage, consistency, logical warnings | Constitution compliance, spec alignment, content quality |
| **Output Suggestions**| Fix document contradictions, add missing tasks | Fix content issues, optimize chapter quality |

### Practical Usage Examples

#### Example 1: Preparation Stage

```bash
User: /tasks
AI: 28 writing tasks have been generated.

User: /analyze
AI: No chapter content detected. Executing framework consistency analysis...

【Coverage Analysis】
⚠️ The "villain setting" spec requirement has no corresponding task.
❌ The plan for chapters 10-12 is missing task decomposition.

【Preparation Score】6/10
Suggestion: Fix the above issues before starting to write.
```

#### Example 2: After Completing Several Chapters

```bash
User: /write (completes chapters 1-7)

User: /analyze
AI: 7 chapters detected. Executing content quality analysis...

【Constitution Compliance】8/10 ✅
【Specification Alignment】7/10 ⚠️ P0 requirement coverage is 67%.
【Content Quality】7.5/10 ⚠️ There are 2 logical issues.

【Improvement Suggestions】
P0: Fix timeline contradictions and character ability contradictions.
P1: Add suspense at the end of Chapter 5.

【Suggestion】: After fixing P0 issues, continue writing chapters 8-10.
```

### Design Philosophy

**Restrained but not simple**:
-   ❌ Do not create two separate commands like `/framework-analyze` and `/content-analyze`.
-   ✅ One `/analyze` command that intelligently judges the context.
-   ✅ Support for manual mode specification (for advanced users).
-   ✅ 90% of scenarios are handled automatically; 10% can be controlled manually.

**The principle of restraint in WeChat**:
-   Powerful features, but simple commands.
-   The system is smart, so the user is not burdened.
-   Advanced features are hidden and do not interfere with regular use.

### Technical Implementation

See `templates/commands/analyze.md` and `scripts/bash/check-analyze-stage.sh` for details.

---

**Conclusion**: The smart dual-mode design satisfies both analysis needs while maintaining command simplicity. This is a perfect example of balancing the **principle of restraint** with **user needs**.
