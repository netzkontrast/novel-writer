---
description: Execute chapter writing based on the task list, automatically loading context and validation rules.
argument-hint: [Chapter number or Task ID]
allowed-tools: Read(//**), Write(//stories/**/content/**), Bash(ls:*), Bash(find:*), Bash(wc:*), Bash(grep:*), Bash(*)
model: claude-sonnet-4-5-20250929
scripts:
  sh: .specify/scripts/bash/check-writing-state.sh
  ps: .specify/scripts/powershell/check-writing-state.ps1
---

Execute chapter writing based on the seven-step methodology process.
---

## Pre-check

1. Run the script `{SCRIPT}` to check the creation status.

### Query Protocol (Mandatory Reading Order)

⚠️ **Important**: Please strictly follow the order below to query documents, ensuring complete context and correct priority.

**Query Order**:
1.  **Query First (Highest Priority)**:
    -   `.specify/memory/constitution.md` (Creative Constitution - a supreme principle)
    -   `.specify/memory/style-reference.md` (Style Reference - if generated via `/book-internalize`)

2.  **Then Query (Specifications and Plans)**:
    -   `stories/*/specification.md` (Story Specification)
    -   `stories/*/creative-plan.md` (Creative Plan)
    -   `stories/*/tasks.md` (Current Task)

3.  **Then Query (Status and Data)**:
    -   `spec/tracking/character-state.json` (Character Status)
    -   `spec/tracking/relationships.json` (Relationship Network)
    -   `spec/tracking/plot-tracker.json` (Plot Tracker - if any)
    -   `spec/tracking/validation-rules.json` (Validation Rules - if any)

4.  **Then Query (Knowledge Base)**:
    -   `spec/knowledge/` related files (worldview, character profiles, etc.)
    -   `stories/*/content/` (Previous content - to understand the context)

5.  **Then Query (Writing Guidelines)**:
    -   `.specify/memory/personal-voice.md` (Personal Corpus - if any)
    -   `spec/knowledge/natural-expression.md` (Natural Expression - if any)
    -   `spec/knowledge/punctuation-personality.md` (Punctuation Personalization - if any)
    -   `spec/knowledge/detail-formulas.md` (Concretization Formulas - if any)
    -   `spec/presets/anti-ai-detection.md` (Anti-AI Detection Guidelines)

6.  **Conditional Query (For the first three chapters only)**:
    -   **If chapter number ≤ 3 or total word count < 10,000**, additionally query:
        -   `spec/presets/golden-opening.md` (Golden Opening Rules)
        -   And strictly follow its five golden rules.

<!-- PLUGIN_HOOK: genre-knowledge-write -->
<!-- Plugin Enhancement Area: Style Application
     If you have the genre-knowledge plugin installed, insert the style application enhancement prompt here.
     Reference: "2.3 Enhance the /write command" section in plugins/genre-knowledge/README.md
-->

## Writing Execution Flow

### 1. Select Writing Task
Select a writing task with the status `pending` from `tasks.md` and mark it as `in_progress`.

### 2. Validate Prerequisites
- Check if related dependent tasks are completed.
- Verify that the necessary settings are ready.
- Confirm that the preceding chapters are finished.

### 3. Pre-writing Reminders
**Reminders based on Constitutional Principles**:
- Key points of core values
- Quality standard requirements
- Style consistency guidelines

**Reminders based on Specification Requirements**:
- P0 must-include elements
- Target reader characteristics
- Content red line reminders

**Paragraph Formatting Guidelines (Important)**:
- ⛔ **Do Not Use**: Numbered markers like "One," "Two," "Three" to separate paragraphs.
- ✅ **Usage**: Use two blank lines (one empty line) for scene transitions.
- 📖 **Reason**: Numbered markers are too rigid, break reading immersion, and do not conform to web novel habits.

**Anti-AI Detection Writing Guidelines (Based on Tencent's Zhuque standard)**:

⚠️ **Important Context**: AI programming tools use low-temperature parameters, but traditional "compensation methods" (forcing the piling up of details) lead to excessive description, which in turn increases AI characteristics. The following guidelines are based on tested standards for passing (0% AI concentration).

### 📏 Paragraph Structure Guidelines (Key) ⭐

**Single-Sentence Paragraph Ratio**:
- ✅ **30%-50% of paragraphs should be single-sentence paragraphs.**
- ✅ **Keep each paragraph between 50-100 words.**
- ✅ **Place key information in a separate paragraph.**

**Example Comparison**:

❌ **AI-style Writing** (Excessive description, 95% AI concentration):
> A musty smell permeated the room, the only light source being the grayish-white moonlight filtering through a gap in the curtains. He fumbled his way forward along the wall, his fingertips touching the cold stone, until his knee hit the corner of a table—a rickety wooden table covered in dust.

✅ **Natural Writing** (Concise and restrained, 0% AI concentration):
> After the Yongjia Disturbance, the Central Plains were occupied by foreign tribes.
>
> Most of the Han gentry and commoners, except for a few who were unwilling to leave their homes, fled south across the river.
>
> In recent years, Wang Qiao had recruited hundreds of refugees to farm for him.

### 🚫 Prohibited Items Checklist (Anti-AI Phrasing)

1.  **Prohibit Meaningless Piling**
    -   ❌ Don't force the inclusion of "3 sensory details."
    -   ❌ Don't use a list-like description of emotions.
    -   ✅ One accurate detail is better than three piled-up ones.

2.  **Prohibit Ornate Metaphors**
    -   ❌ "a rickety wooden table," "the air froze."
    -   ✅ Direct description: "an old wooden table," "silence."

3.  **Prohibit Over-dramatization**
    -   ❌ "Before the words were out of her mouth, she had already turned to leave. He rushed up and grabbed..."
    -   ✅ Concise handling: "She turned and left. He chased after her."

4.  **Prohibit Explanatory Dialogue**
    -   ❌ "I'm angry because you didn't come yesterday."
    -   ✅ "Where were you yesterday?" "...None of your business."

5.  **Prohibit Direct Psychological Descriptions**
    -   ❌ "He thought to himself, this is not simple."
    -   ✅ Imply through action: "His brow furrowed."

### ✅ Natural Writing Principles

**1. Historical Plain Description** (Applicable to ancient settings)
- State facts without embellishment.
- Example: "In recent years, Wang Qiao had recruited hundreds of refugees to farm for him."

**2. Colloquial Handling** (Dialogue)
- Include grammatical errors, pauses, repetitions.
- Example: "Most'a people went south" (instead of "Most people").

**3. Short Sentence Rhythm** (Narration)
- Single sentences of 15-25 words.
- Key information in a separate paragraph.

**4. Restrained Description** (Scenes)
- 1-2 details per scene is sufficient.
- ❌ Don't write: "A musty smell permeated the room, the walls were cold, the light was dim..."
- ✅ Instead, write: "The room was dark." (Sufficient).

### 📊 Self-Check Standards

After writing a paragraph, check:
- [ ] Is the single-sentence paragraph ratio between 30%-50%?
- [ ] Is the word count of each paragraph between 50-100 words?
- [ ] Are there high-frequency AI words like "the only," "until," "permeated"?
- [ ] Are sensory details being forced?
- [ ] Is the dialogue too perfect (lacking pauses, grammatical errors)?
- [ ] Are the metaphors too ornate?

**High-Frequency AI Word Blacklist**:
- "the only," "until," "permeated with," "rickety"
- "the air froze," "before the words were out," "suddenly"
- "couldn't help but," "immediately," "thought to himself"
- "frowned," "sighed"

**Replacement Strategy**:
| ❌ AI Vocabulary | ✅ Natural Replacement |
|---|---|
| permeated with a musty smell | had a musty smell |
| the only light source | there was only a little light |
| a rickety wooden table | an old wooden table |
| he thought to himself | he thought / delete |
| before the words were out | he hadn't finished speaking / delete |

### 4. Real-time Assistance Mode (Optional)

**If the user encounters difficulties during the writing process**, for example:
- "Help me think about what the protagonist should do."
- "How should the plot develop from here?"
- "Give me a few options."

**You can proactively provide 2-3 action options**, for example:

> **Plot Development Suggestions**:
>
> **Option A (Proactive)**: The protagonist takes direct action, using their special ability to crush the opponent.
> - Pros: Direct thrill, high reader satisfaction.
> - Cons: May make the protagonist seem too powerful.
>
> **Option B (Strategic)**: The protagonist hides their strength and outsmarts the opponent.
> - Pros: Showcases the protagonist's intelligence, adds suspense.
> - Cons: The pacing might be slightly slower.
>
> **Option C (Unexpected)**: Introduce a new variable that interrupts the current conflict.
> - Pros: Adds complexity, introduces new plotlines.
> - Cons: May make the reader feel interrupted.

**Then, based on the user's choice**, continue creating the content.

⚠️ **Note**: This is an assistance mode. Do not proactively provide options unless the user explicitly requests help.

---

### 5. Create Content According to the Plan:
   - **Opening**: Attract the reader, connect with the previous text.
   - **Development**: Advance the plot, deepen the characters.
   - **Turning Point**: Create conflict or suspense.
   - **Closing**: Conclude appropriately, lead into the next text.

### 6. Quality Self-Check

**Constitution Compliance Check**:
- Does it align with the core values?
- Does it meet the quality standards?
- Does it maintain style consistency?

**Specification Compliance Check**:
- Does it include the necessary elements?
- Does it align with the target positioning?
- Does it adhere to the constraints?

**Plan Execution Check**:
- Does it follow the chapter architecture?
- Does it conform to the pacing design?
- Does it meet the word count requirement?

**Format Guideline Check**:
- ⚠️ Confirm that numbered markers like "One," "Two," "Three" are not used for paragraphs.
- ✅ Use two blank lines (one empty line) for scene transitions.
- ✅ Maintain a natural and smooth paragraph spacing.

### 📊 Concretization Checklist (Key to De-AI-ifying) ⭐

After writing a paragraph, proactively identify and replace abstract expressions:

#### 🔍 Identify Abstract Expressions

**Time Abstraction** ❌ → **Concretization** ✅
- "recently" → "last Wednesday afternoon"
- "a long time ago" → "three years ago in the autumn"
- "not long ago" → "yesterday at 8 a.m."
- "after a long time" → "waited for two full hours"

**Character Abstraction** ❌ → **Concretization** ✅
- "many people" → "at least 5 of my friends"
- "someone said" → "Uncle Li told me" / "Old Wang next door mentioned"
- "everyone knows" → "the old folks in the village all say"
- "it is said" → "I heard from Uncle Wang in private"

**Quantity Abstraction** ❌ → **Concretization** ✅
- "the effect was good" → "we harvested three more bushels of grain this time" / "twice as many customers as usual"
- "very expensive" → "the meal cost three hundred dollars"
- "very far" → "it's a two-hour drive"
- "a lot" → "at least twenty"

**Scene Abstraction** ❌ → **Concretization** ✅
- "the room was messy" → "a pile of unwashed clothes from three days ago on the floor"
- "the weather was cold" → "you could see your breath"
- "very tired" → "walked for five full hours on a mountain path"
- "the atmosphere was tense" → "no one spoke, only the ticking of the clock could be heard"

#### 💡 Proactive Search Suggestions

**When encountering the following situations, consider using WebSearch to get real details**:
- Historical events: search for real dates, people, places.
- Technical details: search for actual parameters, professional terms.
- Geographical information: search for real place names, distances, landmarks.
- Cultural customs: search for local dialects, customs, specialties.
- Data support: search for real statistics, cases, news.

**Search Formulas**:
```
- "ancient Chinese [dynasty] official system"
- "[city name] characteristic dialect words"
- "[year] real historical events"
- "[industry] professional terminology glossary"
```

#### ✅ Concretization Self-Check Questions

- [ ] Is the time specific? (Avoid "recently," "a long time ago")
- [ ] Is the source of the character clear? (Avoid "someone," "everyone")
- [ ] Is the quantity precise? (Avoid "many," "a lot")
- [ ] Are the scene details visible? (Avoid adjectives like "very xx")
- [ ] Are real place names/personal names/data used?
- [ ] Does the dialogue have specific content? (Avoid "he said a lot")

#### 📌 Concretization Notes

**Principle of Moderation**:
- ✅ Key plot points must be concrete: turning points, climaxes, foreshadowing.
- ✅ Important details must be concrete: first impressions, key props.
- ⚠️ Minor information can be summarized: transitional paragraphs, background setup.
- ❌ Avoid excessive concretization: tedious lists, wordiness.

**Scene Adaptation**:
- Ancient settings: historical plain description, moderately concrete.
- Modern settings: life details, highly concrete.
- Fantasy settings: world-building settings, moderately concrete.

**Example Comparison**:

❌ **Abstract Version** (AI-like):
```
A lot has been happening in the city recently, and everyone is talking about it. Wang Qiang was worried when he heard and decided to go see the situation.
```

✅ **Concrete Version** (Realistic):
```
Starting last Wednesday, Auntie Li at the market has been saying something happened on East Street.

After listening for two days, Wang Qiang couldn't hold back anymore: "What exactly happened?"

"Someone died!" Auntie Li lowered her voice, "I heard it was Old Zhang who runs the supermarket..."

Wang Qiang's heart tightened. He knew Old Zhang, he had just bought rice from him last month.

He decided to go over in the afternoon to see.
```

**Concretization Effect Comparison**:
- Time: recently → last Wednesday
- Place: in the city → East Street, the market
- Character: everyone → Auntie Li, Old Zhang
- Event: a lot has been happening → someone died, who runs the supermarket
- Detail: heard → lowered her voice, bought rice from him last month

### 7. Save and Update
- Save the chapter content to `stories/*/content/`.
- Update the task status to `completed`.
- Record the completion time and word count.

## Key Writing Points

- **Follow the Constitution**: Always adhere to the creative principles.
- **Meet the Specifications**: Ensure all necessary elements are included.
- **Execute the Plan**: Proceed according to the technical plan.
- **Complete the Tasks**: Systematically work through the task list.
- **Continuous Validation**: Periodically run `/analyze` to check.

## Post-completion Actions

### 8. Validate Word Count and Update Progress

**Word Count Explanation**:
- Use an accurate method for counting Chinese characters.
- Exclude Markdown markers (`#`, `*`, `-`, etc.).
- Only count actual content characters.
- Word count requirements come from `spec/tracking/validation-rules.json` (default 2000-4000 words).

**Validation Method**:
Use the project's provided word count script to validate the chapter's word count:
```bash
source scripts/bash/common.sh
count_chinese_words "stories/*/content/ChapterX.md"
```

⚠️ **Note**: Do not use `wc -w` to count Chinese characters, as it is highly inaccurate for Chinese!

**Completion Report**:
```
✅ Chapter writing complete.
- Saved to: stories/*/content/ChapterX.md
- Actual word count: [X] words
- Word count requirement: 2000-4000 words
- Word count status: ✅ Meets requirement / ⚠️ Insufficient words / ⚠️ Exceeds word count
- Task status: Updated
```

### 9. Suggest Next Steps
- Continue with the next writing task.
- Run `/analyze` every 5 chapters for a quality check.
- Adjust the plan promptly if issues are found.

## Relationship with the Methodology

```
/constitution → Provides creative principles
     ↓
/specify → Defines story requirements
     ↓
/clarify → Clarifies key decisions
     ↓
/plan → Develops the technical plan
     ↓
/tasks → Decomposes into execution tasks
     ↓
/write → 【Current】Executes writing
     ↓
/analyze → Validates quality and consistency
```

Remember: Writing is the execution layer; it must strictly follow the specifications and plans from the upper layers.
