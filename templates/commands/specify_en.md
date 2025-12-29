---
description: Define the story specifications, clarifying what kind of work to create.
argument-hint: [Story description]
allowed-tools: Read(//stories/**/specification.md), Read(stories/**/specification.md), Write(//stories/**/specification.md), Write(stories/**/specification.md), Read(//memory/constitution.md), Read(memory/constitution.md), Bash(find:*), Bash(*)
model: claude-sonnet-4-5-20250929
scripts:
  sh: .specify/scripts/bash/specify-story.sh --json
  ps: .specify/scripts/powershell/specify-story.ps1 -Json
---

User Input: $ARGUMENTS

## Objective

To define the story like a product requirements document (PRD), clarifying "what to create" rather than "how to create." Supports **progressive specification definition**, moving from a single sentence to a full specification in stages. Outputs specifications with `[Clarification Needed]` tags to leave room for subsequent clarification steps.

## Progressive Specification Levels

Automatically determines and generates the appropriate level of specification based on the detail of the user's input:

**Level 1 - One-Sentence Story (Logline)**:
- A core idea of 30 words or less.
- Example: "A hacker with amnesia discovers he is living in a virtual world."
- Applicable: Initial inspiration stage.

**Level 2 - One-Paragraph Summary (Premise)**:
- A story summary of 100-200 words.
- Includes: protagonist, conflict, goal, obstacle.
- Applicable: Idea validation stage.

**Level 3 - One-Page Outline (One-Page Spec)**:
- Includes: summary, protagonist, main conflict, three-act structure.
- Simplified version of the specification (about 500 words).
- Applicable: Rapid prototyping stage.

**Level 4 - Full Specification**:
- Contains the full nine-chapter specification content.
- Detailed plotline management, success criteria, etc.
- Applicable: Formal creation preparation stage.

## Execution Steps

### 0. Determine Specification Level

**Automatically determine the level based on user input**:

```python
input_length = len($ARGUMENTS)
existing_spec = check_if_exists()

if existing_spec:
    # Existing spec, perform upgrade or modification
    level = get_current_level(existing_spec)
    action = "upgrade" or "modify"
elif input_length < 50:
    # One-sentence input → Level 1
    target_level = 1
elif input_length < 300:
    # One-paragraph input → Level 2
    target_level = 2
elif input_length < 1000:
    # Brief description → Level 3
    target_level = 3
else:
    # Detailed description → Level 4
    target_level = 4
```

**Decision Logic**:
- If a specification file already exists, ask the user whether to "upgrade the level" or "modify the content."
- If it's a new story, automatically select the level based on the input length.
- After generating the specification, prompt the user on how to expand to the next level.

### 1. Initialize Story Specification

Run `{SCRIPT}` to get path information:
- Parse JSON to get `STORY_NAME` and `SPEC_PATH`.
- If it's a new story, create the specification file.
- If it already exists, prepare to update or upgrade.

### 2. Check for Constitution Compliance

If `.specify/memory/constitution.md` exists:
- Load the constitutional principles.
- Ensure the specification complies with the constitutional values.
- Reference the relevant principles in the specification.

### 3. Create Story Specification Document

**Generate the corresponding specification based on the determined level**:

---

#### Level 1 Specification Template (One-Sentence Story)

```markdown
# Story Specification Document

## Metadata
- Story Name: [Name]
- Version: 1.0.0-L1 (One-Sentence)
- Creation Date: [YYYY-MM-DD]
- Current Level: Level 1
- Author: [Author's Name]

## One-Sentence Story (Logline)

[Core idea of 30 words or less]

**Examples**:
- "A hacker with amnesia discovers he is living in a virtual world."
- "A regular high school student gains the ability to see the future, but each use shortens his lifespan."
- "A retired special agent must complete an impossible mission to save his daughter."

## Core Element Identification

Based on this sentence, identify the following core elements:
- **Protagonist**: [Who]
- **Dilemma/Conflict**: [What problem are they facing]
- **Goal**: [What they want to achieve]
- **Obstacle/Cost**: [What is standing in their way]

## Next Steps

✅ **Level 1 Specification is complete.**

**Expansion Suggestions**:
1. Expand the one sentence into a paragraph (100-200 words).
2. Run the `/specify` command with the expanded paragraph as input.
3. The system will automatically upgrade to Level 2.

**Expansion Tips**:
- Add the protagonist's background and motivation.
- Clarify the cause of the main conflict.
- Hint at the story's direction and possible outcomes.
```

---

#### Level 2 Specification Template (One-Paragraph Summary)

```markdown
# Story Specification Document

## Metadata
- Story Name: [Name]
- Version: 1.1.0-L2 (One-Paragraph)
- Creation Date: [YYYY-MM-DD]
- Current Level: Level 2
- Author: [Author's Name]

## One-Sentence Story (Logline)

[Inherited from Level 1, or newly extracted]

## One-Paragraph Summary (Premise)

[A story summary of 100-200 words, including protagonist, conflict, goal, and obstacle]

**Example Structure**:
{Protagonist's background} in {initial situation}, discovers/encounters {core conflict}. In order to {goal}, he/she must {action}, but {main obstacle} makes it {difficult/costly}. Ultimately, {hint at the ending's direction}.

## Core Elements

### Protagonist Setting
- **Identity**: [Profession/Age/Background]
- **Personality Traits**: [2-3 keywords]
- **Core Desire**: [What they want]
- **Fatal Flaw**: [What holds them back]

### Core Conflict
- **External Conflict**: [What are they fighting against]
- **Internal Conflict**: [What is their inner struggle]
- **Cause of Conflict**: [Why is it erupting now]

### Story Direction
- **Beginning**: [Where the story starts]
- **Midpoint**: [Expected turning point]
- **End**: [Possible directions for the ending]

## Preliminary Genre Positioning

- **Main Genre**: [Fantasy/Urban/Historical/Sci-Fi, etc.]
- **Subgenre**: [Specific sub-genre]
- **Reference Works**: Similar to [Work's Name]'s [specific feature].

## Next Steps

✅ **Level 2 Specification is complete.**

**Expansion Suggestions**:
1. Expand the summary into a one-page outline (about 500 words).
2. Add the three-act structure, main characters, and key scenes.
3. Run the `/specify` command with the expanded content as input.
4. The system will automatically upgrade to Level 3.

**Expansion Tips**:
- Define the dividing points of the three-act structure.
- Add 2-3 main supporting characters.
- List 5-10 key scenes.
- Think about the basic settings of the world-building.
```

---

#### Level 3 Specification Template (One-Page Outline)

```markdown
# Story Specification Document

## Metadata
- Story Name: [Name]
- Version: 1.2.0-L3 (One-Page)
- Creation Date: [YYYY-MM-DD]
- Current Level: Level 3
- Author: [Author's Name]

## I. Story Core

### One-Sentence Story
[Inherited from Level 1]

### Story Summary (100-200 words)
[Inherited from Level 2]

### Core Theme
- **Theme**: [e.g., "Growth," "Redemption," "Revenge"]
- **Emotional Core**: [What you want the reader to feel]

## II. Main Characters (3-5)

### Protagonist
- **Name**: [Name]
- **Identity/Background**: [Brief]
- **Core Desire**: [What they want]
- **Character Arc**: From [State A] → experiences [Conflict] → becomes [State B]

### Main Supporting Character 1
[Name, functional role, relationship with the protagonist]

### Main Supporting Character 2
[Name, functional role, relationship with the protagonist]

### Antagonist/Opposing Force
[Name/Faction, motivation, threat level]

## III. Three-Act Structure

### Act 1: Setup (Approx. 25% of the story)
- **Beginning**: [The ordinary world]
- **Inciting Incident**: [Event that disrupts the balance]
- **Decision**: [Protagonist decides to act]
- **Entering the New World**: [Leaving the comfort zone]

### Act 2: Confrontation (Approx. 50% of the story)
- **First Half**: [Trials and failures]
- **Midpoint**: [Major turning point / false victory or defeat]
- **Second Half**: [Situation worsens / dark moment]
- **Lowest Point**: [All seems lost]

### Act 3: Resolution (Approx. 25% of the story)
- **Epiphany**: [Protagonist's awakening/growth]
- **Final Confrontation**: [Climactic battle/conflict]
- **Resolution**: [The new state of balance]

## IV. Key Scenes (5-10)

1.  **[Scene 1 Name]**: [1-2 sentence description, emotional purpose]
2.  **[Scene 2 Name]**: [Description]
3.  **[Scene 3 Name]**: [Description]
4.  ...

## V. Target Positioning

### Target Audience
- **Age Range**: [Range]
- **Genre Preference**: [Fantasy/Urban/Historical, etc.]
- **Reading Scenario**: [Fragmented time/in-depth reading]

### Market Positioning
- **Main Genre**: [Genre]
- **Subgenre**: [Sub-genre]
- **Competitive Analysis**: Similar to [Work 1]'s [feature] + [Work 2]'s [feature]
- **Differentiation**: [Core selling point]

### Quantitative Goals
- **Target Word Count**: [30k/100k/500k]
- **Update Frequency**: [Daily/Weekly]
- **Completion Time**: [Estimated duration]

## VI. Next Steps

✅ **Level 3 Specification is complete.**

**Expansion Suggestions**:
1. Expand the one-page outline into a full specification.
2. Add detailed plotline management specifications (multiple plotlines, foreshadowing, intersection points).
3. Add success criteria, constraints, and risk assessment.
4. Run the `/specify` command and tell the AI to "expand to full specification."
5. The system will automatically upgrade to Level 4.

**Expansion Tips**:
- Define a multi-plotline management strategy (if any).
- List all constraints and red lines.
- Mark 5-10 decision points that need clarification.
- Prepare reference materials and sources of inspiration.
```

---

#### Level 4 Specification Template (Full Specification)

Use the full nine-chapter specification structure (keep the original template):

```markdown
# Story Specification Document

## Metadata
- Story Name: [Name]
- Version: 1.0.0
- Creation Date: [YYYY-MM-DD]
- Status: Draft
- Author: [Author's Name]

## I. Story Summary

### One-Sentence Story (Elevator Pitch)
[Describe the core of the story in 30 words or less]

### Story Summary (100-200 words)
[Expanded description, including the main conflict and a hint of the ending]

### Core Theme
- Theme: [e.g., "Growth," "Redemption," "Revenge"]
- Deeper Meaning: [What you want to express]
- Emotional Core: [What you want the reader to feel]

## II. Target Positioning

### Target Audience Profile
- Age Range: [Clarification Needed: specific age range]
- Gender Preference: [Clarification Needed: Male-oriented/Female-oriented/General]
- Reading Level: [Clarification Needed: Beginner/Intermediate/Advanced]
- Genre Preference: [Fantasy/Urban/Historical, etc.]
- Reading Scenario: [Fragmented time/in-depth reading]

### Market Positioning
- Main Genre: [Clarification Needed: Shuangwen/Suspense/Romance/Serious Literature/Sci-Fi/Fantasy/Historical/Urban/Other]
- Subgenre: [Specific sub-genre, e.g., system-flow/whodunit/CEO romance, etc.]
- Genre Fusion: [If any, e.g., suspense + romance]
- Genre Tags: [Main Tag] + [Secondary Tag]
- Competitive Analysis: Similar to [Work 1]'s [feature] + [Work 2]'s [feature]
- Differentiation: [Clarification Needed: What is the core selling point]

## III. Success Criteria

### Quantitative Metrics
- Target Word Count: [Clarification Needed: 30k/100k/500k]
- Update Frequency: [Clarification Needed: Daily/Weekly/Monthly]
- Completion Time: [Estimated duration]
- Commercial Goals: [If applicable]

### Quality Standards
- Logical Consistency: [Must/Should] have no obvious loopholes.
- Character Richness: The protagonist has [X] layers, supporting characters have [Y] layers.
- Plot Compactness: [Clarification Needed: Every chapter has conflict/transitional chapters are allowed].
- Writing Quality: [Clarification Needed: Easy to understand/Literary/Professional].

### Reader Feedback Metrics
- Target Rating: [If applicable]
- Engagement Rate: [Comment/collection ratio]
- Completion Rate: [Desired reader completion percentage]

## IV. Core Requirements

### Must-Haves (P0)
1. [Core plot element 1]
2. [Core character relationship]
3. [Core conflict setting]
4. [Necessary world-building element]

### Should-Haves (P1)
1. [Experience-enhancing element]
2. [Content that deepens the theme]
3. [Subplots that enrich the characters]

### Could-Haves (P2)
1. [Nice-to-have content]
2. [Optional subplots]
3. [Extra Easter eggs]

## V. Plotline Management Specification

> **Multi-Plotline Management Note**: This chapter defines all the story's plotlines (main plot, subplots) and their management strategies, used to solve issues with parallel progression, control of intersection timing, and ensuring consistency after modifications.

### 5.1 Plotline Definition Table

Defines the basic information for all story plotlines:

| Plotline ID | Plotline Name | Type | Priority | Start-End Chapter | Core Conflict | Main Characters |
|---|---|---|---|---|---|---|
| PL-01 | [Plotline 1 Name, e.g., "Family Line"] | Main/Sub/Main Support | P0/P1/P2 | [Start-End] | [Core conflict of the plotline] | [Main characters involved] |
| PL-02 | [Plotline 2 Name, e.g., "Love Line"] | Main/Sub/Main Support | P0/P1/P2 | [Start-End] | [Core conflict of the plotline] | [Main characters involved] |

**Notes**:
- Plotline ID format: PL-XX (abbreviation for Plotline)
- Type: Main (drives the story's development), Sub (enriches the plot), Main Support (serves the main plot)
- Priority: P0 (Must-have), P1 (Important), P2 (Optional)

### 5.2 Plotline Pacing Plan

Plans the activity level of each plotline at different stages:

| Plotline ID | Volume 1 | Volume 2 | Volume 3 | Volume 4 |
|---|---|---|---|---|
| PL-01 | ⭐⭐⭐ Active | ⭐⭐ Medium | ⭐ Background | ⭐⭐⭐ Active |
| PL-02 | ⭐⭐ Starting | ⭐⭐⭐ Active | ⭐⭐⭐ Active | ⭐⭐ Wrapping up |

**Notes**:
- ⭐⭐⭐ Active: A focus of this volume, gets a lot of page time.
- ⭐⭐ Medium: Normal progression, gets a moderate amount of page time.
- ⭐ Background: Mentioned occasionally to maintain its presence.
- ❌ Not present: The plotline has not yet started.

### 5.3 Plotline Intersection Point Plan

Pre-plan the intersection points between plotlines to avoid the AI improvising:

| Intersection ID | Chapter | Involved Plotlines | Intersection Content | Expected Effect |
|---|---|---|---|---|
| X-001 | [Chapter #] | PL-XX + PL-YY | [How two/more plotlines intersect, what happens] | [Impact on the story and characters] |
| X-002 | [Chapter #] | PL-XX + PL-YY + PL-ZZ | [Specific content of the intersection] | [Expected effect to be produced] |

**Notes**:
- Intersection ID format: X-XXX (from the first letter of Intersection)
- Involved Plotlines: List all plotline IDs that intersect here.
- Intersection Content: The specific plot or event.
- Expected Effect: Emotional conflict, plot twist, character growth, etc.

### 5.4 Foreshadowing Management Table

Manage the setup and reveal of all foreshadowing to ensure nothing is missed:

| Foreshadowing ID | Setup Chapter | Involved Plotlines | Foreshadowing Content | Reveal Chapter | Reveal Method |
|---|---|---|---|---|---|
| F-001 | [Chapter #] | PL-XX | [What foreshadowing is planted, specific content] | [Chapter #] | [How it's revealed, through what event] |
| F-002 | [Chapter #] | PL-XX + PL-YY | [Cross-plotline foreshadowing content] | [Chapter #] | [Reveal method] |

**Notes**:
- Foreshadowing ID format: F-XXX (from the first letter of Foreshadowing)
- Involved Plotlines: The plotlines associated with the foreshadowing, may cross multiple plotlines.
- The setup and reveal chapter numbers must be clear to avoid being forgotten.

### 5.5 Plotline Modification Decision Matrix

When a plotline needs to be modified, this process must be followed to assess the impact:

**Modification Checklist**:
1. Check section 5.2: In which volumes is this plotline active? Does the activity level need to be adjusted?
2. Check section 5.3: Which intersection points does this plotline involve? Does the timing of the intersection points need to change?
3. Check section 5.4: Which foreshadowing does this plotline involve? Does the setup/reveal of the foreshadowing need to be adjusted?
4. Check creative-plan.md: Which chapter sections need to be synchronized with the changes?
5. Check tasks.md: Which writing tasks need to be re-planned?
6. Check plot-tracker.json: What is the current progress, and how should it be adjusted going forward?

**Example**: Suppose you modify PL-03 (the love line), moving a character's appearance up to Chapter 50 (originally planned for Chapter 100):
- ✅ Intersection point X-004 needs to be moved up or canceled.
- ✅ The active plotlines for the chapter sections in Volume 2 need to be adjusted.
- ✅ The "Involved Plotlines" field of the relevant tasks needs to be updated.
- ✅ It may affect the timing of the reveal for foreshadowing F-002.

### 5.6 Plotline Consistency Principles

**Planning Principles**:
- Each plotline must have a clear beginning, middle, and end.
- The main plot should occupy 40-60% of the total length.
- No more than 2-3 subplots to avoid being too scattered.
- Plotline intersections should serve the theme, not just happen for the sake of it.
- Long-dormant plotlines (>20 chapters) must have a reasonable justification.

**Validation Criteria**:
- [ ] All plotlines have clear conflicts and resolutions.
- [ ] The plotline pacing distribution is reasonable, with no excessive concentration or gaps.
- [ ] The number of intersection points is moderate (recommend 2-3 every 50 chapters).
- [ ] All foreshadowing has a corresponding reveal plan.
- [ ] The modification decision matrix is actionable.

## VI. Constraints

### Content Red Lines
- Absolutely Prohibited: [e.g., illegal content]
- To Be Avoided: [e.g., sensitive topics]
- Handle with Care: [Clarification Needed: how to handle romantic relationships]

### Creative Constraints
- Knowledge Limitations: [Clarification Needed: is professional knowledge required]
- Time Constraints: [Completion deadline]
- Resource Constraints: [e.g., required reference materials]

### Technical Constraints
- Publishing Platform: [Clarification Needed: web novel platform/publishing/self-media]
- Formatting Requirements: [Chapter length, etc.]
- Update Requirements: [Fixed times, etc.]

## VII. Risk Assessment

### Creative Risks
- Writing Difficulty: [Clarification Needed: where are the challenges]
- Running out of Inspiration: [How to cope]
- Logical Loopholes: [Complexity assessment]
- Multi-Plotline Management: [Clarification Needed: are there too many plotlines]

### Market Risks
- Homogenization: [How to differentiate]
- Reader Acceptance: [Clarification Needed: is the innovation excessive]
- Timeliness: [Will the theme become outdated]

## VIII. Core Decision Points [Clarification Needed]

The following key decisions need to be clarified in the `/clarify` stage:
1. [Decision 1: e.g., is the protagonist's personality passionate or calm]
2. [Decision 2: e.g., is the ending open or happy]
3. [Decision 3: e.g., is the narrative single-line or multi-line]
4. [Decision 4: e.g., is the pacing fast or slow]
5. [Decision 5: e.g., is the style lighthearted or serious]

## IX. Validation Checklist

- [ ] The story summary is clear and unambiguous.
- [ ] The target audience is accurately defined.
- [ ] The success criteria are measurable.
- [ ] The core requirements are listed.
- [ ] The plotline management specification is defined.
- [ ] The constraints are identified.
- [ ] The risks are assessed.
- [ ] The key decision points are marked.

## Appendix: Reference Materials

### Sources of Inspiration
- [Source 1]
- [Source 2]

### Reference Works
- [Work 1]: Reference its [feature]
- [Work 2]: Reference its [feature]

### Additional Notes
[Other content that needs to be explained]
```

### 4. Output Specification Based on Level

**Output the corresponding template based on the level determined in step 0**:

- **Level 1**: Output the one-sentence spec + core element identification + expansion guide.
- **Level 2**: Output the one-paragraph spec + core elements + genre positioning + expansion guide.
- **Level 3**: Output the one-page spec + three-act structure + key scenes + expansion guide.
- **Level 4**: Output the full nine-chapter spec + `[Clarification Needed]` tags.

**Prompt after output**:

For Levels 1-3, inform the user after completion:
```
✅ Level X Specification is complete!

**Current Progress**: One-Sentence → One-Paragraph → One-Page → Full Spec
                 [=====---] (You are here)

**Next Options**:
1. Start creating directly with the current specification (suitable for rapid prototyping).
2. Expand to the next level (for more detailed planning).
3. Run `/clarify` to clarify the current specification.

**How to Expand**:
- Provide a more detailed story description.
- Run `/specify [detailed description]` again.
- The system will automatically upgrade to Level X+1.
```

For Level 4, inform the user after completion:
```
✅ Full Specification is complete!

**Next Steps**:
1. Run `/clarify` to clarify all decision points marked with [Clarification Needed].
2. Or run `/plan` directly to start developing the creative plan.
```

### 5. Mark Points Needing Clarification (Level 4 only)

**Only in the Level 4 full specification**, mark all decision points that need further clarification:
- Use the `[Clarification Needed: specific question]` format.
- Ensure 5-10 key decision points are marked.
- These will be handled in the `/clarify` step.

**Levels 1-3 do not need clarification points marked**, as they are themselves a process of progressive clarification.

### 6. Version Management

**Progressive Version Numbering Rules**:

- **Level 1**: 1.0.0-L1 (One-Sentence)
- **Level 2**: 1.1.0-L2 (One-Paragraph)
- **Level 3**: 1.2.0-L3 (One-Page)
- **Level 4**: 1.0.0 (Full Spec - Draft)
- **After Clarification**: 1.1.0 (Clarified Version)
- **After Planning**: 1.2.0 (Confirmed Version)
- **In Execution**: 2.0.0 (Execution Version)

**Upgrade Rules**:
- Level 1 → Level 2: Minor version number +1
- Level 2 → Level 3: Minor version number +1
- Level 3 → Level 4: Reset to 1.0.0 (entering the formal specification cycle)
- Subsequent modifications to Level 4: Follow the original rules.

### 7. Output and Save

- Save the specification to `stories/[story-name]/specification.md`.
- Output a creation success message.
- **Prompt the next step based on the level**:
  - Levels 1-3: Prompt on how to expand to the next level.
  - Level 4: Prompt to run `/clarify` to clarify key decisions.

## Notes

### The Value of Progressive Specification

**Why is progressive specification needed?**
- ✅ **Lowers the barrier to entry**: Start with one sentence, no need to complete a full specification at once.
- ✅ **Quick validation**: Validate if an idea is feasible before investing a lot of effort.
- ✅ **Natural evolution**: Naturally expand the specification details as your thinking deepens.
- ✅ **Avoids over-planning**: Decide the depth of the specification based on actual needs.

**When to use which level?**
- **Level 1**: Inspiration stage, to quickly record ideas.
- **Level 2**: Validation stage, to confirm if a story is worth writing.
- **Level 3**: Prototyping stage, to start writing quickly (suitable for short and medium-length stories).
- **Level 4**: Formal stage, for long novels or works that require detailed planning.

### Focus on WHAT, not HOW
- ✅ Correct: "Need a villain that readers will hate."
- ❌ Incorrect: "The villain appears in Chapter 3, using a flashback."

### Maintain Flexibility in the Specification
- Leave room for clarification (Level 4).
- Don't determine details too early (Levels 1-3).
- Mark all uncertain points (Level 4).

### Principles of Progressive Expansion
- **Inheritance**: Level N+1 should include all the content of Level N.
- **Incrementality**: Only add necessary details each time.
- **Reversibility**: You can go back to a lower level to modify core elements.
- **Jumpability**: You can jump directly from Level 1 to Level 4 (if you have thought it through).

### Relationship with Subsequent Steps
- **Levels 1-3**: Can start writing directly (suitable for exploratory writing).
- **Level 4**: Run `/clarify` to handle all `[Clarification Needed]` tags.
- **After Clarification**: Run `/plan` to develop the creative plan.
- **During Execution**: Run `/analyze` to verify if the implementation meets the specification.

Remember: **Specifications define the goal, not the path. Progressive specification takes you from vague to clear, rather than demanding clarity from the start.**
