---
description: Clarifies ambiguous points in the story outline through targeted Q&A to ensure a clear creative direction, supports focus parameters.
argument-hint: [Keyword or focus area]
allowed-tools: Read(//stories/**), Read(stories/**), Read(//plugins/**), Read(plugins/**), Write(//stories/*/story.md), Write(stories/*/story.md), Bash(ls:*), Bash(find:*), Bash(*)
model: claude-sonnet-4-5-20250929
disable-model-invocation: false
scripts:
  sh: .specify/scripts/bash/clarify-story.sh --json --paths-only
  ps: .specify/scripts/powershell/clarify-story.ps1 -Json -PathsOnly
---

Focus Area (optional): $ARGUMENTS
---

## Objective

To detect and reduce ambiguities or missing decision points in the story outline, gather clarifying information through interactive Q&A, and record the results in the story file.

**Note**: This clarification process should be run and completed before `/plan`. If the user explicitly skips clarification (e.g., for exploratory writing), you may proceed, but you must warn that the risk of downstream rework will increase.

## Execution Steps

### 1. Initialization Check

Run `{SCRIPT}` to get the current story path:
- Parse the JSON to get `STORY_PATH` and `STORY_NAME`.
- If no story file is found, prompt the user to run `/story` first to create a story outline.
- Load the story file content for analysis.

<!-- PLUGIN_HOOK: genre-knowledge-clarify -->
<!-- Plugin Enhancement Area: Genre Recognition
     If you have the genre-knowledge plugin installed, insert the genre recognition enhancement prompt here.
     Reference: "2.1 Enhance the /clarify command" section in plugins/genre-knowledge/README.md
-->

### 2. Structured Ambiguity Scan

Perform a comprehensive scan of the story outline, assessing the clarity of each category (Clear / Partially Clear / Missing):

**Creative Positioning**
- Target audience (age range, gender preference, reading level)
- Work positioning (commercial "cool" fiction / serious literature / genre fiction)
- Expected scale (short story 30-50k words / novella 100-200k / long-form 500k+)

**World-building Setting**
- Precision of the historical background (specific year/dynasty/degree of fictionalization)
- World rules (magic system / technology level / social system)
- Geographical scope (single city / multiple countries / continent / interstellar)

**Character Design**
- Protagonist's growth curve (from zero to hero / genius type / steady progress)
- Protagonist's personality tone (passionate / calm / cunning / saint-like)
- Supporting characters' functional roles (plot driver / emotional support / foil)
- Villain's intelligence setting (dumbed-down villain / evenly matched / superior intellect)

**Narrative Strategy**
- Point of view (first person / third-person limited / omniscient)
- Timeline structure (linear narrative / flashbacks and flashforwards / multiple parallel lines)
- Narrative pacing (fast-paced "cool" fiction / slow-burn setup / varied tempo)

**Plot Core**
- Core conflict type (man vs. man / man vs. nature / man vs. society / man vs. self)
- Clarity of the main goal (revenge / growth / rescue / exploration)
- Ending tendency (happy ending / tragedy / open-ended)

**Style and Tone**
- Writing style choice (vernacular and smooth / classic and elegant / humorous / gritty and realistic)
- Descriptive focus (action scenes / psychological description / atmosphere / dialogue-driven)
- Emotional tone (passionate and exciting / oppressive and dark / warm and healing / heart-wrenching)

**Creative Constraints**
- Handling of sensitive content (level of violence / emotional scale)
- Value orientation (positive energy / realism / critical)
- Update schedule (daily / weekly / monthly)

Generate candidate questions for each "Partially Clear" or "Missing" category, unless:
- Clarification would not substantially affect the creative direction.
- The information is more suitable to be determined during the chapter planning stage.

### 3. Generate a Prioritized Question Queue

Internally generate a maximum of 5 prioritized clarification questions, applying the following constraints:
- A maximum of 5 questions for the entire session.
- Each question must be answerable in one of the following ways:
  * Multiple choice (2-5 mutually exclusive options)
  * Short answer (limited to 5 words or less)
- Only include questions that have a substantial impact on the creative direction.
- Ensure balanced category coverage, prioritizing high-impact areas.
- If more than 5 categories need clarification, select the 5 with the highest (impact × uncertainty).

### 3.5 Question Design Principles (Conversational Understanding)

Each question should sound **like a real writer having a conversation**, not an engineer filling out a form.

**Core Principles**:
- ✅ **State the observation first, then ask the question**: Let the author understand "why you're asking this."
- ✅ **Use plain language**: Use the creator's language, not academic jargon.
- ✅ **Point out the impact**: Let the author understand what this decision will affect.
- ❌ **Avoid asking questions directly**: Don't just start with "Who is your target audience?"

**Comparison Example**:

❌ **Engineering-style Question** (Avoid):
```
Question 1: What is the age range of your target audience?
A. 18-25  B. 26-35  C. 36-45
```

✅ **Conversational Question** (Recommended):
```
💬 I noticed your story has campus elements, but also workplace content. The reader groups for these two scenes are very different—
Campus readers like passionate growth stories, while workplace readers are more interested in power struggles and strategy. This will directly affect our pacing design and value expression.

So, I'd like to confirm: **Who are you primarily writing for?**

| Option | Description |
|---|---|
| A | Student group (18-25 years) - focusing on growth and idealism |
| B | Working professionals (26-35 years) - focusing on realism and strategic thinking |
| C | All-encompassing (adjust to a dual-narrative, catering to both) |
| D | Custom (please enter your idea) |
```

**Question Structure Template**:
```markdown
💬 [Observed phenomenon/contradiction]. [How this will affect a creative decision].

So, I'd like to confirm: **[Core Question]**

[Options table or short answer prompt]
```

### 4. Sequential Q&A Loop

**Present one question at a time**, following the conversational format.

Multiple Choice Format (must include question context):
```markdown
💬 [Question context: What did you observe? What will this affect?]

So, I'd like to confirm: **[Core Question]**

| Option | Description |
|---|---|
| A | Detailed description of option A |
| B | Detailed description of option B |
| C | Detailed description of option C |
| D | Detailed description of option D |
| E | Detailed description of option E (optional) |
| F | Custom (please enter your idea) |
```

Short Answer Format (must include question context):
```markdown
💬 [Question context: What did you observe? What will this affect?]

So, I'd like to confirm: **[Core Question]**

Please provide a brief answer (5 words or less): _______
```

**Handling User Responses**
- Validate the answer's effectiveness.
- If F (Custom) is chosen, accept and record the user's custom input.
- If there's ambiguity, request a quick clarification.
- Record the answer and proceed to the next question.

**Stopping Conditions**
- All key ambiguities have been resolved.
- The user indicates completion ("okay," "that's enough," "no more").
- The 5-question limit has been reached.

### 5. Consolidate Clarification Results

Immediately after each accepted answer:

**On first consolidation**
- Create a `## Clarification Log` section in the story outline (if it doesn't exist).
- Add a `### Clarification Session [Date]` subsection.

**Record Format**
```markdown
- Q: [Question content] → A: [User's answer]
```

**Update Relevant Sections**
Update the corresponding parts of the story outline based on the clarified content:
- Creative Positioning → Update story overview
- World-building → Update world-building setting
- Characters → Update character settings
- Narrative Strategy → Add to creative notes
- Style and Tone → Add to style guide

### 6. Validate and Save

After each update, validate:
- Completeness of the clarification log.
- No legacy ambiguity markers have been resolved by new answers.
- No contradictory statements.
- Correct Markdown formatting.

Write the updated content back to the story file.

### 7. Completion Report

The report includes:
- The number of questions asked and answered.
- The path of the updated story file.
- A list of the sections touched.
- A coverage summary table:

| Category | Status |
|---|---|
| Creative Positioning | ✅ Clarified |
| World-building Setting | ✅ Clarified |
| Character Design | ⏸ Deferred to planning |
| ... | ... |

- Suggested next command (usually `/plan`).

## Behavioral Rules

- If no significant ambiguities are found, respond with: "No key ambiguities detected that require immediate clarification."
- If the story file is missing, instruct the user to run `/story` first.
- Do not exceed the total limit of 5 questions.
- Avoid asking purely technical writing details.
- Respect the user's signal to terminate early.
- If full coverage is achieved without asking questions, output a concise coverage summary.

## Novel Writing-Specific Considerations

- **Genre Adaptation**: Load a corresponding knowledge base based on the story's genre (e.g., "cool" fiction/suspense/romance/serious literature) to provide targeted questions.
- **Reader Orientation**: Different standards and focuses for commercial vs. literary works.
- **Cultural Sensitivity**: Certain themes require particularly careful handling.
- **Series Planning**: Whether it's a series will affect the overall architecture and decisions.

Priority Context: {ARGS}
