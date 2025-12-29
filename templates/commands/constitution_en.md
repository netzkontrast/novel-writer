---
description: Create or update the novel's creative constitution, defining non-negotiable creative principles.
argument-hint: [Description of creative principles]
allowed-tools: Write(//memory/constitution.md), Write(memory/constitution.md), Read(//memory/**), Read(memory/**), Bash(find:*), Bash(*)
model: claude-sonnet-4-5-20250929
scripts:
  sh: .specify/scripts/bash/constitution.sh
  ps: .specify/scripts/powershell/constitution.ps1
---

User Input: $ARGUMENTS

## Objective

To establish the core principles and values for the novel's creation, forming a "constitution" document for the creative process. These principles will guide all subsequent creative decisions.

## Execution Steps

### 1. Check Existing Documents

**First, check if a style reference document exists** (from `/book-internalize`):
```bash
test -f .specify/memory/style-reference.md && echo "exists" || echo "not-found"
```

- If it exists, use the Read tool to read `.specify/memory/style-reference.md`.
- Then inform the user: "I've detected that you have completed the analysis of a reference work. I will use that style as a reference to help you draft the constitution."

**Then, check for an existing constitution**:
```bash
test -f .specify/memory/constitution.md && echo "exists" || echo "not-found"
```

- If it exists (outputs "exists"), use the Read tool to read `.specify/memory/constitution.md` and prepare for an update.
- If it does not exist (outputs "not-found"), skip the reading step and proceed to create a new constitution.

### 2. Collect Creative Principles

Based on user input, collect principles for the following dimensions (if not provided, ask or infer):

#### Core Values
- What core idea does the work aim to convey?
- What are the absolute bottom lines that cannot be crossed?
- What is the fundamental purpose of the creation?

#### Quality Standards
- Requirements for logical consistency.
- Standards for writing quality.
- Commitment to update frequency.
- Guarantee of completion.

#### Creative Style Principles
- Narrative style (concise/ornate/simple/poetic).
- Pacing control (fast/slow/varied).
- Emotional tone (passionate/profound/lighthearted/serious).
- Language features (classical/modern/colloquial/written).

#### Content Principles
- Character development principles:
  - Every character must have a complete motivation.
  - Character growth must be logical.
  - Dialogue must fit the character's identity.
- Plot design principles:
  - Principles for conflict design.
  - Requirements for plausible plot twists.
  - Principles for foreshadowing and resolution.
- World-building principles:
  - Requirements for internal consistency of the setting.
  - Standards for the authenticity of details.
  - Requirements for cultural research.

#### Reader-Oriented Principles
- Target audience positioning.
- Guarantee of reader experience.
- Principles for interaction and feedback.

#### Creative Discipline
- Daily writing norms.
- Process for revision and improvement.
- Principles for version management.

### 3. Draft the Constitution Document

Use the following template structure:

```markdown
# Novel Creation Constitution

## Metadata
- Version: [Version number, e.g., 1.0.0]
- Creation Date: [YYYY-MM-DD]
- Last Revised: [YYYY-MM-DD]
- Author: [Author's Name]
- Work: [Work's Name or "General"]

## Preamble
[Explain why this constitution is needed and its binding force]

## Chapter 1: Core Values

### Principle 1: [Principle Name]
**Declaration**: [Clear statement of the principle]
**Reason**: [Why this principle is important]
**Execution**: [How to reflect it in the creation]

### Principle 2: [Principle Name]
[Same format as above]

## Chapter 2: Quality Standards

### Standard 1: Logical Consistency
**Requirement**: [Specific requirement]
**Verification Method**: [How to verify]
**Consequence of Violation**: [Must be corrected]

[More standards...]

## Chapter 3: Creative Style

### Style Principle 1: [Name]
**Definition**: [What this style is]
**Example**: [Specific examples]
**Taboo**: [What should absolutely not be done]

[More style principles...]

## Chapter 4: Content Guidelines

### Character Development Guidelines
[Specific guideline content]

### Plot Design Guidelines
[Specific guideline content]

### World-building Guidelines
[Specific guideline content]

## Chapter 5: Reader Contract

### Promises to the Readers
- [Promise 1]
- [Promise 2]
- [Promise 3]

### Bottom-line Guarantees
- [Guarantee 1]
- [Guarantee 2]

## Chapter 6: Revision Procedure

### Conditions for Triggering Revision
- Major changes in creative direction.
- Accumulated reader feedback.
- Personal growth and changes in understanding.

### Revision Process
1. Propose a motion for revision.
2. Assess the impact.
3. Update the version.
4. Record the changes.

## Appendix: Version History
- v1.0.0 (Date): Initial version
- [Subsequent version records]
```

### 4. Version Management

- **Major version number**: Major changes or deletions of principles.
- **Minor version number**: Addition of new principles or chapters.
- **Revision number**: Wording optimization, clarifying notes.

### 5. Consistency Propagation

Check and update related files to maintain consistency:
- Reference constitutional principles in subsequent commands.
- Suggest updating the creative philosophy section in the README.

### 6. Generate Impact Report

Output the impact of the constitution's creation/update:
```markdown
## Constitution Impact Report
- Version: [Old Version] → [New Version]
- New Principles: [List]
- Modified Principles: [List]
- Scope of Impact:
  ✅ Specification definition must follow the constitution.
  ✅ Plan development must comply with the principles.
  ✅ Creative execution must adhere to the guidelines.
  ✅ Verification must check for compliance.
```

### 7. Output and Save

- Save the constitution to `.specify/memory/constitution.md`.
- Output a success message for creation/update.
- Suggest the next step: `/specify` to define the story's specifications.

## Execution Principles

### Must Adhere To
- Principles must be verifiable, not too abstract.
- Use clear terms like "must," "prohibited."
- Every principle must have a clear reason.

### Should Include
- At least 3-5 core values.
- A clear quality baseline.
- Actionable creative guidelines.

### Avoid
- Vague slogans (e.g., "strive for excellence").
- Unverifiable requirements.
- Clauses that excessively restrict creativity.

## Example Principles

**Good Principles**:
- "The actions of main characters must have a clear chain of motivation; actions 'for the sake of the plot' are not allowed."
- "Every piece of foreshadowing must be resolved or explained within a reasonable time (at most 10 chapters)."
- "Never use modern internet slang that breaks the immersion of an ancient setting."

**Bad Principles**:
- "Write well" (too vague).
- "Pursue artistic quality" (unverifiable).
- "Satisfy the readers" (unclear standard).

## Subsequent Flow

After the constitution is established, all subsequent creative steps must follow it:
1. `/specify` - Specifications must align with the constitutional values.
2. `/plan` - The plan must follow the constitutional principles.
3. `/write` - The creation must adhere to the constitutional guidelines.
4. `/analyze` - Verification must check for constitutional compliance.

Remember: **The constitution is the supreme guide, but it can also be revised with the times.**
