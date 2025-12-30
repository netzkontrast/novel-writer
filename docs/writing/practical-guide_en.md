# Practical Guide to Novel Writing: Applying the SDD Methodology

## Core Concept: The Essence of Specification-Driven Development (SDD)

### What is Specification-Driven Development?

**SDD (Specification-Driven Development)**, originating from software engineering, is based on the core idea:

> **First define "what is needed" (Spec), then decide "how to do it" (Implementation)**

Traditional Novel Writing:
```
Inspiration → Start writing directly → See where it goes → Discover problems → Major revisions/Abandon project
```

SDD-Driven Writing:
```
Inspiration → Specify → Plan → Decompose into tasks → Implement (write) → Analyze
  ↑                                                                              ↓
  └────────────────── Discover problems, return to the appropriate level to modify the spec ──────────────────┘
```

### Key Insight of SDD: It's Not a One-Time Process, but a Recursive Cycle

**❌ Misunderstanding: Linear Process**
```
Entire Book:
constitution → specify → clarify → plan → tasks → write(300 chapters) → analyze
                                                     ↑
                                              Looping here forever
```

**✅ Correct Understanding: Hierarchical Recursion**
```
Level 1 - Entire Book:   specify → plan → tasks
           ↓
Level 2 - A Volume:     specify → plan → tasks → write
           ↓
Level 3 - A Chapter:     plan → write → analyze
           ↓
Level 4 - A Paragraph:     write
```

**Each level is a complete SDD cycle!**

![SDD Hierarchical Recursion Diagram](images/sdd-levels.svg)

---

## Detailed Explanation of the Seven-Step Method

![Complete SDD Flow](images/sdd-flow.svg)

### The Complete Seven-Step Process

```
1. /constitution  →  Writing Constitution (Highest principles, not easily changed)
2. /specify       →  Story Specification (Single source of truth)
3. /clarify       →  Clarify Decisions (Eliminate ambiguity)
4. /plan          →  Creative Plan (Technical solution)
5. /tasks         →  Task Decomposition (Executable steps)
6. /write         →  Execute Writing (Implementation)
7. /analyze       →  Comprehensive Validation (Quality assurance)
```

### Namespace Explanation

> 📝 **Important**: Different AI platforms use different command formats.

Novel Writer supports 13 AI platforms. According to the multi-platform optimization in v0.15.0, command formats are divided into three categories:

| AI Platform | Command Format | Example |
|---|---|---|
| **Claude Code** | `/novel.command_name` | `/novel.constitution`, `/novel.write` |
| **Gemini CLI** | `/novel/command_name` | `/novel/constitution`, `/novel/write` |
| **Other Platforms** | `/command_name` | `/constitution`, `/write` |

**Other platforms include**: Cursor, Windsurf, Roo Code, GitHub Copilot, Qwen Code, OpenCode, Codex CLI, Kilo Code, Auggie CLI, CodeBuddy, Amazon Q Developer

**This document's convention**:
- For brevity, examples use the **generic format** `/command_name`.
- Please add the appropriate prefix based on your AI platform when using them.
- For example: `/write` should be entered as `/novel.write` in Claude, and `/novel/write` in Gemini.

### How to Apply the Seven-Step Method at Each Level

| Level | Specify | Clarify | Plan | Tasks | Write | Analyze |
|---|---|---|---|---|---|---|
| **Entire Book** | ✅ Complete | ✅ 5 questions | ✅ 300 chapters | ✅ 47 tasks | - | ✅ After finishing |
| **A Volume** | ✅ Volume spec | ✅ Volume decisions | ✅ 75 chapters | ✅ Volume tasks | ✅ Write chapters | ✅ End of volume |
| **Chapter Section** | ✅ Paragraph spec | - | ✅ 10 chapters | ✅ 10 tasks | ✅ Write | ✅ End of section |
| **Single Chapter** | - | - | ✅ Chapter outline | - | ✅ Write | ✅ After writing |

---

## Example: The SDD Practice of "Back to 1984"

### Project Background

- **Story**: 18-year-old Chen Rui travels from 2025 back to 1984 in Longxi County, Fujian.
- **Goal**: 1.2 million words, 300 chapters, 4 volumes.
- **Status**: 7 chapters written, about 28,000 words.
- **Genre**: Historical time travel + period drama + harem.

---

## Scenario 1: Book-Level SDD (Project Initialization)

### Step 1: Constitution (Writing Constitution)

```bash
/constitution
```

**Definition**: The highest principles for "Back to 1984"

```markdown
## Core Principles

### Principle 1: Balance Between Historical Authenticity and Creative Freedom
- The historical background of 1984 must be accurate.
- Prices, wages, and policies must not be wrong.
- The protagonist's actions can change personal destinies, but not national-level history easily.

### Principle 2: Story-Driven, Rejecting Gimmicky Time Travel
- Time travel is a means, not an end.
- The advantage of foresight is limited (remembers major trends, details are fuzzy).
- Every use of future knowledge must face difficulties and costs.

### Principle 3: Three-Dimensional Characters, Rejecting Cardboard Cutouts
- Every character that appears has an independent motivation.
- Supporting characters do not just serve the protagonist.
- Villains have logical reasons for their actions.

### Principle 4: Authentic Emotions, Avoiding Superficiality
- Love must have a development process.
- Friendship and family ties are equally important.
- Emotional conflicts come from the differences between characters themselves.

### Principle 5: Conveying Values, Rejecting Preaching
- Convey values naturally through the story.
- Allow characters to make mistakes, struggle, and grow.
- Do not shy away from the limitations of the era and the complexity of human nature.
```

---

### Step 2: Specify (Story Specification)

```bash
/specify
```

**Definition**: What the entire book will be about.

```markdown
## One-Sentence Story
A growth story of an 18-year-old mountain boy, Chen Rui, who accidentally travels back to 1984 and uses his future memories to change his and his family's destiny.

## Target Positioning
- Word count: 1-1.5 million words
- Chapters: 300 chapters
- Readers: 18-40 years old, likes period dramas and time travel novels.
- Genre: Historical time travel + period drama + harem.

## Core Setting
- Protagonist: Chen Rui, 18 years old, travels from 2025 to 1984.
- Location: Longxi County, Fujian (fictional, to reduce research pressure).
- Time: July 1984 - 1990s.
- Foresight Limitation: Only remembers major events and trends, details are fuzzy.

## Main Relationship Lines
- Family line: Parents, younger brother.
- Friendship line: Classmates, business partners.
- Love line: Childhood sweetheart Lin Wan + university classmate Jiang Chenxi + others.
- Career line: College entrance exam → First pot of gold → Studying and doing business in university → Starting a business.

## Success Criteria
- Logical consistency: No obvious loopholes.
- Character depth: Protagonist has 5 personality layers, main supporting characters have 3 layers.
- Plot tightness: Each chapter advances the main plot or important subplots.
```

---

### Step 3: Clarify (Key Clarifications)

```bash
/clarify
```

**AI identifies ambiguous points and asks 5 key questions**:

```
Q1: What is the protagonist's attitude towards changing history?
   A) Cautious, tries not to change major history.
   B) Radical, wants to change major events.
   C) Casual, takes it one step at a time.
→ Choice: A (conforms to Principle 1)

Q2: How to handle the multi-angle relationships?
   A) Love triangle conflict type.
   B) Multi-line parallel harem type.
   C) Sequential development in order.
→ Choice: B (harem positioning)

Q3: What is the focus of the business line?
   A) Stock market speculation.
   B) Real estate investment.
   C) Industrial entrepreneurship.
→ Choice: C (more dramatic)

Q4: Where does the first pot of gold come from?
   A) Selling cassettes on the street.
   B) Reselling scarce goods.
   C) Using information gaps for trade.
→ Choice: C (more in line with the protagonist's intelligent character)

Q5: What is the final scale of the story?
   A) A small fortune in the county.
   B) A provincial business empire.
   C) A national-level entrepreneur.
→ Choice: B (fits the 1.2 million word count)
```

**The clarification results will be automatically updated to `specification.md`**

---

### Step 4: Plan (Creative Plan)

```bash
/plan
```

**Based on the clarified specification, create a technical plan**:

```markdown
## Writing Method: Hero's Journey + Three-Act Structure

### Three-Act Structure
- Act One (Chapters 1-75): Time travel + Adaptation + College entrance exam [Setup]
- Act Two (Chapters 76-225): University + Entrepreneurship + Relationships + Crisis [Confrontation]
- Act Three (Chapters 226-300): Climax + Choice + Resolution [Resolution]

### Detailed Plan for Volume One (Chapters 1-75)
#### Part One: Time Travel and Adaptation (Chapters 1-10)
- Chapter 1: The moment of time travel [Completed ✓]
- Chapter 2: The unfamiliar home [Completed ✓]
- Chapter 3: Longxi County in 1984 [Completed ✓]
- ...

#### Part Two: Determination and Action (Chapters 11-25)
- Chapter 11: Deciding to change destiny.
- Chapter 12: Testing "foresight" memory.
- Chapter 13: First attempt at making money.
- ...

#### Part Three: Exam Preparation and Growth (Chapters 26-50)
- Chapter 26: Entering exam preparation mode.
- Chapter 30: First pot of gold (mid-term climax).
- Chapter 40: Major family event (turning point).
- ...

#### Part Four: College Entrance Exam and Farewell (Chapters 51-75)
- Chapter 51: The final sprint.
- Chapter 70: The college entrance exam (tense).
- Chapter 75: Leaving Longxi (end-of-volume climax).
```

---

### Step 5: Tasks (Task Decomposition)

```bash
/tasks
```

**Decompose the plan into executable tasks**:

```markdown
## Preparation Tasks [P0 - Must be completed first]

- [ ] T001 - Create protagonist profile: Chen Rui [2 hours]
  Output: characters/chen-rui.md

- [ ] T002 - Create female character profile: Lin Wan [1.5 hours]
  Output: characters/lin-wan.md

- [ ] T003 - Create female character profile: Jiang Chenxi [1.5 hours]
  Output: characters/jiang-chenxi.md

- [ ] T004 - Create family profiles [1 hour]
  Output: characters/family.md

- [ ] T005 - Create Longxi County setting [3 hours]
  Output: worldbuilding/longxi-county.md

- [ ] T006 - Create 1984 database [4 hours]
  Output: worldbuilding/1984-database.md

- [ ] T007 - Create detailed outline for the first 50 chapters [6 hours]
  Output: outline/chapters-001-050.md

## Writing Tasks [P1 - Normal progress]

- [x] W001 - Chapter 1: The moment of time travel [4000 words] ✓
- [x] W002 - Chapter 2: The unfamiliar home [4000 words] ✓
- [x] W003 - Chapter 3: Longxi County in 1984 [4000 words] ✓
- [x] W004 - Chapter 4: Fragments of memory [4000 words] ✓
- [x] W005 - Chapter 5: The shock of the supply and marketing cooperative [4000 words] ✓
- [x] W006 - Chapter 6: Classmate gathering [4000 words] ✓
- [x] W007 - Chapter 7: Parents' expectations [4000 words] ✓
- [ ] W008 - Chapter 8: Testing memory [4000 words]
- [ ] W009 - Chapter 9: The first attempt [4000 words]
- [ ] W010 - Chapter 10: An unexpected opportunity [4000 words]
```

---

### Step 6: Write (Execute Writing)

```bash
/write
```

**The AI will**:
1. Read `constitution.md` (writing principles)
2. Read `specification.md` (story specification)
3. Read `creative-plan.md` (creative plan)
4. Read `tasks.md` (current task: W008)
5. Read `outline/chapters-001-050.md` (Chapter 8 outline)
6. Read the content of the first 7 chapters (context)
7. Read `validation-rules.json` (character validation rules)
8. Start writing Chapter 8

---

### Step 7: Analyze (Comprehensive Validation)

```bash
/track --check  # Execute every 5-10 chapters
```

**Validation Content**:
```
✅ Constitution compliance: Does it violate the 5 major principles?
✅ Specification satisfaction: Does it deviate from the specification?
✅ Plot consistency: Are the timeline and cause-and-effect chain reasonable?
✅ Character consistency: Are names, titles, and personalities consistent?
✅ Worldview consistency: Does anything appear that shouldn't exist in 1984?
```

---

## Scenario 2: Volume-Level SDD (Re-planning)

### Scenario Description

After finishing Chapter 75 (end of Volume One), you find that:
- ✅ Chapters 1-50 were written well according to the plan.
- ❌ When writing about the college entrance exam in chapters 51-75, the original plan of "selling cassettes on the street" was too thin.
- ✅ The actual story of "doing transportation business with veteran Lao Zhang" was better.
- 🤔 Should the transportation line continue in Volume Two (chapters 76-150)?

**This is a typical "volume-level specification change"**

---

### SDD Response: Re-execute the complete process for Volume Two

#### Step 1: Specify (Volume Two Specification)

```bash
/specify
```

**❌ Bad prompt** (too brief):
```
Volume Two will be about university and business.
```
→ Problem: No background, no specific direction, AI cannot understand the modification intent.

**✅ Good prompt** (structured):

![Good Prompt Structure](images/prompt-structure.svg)

```
📝 User Input Box:

I need to re-plan Volume Two (chapters 76-150).

【Background】
- Volume One was actually written as: Chen Rui made his first pot of gold by doing transportation business with Lao Zhang.
- End of Volume One: Chen Rui was admitted to Xiamen University and is about to enroll.
- Lao Zhang is willing to continue the cooperation, but needs Chen Rui's decision.

【Volume Two Specification Adjustment】
1. Main plot: University life + expansion of transportation business.
2. How does Chen Rui balance studies and business?
   → Focus on studies in the first year, participate in transportation during winter and summer vacations.
   → Start studying and doing business in the second year.
3. Development of the transportation line:
   → Expand from the Longxi-Xiamen line to multiple lines within the province.
   → Develop from freight to a prototype of express delivery.
4. New characters:
   → University roommates (3).
   → Jiang Chenxi (economics student, future partner).
   → Business rival (local transportation company in Xiamen).
5. Love line:
   → Lin Wan in Longxi (long-distance relationship).
   → Jiang Chenxi in Xiamen (gradually getting closer).

【Expected Output】
Please update the "Career Line" and "Volume Two" related parts of `specification.md`.
```

**💬 Complete dialogue flow**:

```
┌─────────────────────────────────┐
│ 👤 User Input                      │
└─────────────────────────────────┘
/specify

[The complete prompt above]

┌─────────────────────────────────┐
│ 🤖 AI Response                        │
└─────────────────────────────────┘
I have understood the new direction for Volume Two.
Let me update `specification.md` for you:

【Update Preview】
## Career Line Adjustment

### First Pot of Gold (Volume One)
- Method: Partnering with veteran Lao Zhang in freight transport.
- Time: Before the 1984 college entrance exam.
- Scale: Longxi County-Xiamen line, monthly income of about 1000 yuan.
- Capital accumulation: About 5000 yuan.

### University Stage (Volume Two)
- First semester of freshman year (chapters 76-90): Focus on studies, help Lao Zhang in Longxi during winter vacation.
- Second semester of freshman year (chapters 91-105): Participate during summer vacation, discover opportunities in express delivery.
...

Confirm update?

┌─────────────────────────────────┐
│ 👤 User Confirmation                      │
└─────────────────────────────────┘
Confirm

┌─────────────────────────────────┐
│ 🤖 AI Completion                        │
└─────────────────────────────────┘
✅ `specification.md` updated.
✅ "Career Line" section updated.
✅ "Volume Two" specification added.

Next step suggestion: /clarify (clarify key decisions for Volume Two)
```

**AI Output**: Updated `specification.md` snippet

```markdown
## Career Line Adjustment

### First Pot of Gold (Volume One)
- Method: Partnering with veteran Lao Zhang in freight transport.
- Time: Before the 1984 college entrance exam.
- Scale: Longxi County-Xiamen line, monthly income of about 1000 yuan.
- Capital accumulation: About 5000 yuan.

### University Stage (Volume Two)
- First semester of freshman year (chapters 76-90): Focus on studies, help Lao Zhang in Longxi during winter vacation.
- Second semester of freshman year (chapters 91-105): Participate during summer vacation, discover opportunities in express delivery.
- First semester of sophomore year (chapters 106-120): Study and do business, try out express delivery prototype.
- Second semester of sophomore year (chapters 121-135): Encounter business rivals, competition escalates.
- End of volume (chapters 136-150): Crisis and breakthrough.
```

---

#### Step 2: Clarify (Key Decisions for Volume Two)

```bash
/clarify
```

**AI asks**:

```
Q1: What is Chen Rui's identity positioning in university?
   A) Low-key student, business is secret.
   B) Semi-public, roommates know but not flaunted.
   C) High-profile and flashy, becomes a campus celebrity.
→ Suggested choice: B (fits the protagonist's personality + adds drama)

Q2: How does the long-distance relationship with Lin Wan go?
   A) Stable development, eventually get together.
   B) Crisis arises, but eventually reconcile.
   C) Gradually drift apart, making way for Jiang Chenxi.
→ Suggested choice: B (increases tension in the love line)

Q3: What is the final form of the transportation business?
   A) A regional freight company.
   B) A prototype of a provincial express delivery network.
   C) An inter-provincial logistics empire.
→ Suggested choice: B (fits the volume's length)

Q4: How to deal with business rivals?
   A) Positive competition, winning with intelligence.
   B) Malicious competition, involving criminal forces.
   C) Eventual cooperation, a strong alliance.
→ Suggested choice: A (conforms to Principle 2: Story-driven)

Q5: What is Jiang Chenxi's role?
   A) Purely a love interest, not involved in business.
   B) Dual identity of love interest and partner.
   C) Just a business partner.
→ Suggested choice: B (more enriching with intertwined lines)
```

---

#### Step 3: Plan (Creative Plan for Volume Two)

```bash
/plan
```

**❌ Bad prompt**:
```
/plan
Re-plan Volume Two.
```

**✅ Good prompt**:

```
📝 User Input Box:

Based on the updated Volume Two specification and clarification results, re-plan the detailed scheme for chapters 76-150.

【Input】
- Updated `specification.md` (Volume Two specification)
- The 5 decision results from `clarify`.

【Output Requirements】
1. Divide chapters 76-150 into 4 parts.
2. Each part should include:
   - Clear subheadings and chapter ranges.
   - Key events for every 5 chapters.
   - Marked climax and turning points.
3. Reflect:
   - The development of the transportation line from intra-province to a provincial network.
   - Chen Rui's growth from freshman to sophomore.
   - The parallel development of the dual love lines with Lin Wan (long-distance) and Jiang Chenxi (new).
   - The escalating competition with business rivals.

【Update Location】
Update the "Volume Two" section of `creative-plan.md`, maintaining continuity with Volume One.
```

**AI Output**: New "Volume Two" section in `creative-plan.md`

```markdown
## Volume Two: University Life (Chapters 76-150)

### Part One: Entering University (Chapters 76-95)

#### Chapters 76-80: Registration and Adaptation
- Chapter 76: Farewell to Longxi, heading to Xiamen.
- Chapter 77: University registration, first meeting with Jiang Chenxi.
- Chapter 78: The group of four in the dormitory.
- Chapter 79: Military training begins.
- Chapter 80: The first long-distance call with Lin Wan.

#### Chapters 81-90: First Semester of Freshman Year
- Chapter 81: Challenges of professional courses.
- Chapter 82: Jiang Chenxi's academic excellence.
- Chapter 85: Phone call before winter vacation (Lao Zhang's predicament).
- Chapter 88: Final exams.
- Chapter 90: Returning to Longxi for winter vacation, helping Lao Zhang solve problems.

#### Chapters 91-95: Winter Vacation in Longxi (Minor Climax)
- Chapter 91: Lao Zhang's crisis (goods detained).
- Chapter 92: Chen Rui uses "foresight" knowledge to solve it.
- Chapter 93: Reunion with Lin Wan.
- Chapter 94: Discovering the opportunity in express delivery.
- Chapter 95: Persuading Lao Zhang to transform.

### Part Two: Studying and Doing Business (Chapters 96-120)

#### Chapters 96-105: Second Semester of Freshman Year
- Chapter 96: Returning to school, mind not on studies.
- Chapter 98: Collaborating with Jiang Chenxi on a project.
- Chapter 100: Summer vacation plan (pilot express delivery in Longxi).
- Chapter 105: End of finals, heading back to Longxi.

#### Chapters 106-115: Summer Vacation Express Delivery Pilot
- Chapter 106: The prototype of express delivery is established.
- Chapter 108: Hiring the first batch of employees.
- Chapter 110: The first major order (turning point).
- Chapter 112: Encountering obstruction from a Xiamen transportation company.
- Chapter 115: Temporarily surviving, but the crisis is looming.

#### Chapters 116-120: First Semester of Sophomore Year
- Chapter 116: Returning to school, identity semi-public.
- Chapter 117: Roommates' curiosity and reactions.
- Chapter 118: Jiang Chenxi proposes cooperation.
- Chapter 119: Chen Rui's dilemma (whether to bring her in).
- Chapter 120: Deciding to cooperate (turning point).

### Part Three: Escalating Competition (Chapters 121-135)

#### Chapters 121-130: Second Semester of Sophomore Year
- Chapter 121: Jiang Chenxi buys in.
- Chapter 123: Business expansion.
- Chapter 125: Malicious competition from the Xiamen company.
- Chapter 127: The incident of goods being smashed.
- Chapter 130: Reporting to the police and maneuvering.

#### Chapters 131-135: Summer Vacation Crisis
- Chapter 131: Returning to Longxi to find Lao Zhang beaten up.
- Chapter 132: Investigating the mastermind.
- Chapter 133: Negotiating with the boss of the Xiamen company.
- Chapter 134: Outsmarting the rival.
- Chapter 135: The crisis is temporarily resolved, but at a heavy cost.

### Part Four: Breakthrough and Choice (Chapters 136-150)

#### Chapters 136-145: Before the End of Sophomore Year
- Chapter 136: Reflection and adjustment.
- Chapter 138: Introducing a new strategy (franchise model).
- Chapter 140: Business breakthrough.
- Chapter 143: Lin Wan comes to Xiamen (emotional crisis).
- Chapter 145: The subtle relationship between the three.

#### Chapters 146-150: Climax of Volume Two
- Chapter 146: The Xiamen company makes a comeback.
- Chapter 147: The final game.
- Chapter 148: Winning with intelligence.
- Chapter 149: Phased victory of Volume Two.
- Chapter 150: End of sophomore year, looking forward to junior year (open ending).
```

---

#### Step 4: Tasks (Volume Two Tasks)

```bash
/tasks
```

**Input**: "Generate the task list for chapters 76-150 based on the new plan for Volume Two."

**AI Output**: `tasks.md` update

```markdown
## Volume Two Writing Tasks [P1]

### Character Supplement Tasks
- [ ] T008 - Create a detailed profile for Lao Zhang [1.5 hours]
- [ ] T009 - Create a detailed profile for Jiang Chenxi [2 hours]
- [ ] T010 - Create a group profile for the roommates [1.5 hours]
- [ ] T011 - Create a profile for the business rival [1 hour]

### Worldview Supplement Tasks
- [ ] T012 - Create the setting for Xiamen University [2 hours]
- [ ] T013 - Create a database for 1985-1986 [3 hours]
- [ ] T014 - Create background for the transportation/express delivery industry in the 80s [2 hours]

### Outline Tasks
- [ ] T015 - Create a detailed outline for chapters 76-100 [4 hours]
- [ ] T016 - Create a detailed outline for chapters 101-125 [4 hours]
- [ ] T017 - Create a detailed outline for chapters 126-150 [4 hours]

### Writing Tasks
- [ ] W076 - Chapter 76: Farewell to Longxi [4000 words]
- [ ] W077 - Chapter 77: First meeting with Jiang Chenxi [4000 words]
- [ ] W078 - Chapter 78: The group of four in the dormitory [4000 words]
...
```

---

#### Step 5: Write (Write Volume Two)

```bash
/write
```

The AI will start writing Chapter 76 based on the new specification, plan, and tasks for Volume Two.

---

#### Step 6: Analyze (Volume Two Validation)

```bash
/track --check  # Every 10 chapters
```

**Validation after Chapter 85**:
```
✅ Do chapters 76-85 conform to the new specification for Volume Two?
✅ Is the development of the transportation line reasonable?
✅ Is Jiang Chenxi's appearance natural?
✅ Is the transition from Volume One smooth?
```

---

## Scenario 3: Chapter/Paragraph-Level SDD (Tactical Adjustment)

### Scenario Description

When writing Chapter 10, the AI wrote:
```
Chen Rui met veteran Lao Zhang in the county town.
Lao Zhang saw that Chen Rui had a knack for business,
and invited him to partner in the transportation business.
```

But the original plan for Chapter 10 was:
```
Chen Rui earns his first pot of money by selling cassettes on the street.
```

You feel that the transportation line has more potential and want to keep the AI's deviation.

**This is a "chapter/paragraph-level" specification change, affecting chapters 10-20.**

---

### SDD Response: Re-execute the process for chapters 10-20

#### Step 1: Specify (Minor Specification for Chapters 10-20)

```bash
/specify
```

**❌ Bad prompt** (too vague):
```
Chapter 10 went off track, I want to change it.
```
→ Problem: Doesn't say how it went off track, how you want to change it, or how many chapters are affected.

**✅ Good prompt** (clear scope):

```
📝 User Input Box:

【Situation】
In Chapter 10, the AI wrote about a "transportation line" (Chen Rui meets Lao Zhang, partners in transportation),
but the original plan was "selling cassettes on the street".

I think the transportation line has more potential and I want to keep this deviation.

【Scope of Impact】
Chapters 10-20 need to be re-planned (about 10 chapters, 40,000 words).

【New Specification】
- Chapter 10: Meet Lao Zhang, decide to partner in transportation [Written ✓]
- Chapters 11-13: Learn the transportation industry, first long-distance trip.
- Chapters 14-16: Business starts, encounter competitors.
- Chapters 17-19: Solve initial difficulties.
- Chapter 20: Earn the first pot of gold (about 5000 yuan).

【Needs Update】
1. Update `specification.md`:
   - "First pot of gold" source: from "selling cassettes on the street" to "transportation business".

2. New character setting:
   - Lao Zhang: Veteran, 40 years old, has a truck, needs a partner.
   - Relationship with protagonist: Mentor + partner.

3. Explain the balance:
   - How does the transportation line not affect college entrance exam preparation?
   - Chen Rui participates on weekends and holidays, focuses on studying on weekdays.

【Unaffected Parts】
- Chapters 1-9 remain unchanged.
- The college entrance exam preparation line in chapters 21-30 continues.
- The family line and childhood sweetheart line are unaffected.
```

**💬 Complete dialogue flow**:

```
┌─────────────────────────────────┐
│ 👤 User Input                      │
└─────────────────────────────────┘
/specify

[The complete prompt above]

┌─────────────────────────────────┐
│ 🤖 AI Response                        │
└─────────────────────────────────┘
I understand! The deviation to the transportation line in Chapter 10 is a good opportunity.

Let me:
1. Update the "First Pot of Gold" section in `specification.md`.
2. Create a character profile for Lao Zhang.
3. Explain the balance between the transportation line and the college entrance exam.

【Update Preview】
## First Pot of Gold (Adjusted)
- Original: Selling cassettes on the street, earning about 3000 yuan.
- New: Partnering with veteran Lao Zhang in transportation, earning about 5000 yuan.
- Time: Two months before the 1984 college entrance exam.
- Participation method: Participates on weekends and holidays, focuses on exam preparation on weekdays.

## New Character: Lao Zhang
- Name: Zhang Guoqing
- Age: 40
- Identity: Veteran, individual transport operator.
- Personality: Upright and straightforward, values loyalty.
- With protagonist: Initially a mentor, later a partner.

Confirm update?

┌─────────────────────────────────┐
│ 👤 User Confirmation                      │
└─────────────────────────────────┘
Confirm

┌─────────────────────────────────┐
│ 🤖 AI Completion                        │
└─────────────────────────────────┘
✅ `specification.md` updated.
✅ `characters/zhang-guoqing.md` created.
✅ "First Pot of Gold" source adjusted.

Next step suggestion: /clarify (clarify 3 key decisions for chapters 10-20)
```

**AI Output**: Updates `specification.md` and adds Lao Zhang's character profile.

---

#### Step 2: Clarify (Decisions for Chapters 10-20)

```bash
/clarify
```

**AI asks**:
```
Q1: Lao Zhang's character depth?
   A) A tool, just to help the protagonist make money.
   B) An important supporting character with his own storyline.
   C) A major partner later on, deeply involved.
→ Suggestion: B

Q2: How does the transportation line not affect college entrance exam preparation?
   A) Chen Rui quickly withdraws and lets Lao Zhang operate independently.
   B) Chen Rui participates on weekends and holidays, prepares for the exam on weekdays.
   C) Chen Rui gives up the college entrance exam and devotes himself fully.
→ Suggestion: B (fits the protagonist's rational character)

Q3: The specific amount of the first pot of gold?
   A) 3000 yuan (conservative)
   B) 5000 yuan (reasonable)
   C) 10000 yuan (too high, unrealistic)
→ Suggestion: B
```

---

#### Step 3: Plan (Detailed Plan for Chapters 10-20)

```bash
/plan
```

**Input**: "Re-plan chapters 10-20, focusing on the transportation line."

**AI Output**: New outline

```markdown
### Chapters 10-20: Transportation Business Line (New)

#### Chapter 10: An Unexpected Opportunity [Written ✓]
- Chen Rui meets Lao Zhang in the county town.
- Lao Zhang invites him to partner in transportation.
- Chen Rui agrees after weighing the options.

#### Chapter 11: Learning the Industry Rules
- Learning the transportation process from Lao Zhang.
- Understanding the policies for individual transport in 1984.
- First visit to the freight station.

#### Chapter 12: The First Long-Distance Trip
- Chen Rui goes with the truck to Xiamen.
- Sights on the road (highways, checkpoints in the 80s).
- Realizing that making money is not easy.

#### Chapter 13: A Taste of Success
- The first order earned 200 yuan.
- Chen Rui gets 100 yuan.
- Excitement and contemplation.

#### Chapter 14: Encountering Trouble
- Goods are detained (incomplete paperwork).
- Chen Rui uses "foresight" knowledge to solve it.
- Lao Zhang is impressed.

#### Chapter 15: Business Takes Off
- Establishing the partnership model (70/30 split, Lao Zhang gets 70, Chen Rui gets 30).
- Chen Rui proposes improvement plans.
- Business gradually stabilizes.

#### Chapter 16: Competitors Appear
- Another transportation team tries to snatch business.
- Price war.
- Chen Rui's advice: service differentiation.

#### Chapter 17: Gaining a Small Reputation
- Word of mouth spreads, business improves.
- Chen Rui becomes somewhat famous in the county.
- Parents' attitude changes (from worry to tacit approval).

#### Chapter 18: Balancing Studies
- The college entrance exam is approaching, Chen Rui reduces his participation.
- Lao Zhang understands and supports him.
- Chen Rui leaves development suggestions for Lao Zhang.

#### Chapter 19: A Crisis
- Lao Zhang has a car accident (minor injuries).
- Chen Rui steps up and completes an order independently.
- Proving his ability.

#### Chapter 20: The First Pot of Gold
- Accounting: earned 5000 yuan in two months.
- Chen Rui decides to use 3000 yuan as capital after the exam.
- 2000 yuan is left for Lao Zhang for working capital.
- Focusing on exam preparation.
```

---

#### Step 4: Tasks (Tasks for Chapters 10-20)

```bash
/tasks
```

**AI Output**: `tasks.md` update

```markdown
## Chapter/Paragraph Task Adjustment

### Deleted Original Tasks
- ~~W010 - Chapter 10: Selling cassettes on the street~~
- ~~W011-W015 - Cassette sales line~~

### New Tasks
- [x] W010 - Chapter 10: An unexpected opportunity (transportation line) ✓
- [ ] T008 - Create a character profile for Lao Zhang [1 hour]
- [ ] W011 - Chapter 11: Learning the industry rules [4000 words]
- [ ] W012 - Chapter 12: The first long-distance trip [4000 words]
- [ ] W013 - Chapter 13: A taste of success [4000 words]
- [ ] W014 - Chapter 14: Encountering trouble [4000 words]
- [ ] W015 - Chapter 15: Business takes off [4000 words]
- [ ] W016 - Chapter 16: Competitors [4000 words]
- [ ] W017 - Chapter 17: Gaining a small reputation [4000 words]
- [ ] W018 - Chapter 18: Balancing studies [4000 words]
- [ ] W019 - Chapter 19: A crisis [4000 words]
- [ ] W020 - Chapter 20: The first pot of gold [4000 words]
```

---

#### Step 5: Write (Write Chapters 11-20)

```bash
/write
```

The AI continues to write Chapter 11 based on the new plan.

---

#### Step 6: Analyze (Validation of Chapters 10-20)

```bash
/track --check  # After Chapter 20
```

**Validation**:
```
✅ Is the transportation line in chapters 10-20 self-consistent?
✅ Is the timeline reasonable (can you earn 5000 yuan in two months before the exam)?
✅ Is Lao Zhang a three-dimensional character?
✅ Is the transition from chapters 1-9 smooth?
✅ Does it leave room for the exam preparation line in chapters 21-30?
```

---

## Scenario 4: Single-Chapter-Level SDD (Smallest Granularity)

### Scenario Description

After finishing Chapter 5, "The Shock of the Supply and Marketing Cooperative," you are not satisfied with the quality:
- The environmental description is too much of a running account.
- It lacks the impact of the era.
- The protagonist's emotions are not full enough.

**This is a single-chapter quality issue that requires a rewrite.**

---

### SDD Response: Re-execute for Chapter 5

#### Step 1: Plan (New Plan for Chapter 5)

```bash
/write
```

**Choice**: Rewrite existing chapter → Chapter 5

**❌ Bad prompt** (too general):
```
Chapter 5 is not well written, rewrite it.
```
→ Problem: Doesn't say what's not good, or how to change it.

**✅ Good prompt** (clear problems + clear direction):

```
📝 User Input Box:

【Choice】Rewrite existing chapter → Chapter 5

【Original Chapter Problems】
1. The environmental description is a running account, lacking details and atmosphere.
2. It fails to show the era's impact of the 1984 supply and marketing cooperative.
3. Chen Rui's emotions are not full enough, the sense of shock is insufficient.
4. The hook at the end of the chapter is weak and not engaging enough.

【Improvement Direction】

1. Add era-specific details and scenes:
   - The glass counters of the supply and marketing cooperative (looking at goods through glass).
   - The attitude of the sales staff (not "the customer is always right").
   - A prominent "supply by coupon" sign.
   - The oppressive atmosphere of customers queuing to buy things.
   - Few types and quantities of goods on the shelves.

2. Add a conflict event (to enhance drama):
   - Chen Rui sees someone who can't buy flour because they don't have grain coupons, and their plea to the sales staff is rejected.
   - Sees a child staring at a can of food (can't afford it).
   - Contrast with the abundance of a 2025 supermarket.
   - Chen Rui's inner shock: So this is what poverty is really like.

3. Enhance emotional layers:
   - Shock: The material scarcity of 1984 is beyond imagination.
   - Heartache: Seeing poor people who can't even buy flour.
   - Guilt: How much he wasted in 2025.
   - Determination: He must let his family live a good life.
   - Thought: What can I change with my foresight knowledge?

4. Strengthen the chapter-end hook:
   - Chen Rui overhears two people discussing "ten-thousand-yuan households".
   - Or meets a future key figure (teacher/businessman).
   - Lay a foundation: Pave the way for the money-making line in Chapter 10.

【Constraints】
- Word count: Around 4000 words.
- Style: In line with the "shaping atmosphere with details" principle in the constitution.
- Connection: Coherent with the content of Chapters 4 and 6.
- Time: Keep it on a certain day in July 1984.

【Expected Effect】
Readers should be able to feel the real life of 1984 through Chapter 5,
and understand why Chen Rui wants to change his destiny.
```

**💬 Complete dialogue flow**:

```
┌─────────────────────────────────┐
│ 👤 User Input                      │
└─────────────────────────────────┘
/write

Choice: Rewrite existing chapter
Choose chapter: Chapter 5

[The complete prompt above]

┌─────────────────────────────────┐
│ 🤖 AI Response                        │
└─────────────────────────────────┘
Understood! I will rewrite Chapter 5, focusing on enhancing the era's atmosphere and emotional impact.

Let me first read the relevant files:
✓ Chapter 4 content (to ensure connection)
✓ Chapter 6 content (to ensure coherence)
✓ `constitution.md` (style principles)
✓ `1984-database.md` (details of the supply and marketing cooperative)

Starting to rewrite Chapter 5...

[AI generates 4000 words of new content]

【Rewrite Key Points】
- Added specific descriptions of the glass counters and "supply by coupon" at the supply and marketing cooperative.
- Added a conflict event of someone being unable to buy flour without grain coupons.
- Enhanced the emotional layers of Chen Rui's inner shock and determination.
- Added a discussion of "ten-thousand-yuan households" at the end of the chapter to pave the way for the subsequent money-making line.

Confirm to replace Chapter 5?

┌─────────────────────────────────┐
│ 👤 User Confirmation                      │
└─────────────────────────────────┘
Confirm

┌─────────────────────────────────┐
│ 🤖 AI Completion                        │
└─────────────────────────────────┘
✅ `chapter-005.md` has been rewritten.
✅ Word count: 4012 words.
✅ Connection with Chapters 4 and 6 has been verified.

Suggestion: Use `/plot-check` to verify the plot coherence after rewriting Chapter 5.
```

---

#### Step 2: Write (Rewrite Chapter 5)

The AI will:
1. Read the constitution (style principles).
2. Read Chapters 4 and 6 (to ensure connection).
3. Read the requirements for Chapter 5 in the outline.
4. Read `1984-database.md` (details of the supply and marketing cooperative).
5. Rewrite using the new plan.

---

#### Step 3: Analyze (Validate Chapter 5)

```bash
/plot-check  # Single chapter validation
```

**Validation**:
```
✅ Is the plot of Chapter 4 → Chapter 5 → Chapter 6 coherent?
✅ Is Chen Rui's psychological change natural?
✅ Are the era details accurate?
✅ Is the word count around 4000?
✅ Is the chapter-end hook effective?
```

---

## Flexible Application of the SDD Methodology

### When to Execute Which Level of SDD?

#### Decision Tree

```
Need to modify?
    ↓
Does it affect the core setting of the entire book? (e.g., major change in protagonist's personality, change in story genre)
    ├─ Yes → Book-level SDD (specify → plan → tasks)
    └─ No ↓

Does it affect the structure of an entire volume? (e.g., change in the main plot of a volume)
    ├─ Yes → Volume-level SDD (specify volume → plan volume → tasks volume)
    └─ No ↓

Does it affect the plot of 10-30 chapters? (e.g., change in a subplot)
    ├─ Yes → Chapter/paragraph-level SDD (plan section → tasks section → write)
    └─ No ↓

Is it just that the quality of a certain chapter is not satisfactory? (e.g., the writing is not exciting enough)
    └─ Yes → Single-chapter-level SDD (rewrite → analyze)
```

---

### Example: Decision Cases for "Back to 1984"

| Change Type | Scope of Impact | SDD Level | Command Flow |
|---|---|---|---|
| Change protagonist from "low-key" to "high-profile" | Entire book | Book-level | `/constitution` → `/specify` → `/plan` → `/tasks` |
| Change the business line in Volume Two from "street vending" to "transportation" | One volume (chapters 76-150) | Volume-level | `/specify`(volume) → `/plan`(volume) → `/tasks`(volume) |
| Change chapters 10-20 from "selling cassettes" to "transportation" | Chapter section (10 chapters) | Chapter/paragraph-level | `/specify`(section) → `/plan`(section) → `/tasks`(section) |
| Rewrite Chapter 5 to add details | Single chapter | Single-chapter-level | `/write`(rewrite) → `/analyze` |
| Change Lin Xiuzhen's personality from "conservative" to "open-minded" | Character setting | Book-level | `/specify`(character) → `/write`(rewrite affected chapters) |
| Add the character of Lao Zhang | New character | Chapter/paragraph-level | `/specify`(character) → `/plan`(affected chapters) |

---

### The Importance of Frequent Validation

#### Suggested Validation Frequency

| Granularity | When to Validate | Command | What to Check |
|---|---|---|---|
| **After spec is complete** | Before writing | `/checklist Spec Completeness` | Quality of specification documents |
| **After plan is complete** | Before writing | `/checklist Outline Quality` | Quality of planning documents |
| **After each chapter is written** | Immediately | AI automatic | Character names, basic logic |
| **Every 5-10 chapters** | Periodically | `/track --check` + `/checklist Character Consistency` | Timeline, relationships, worldview, content quality |
| **After completing a volume** | End of volume | Comprehensive validation | `/plot-check` `/timeline` `/relations` `/world-check` |
| **After completing the whole book** | At the end | Final review | `/analyze` comprehensive quality |

#### Example: Validation Plan for "Back to 1984"

```bash
# Before writing: Specification validation
/specify
/checklist Spec Completeness
- ✅ Ensure `specification.md` quality is up to standard
/plan
/checklist Outline Quality
- ✅ Ensure `creative-plan.md` quality is up to standard

# After Chapter 10 (Part One complete)
/track --check
/checklist Character Consistency 1-10
- Check character consistency in chapters 1-10
- Check the timeline (10 days in July 1984)
- Check worldview details (grain coupons, wages, etc.)
- Scan the quality of character descriptions

# After Chapter 25 (Part Two complete)
/track --check
/plot-check
/checklist Plot Logic 11-25
- Check the plot progression in chapters 11-25
- Check the logic of the transportation line
- Check the setup of foreshadowing
- Validate plot reasonableness

# After Chapter 50 (Part Three complete)
/track --check
/timeline
/relations
/checklist Timeline 26-50
- Check the time span of chapters 26-50
- Check the development of character relationships
- Check the family line, friendship line
- Validate timeline consistency

# After Chapter 75 (Volume One complete)
Complete validation:
/track --check
/plot-check
/timeline
/relations
/world-check
/checklist Constitution Compliance 1-75
/analyze
- Comprehensively check the quality of Volume One
- Validate compliance with the writing constitution
- Prepare for the planning of Volume Two
```

---

## How the Tracking System Supports SDD

### /track-init: Establish Tracking Data

```bash
/track-init
```

**Function**: Initialize the tracking system based on existing content.

**"Back to 1984" Example**:
```
Scan the 7 written chapters and generate:
- `validation-rules.json`
  ↳ Rules for characters like Chen Rui, Lin Xiuzhen, Chen Guoqiang, etc.
- `plot-tracker.json`
  ↳ Plot nodes for chapters 1-7
- `timeline.json`
  ↳ Timeline for July 1984
- `character-state.json`
  ↳ Chen Rui @ Longxi County mining area, Lin Xiuzhen @ home
- `relationships.json`
  ↳ Chen Rui-Lin Xiuzhen (mother-son), Chen Rui-Chen Guoqiang (father-son)
```

---

### /track: View Current Status

```bash
/track              # Full report
/track --brief      # Brief information
/track --stats      # Statistical data
```

**Example Output**:
```
📊 "Back to 1984" Writing Report
━━━━━━━━━━━━━━━━━━━
✍️ Writing Progress
  Completed: 7/300 chapters (2.3%)
  Word count: 28,000/1,200,000
  Current: Volume One, Part One

📍 Plot Status
  Main plot: Time travel adaptation [In progress]
  Subplot: Family relationships [Active]

⏰ Timeline
  Story time: July 1984
  Time span: 10 days

👥 Character Status
  Chen Rui: Longxi County mining area
  Lin Xiuzhen: Active
  Chen Guoqiang: Active

⚡ Pending
  Next chapter: Chapter 8
  Foreshadowing: 1 (reason for time travel)
```

---

### /track --check: Deep Validation

```bash
/track --check
```

**"Back to 1984" Example**:
```
🔍 Deep Validation Report (Chapters 1-7)
━━━━━━━━━━━━━━━━━━━

✅ Passed items: 18/20

❌ Problems found (2):
1. [Low] Chapter 3: "Lin Xiuzhen" first appears without an introduction of her identity.
2. [Low] Chapter 6: The time description "yesterday" is unclear.

Suggestions:
- In Chapter 3, add: "mother Lin Xiuzhen".
- In Chapter 6, clarify: "July X".

Auto-fix: 2 problems can be fixed.
Run `/track --fix` to fix them.
```

---

### /track --fix: Auto-Fix

```bash
/track --fix
```

**"Back to 1984" Example**:
```
🔧 Auto-Fix Report
━━━━━━━━━━━━━━━

✅ Fixed: 2 problems
- Chapter 3: Added "mother Lin Xiuzhen".
- Chapter 6: "yesterday" → "July 15th".

Fix complete! Suggest running `/track --check` again to verify.
```

---

### /checklist: Quality Checklist (New in v0.16.0)

**Double-check mechanism**: Like "unit tests" in software, establish a quality gate for writing documents.

#### Type 1: Specification Quality Check (Before Writing)

```bash
/checklist Outline Quality
```

**Purpose**: Validate the quality of documents like `specification.md`, `creative-plan.md`, etc.

**"Back to 1984" Example**:
```
📋 Outline Quality Checklist
━━━━━━━━━━━━━━━━━

✅ Completeness (5/5)
  ✓ Story specification includes a one-sentence story.
  ✓ Target positioning is clear (word count, chapters, readers).
  ✓ Core setting is complete (protagonist, location, time).
  ✓ Main relationship lines are defined.
  ✓ Success criteria are measurable.

✅ Clarity (4/5)
  ✓ Each chapter section has clear key events.
  ✓ Climax and turning points are marked.
  ✓ The clue management table is complete.
  ⚠️ Insufficient details for chapters 50-75 (suggestion: supplement).

✅ Consistency (5/5)
  ✓ The timeline is consistent.
  ✓ Character settings have no contradictions.
  ✓ Worldview rules are unified.

⚠️ Suggested Improvements:
1. [P1] Supplement specific events for chapters 50-75.
2. [P2] Add planning for clue convergence points in Volume Two.

Generate file: `spec/checklists/outline-quality.md` (overwrite mode)
```

#### Type 2: Content Validation Check (After Writing)

```bash
/checklist Character Consistency 1-10
```

**Purpose**: Scan written chapters to check content quality.

**"Back to 1984" Example**:
```
📋 Character Consistency Check (Chapters 1-10)
━━━━━━━━━━━━━━━━━━━━━

✅ Passed items (18/20)

❌ Problems found (2):
1. [Medium] Chapter 3: Chen Rui's level of shock about 1984 is inconsistent with Chapter 1.
   - Chapter 1: Very shocked.
   - Chapter 3: Acts very adapted.
   Suggestion: Unify the transition rhythm from shock to adaptation.

2. [Low] Chapter 7: Lin Wan's personality description is slightly different from Chapter 5.
   - Chapter 5: "Lively and cheerful".
   - Chapter 7: "Introverted and shy".
   Suggestion: Determine Lin Wan's core personality.

💡 Improvement Suggestions:
- Review Chapter 3 and add descriptions of the adaptation process.
- Unify Lin Wan's personality setting (suggest modifying Chapter 7).

Generate file: `spec/checklists/character-consistency-20251011.md` (with date)
```

#### Five Check Types

**Specification Quality Check (Before Writing)**:
1. `/checklist Outline Quality` - Checks `outline/creative-plan.md`
2. `/checklist Spec Completeness` - Checks `specification.md`
3. `/checklist Worldview Consistency` - Checks `worldbuilding/*.md`
4. `/checklist Character Profiles` - Checks `characters/*.md`
5. `/checklist Clue Planning` - Checks `specification.md` Chapter 5

**Content Validation Check (After Writing)**:
1. `/checklist Character Consistency 1-10` - Scans chapters 1-10
2. `/checklist Plot Logic 1-20` - Scans chapters 1-20
3. `/checklist Timeline 1-30` - Scans chapters 1-30
4. `/checklist Dialogue Style 5-15` - Scans chapters 5-15
5. `/checklist Constitution Compliance 1-50` - Scans chapters 1-50

#### When to Use

```
Validation process before writing:
/specify → /checklist Spec Completeness → /plan → /checklist Outline Quality → /tasks → /write
    ↑                                                                       ↓
    └──────────────── Discover spec problems, fix immediately ────────────────────────────┘

Validation process after writing:
/write (finish 10 chapters) → /checklist Character Consistency 1-10 → Discover problems → Fix → Continue
```

---

## Summary: The Core Value of SDD

### 1. The Specification is the Single Source of Truth

**Traditional way**:
```
Ideas in your head ≠ What you've written ≠ Future plans
→ Chaos, inconsistency, a lot of rework
```

**SDD way**:
```
`specification.md` = The only authority
    ↓
`creative-plan.md` = The implementation plan for the specification
    ↓
`tasks.md` = The decomposition of the plan
    ↓
`content/*.md` = The execution result of the tasks
    ↓
All modifications trace back to the specification.
```

---

### 2. Hierarchical Recursion is the Core

**It's not**:
```
Specify once → Write until the end
```

**But rather**:
```
Entire book: specify → plan → tasks
   ↓
Each volume: specify → plan → tasks → write → analyze
   ↓              ↑_________________________|
Each section: plan → write → analyze
   ↓         ↑____________|
Each chapter: write
```

---

### 3. Allow Deviation, but Synchronize the Specification

**When deviation occurs**:
```
The AI writes a plot that is not in the plan
    ↓
Evaluate: Is it better than the original plan?
    ↓
Good → Keep it → Immediately update the upstream specification
    ↓
`specification` → `plan` → `tasks` remain consistent
    ↓
Continue writing, AI is based on the new specification.
```

**"Back to 1984" Example**:
```
In Chapter 10, the AI writes "transportation line" (not in the original plan)
    ↓
Evaluate: The transportation line is more dramatic than selling on the street ✓
    ↓
Update specification: "First pot of gold comes from transportation"
Update plan: Chapters 10-20 revolve around transportation
Update tasks: W011-W020 are changed to transportation tasks
    ↓
Continue writing Chapter 11, the AI knows that "transportation" is the correct path.
```

---

### 4. Frequent Validation Prevents Accumulated Errors

**Traditional way**:
```
Write 100 chapters → Find a bug in the first 50 chapters → Massive rework
```

**SDD way**:
```
Validate every 5-10 chapters → Find small problems → Small fixes → Continue
    ↓
Validate each volume → Find structural problems → Adjust the plan for the next volume
    ↓
Validate at the end → Final polishing
```

---

## Appendix: Command Quick Reference Table

> 💡 **Namespace Tip**: The table uses the generic format. In actual use:
> - Claude Code adds the `novel.` prefix (e.g., `/novel.write`)
> - Gemini CLI adds the `novel/` prefix (e.g., `/novel/write`)
> - Other platforms use it directly (e.g., `/write`)

### Seven-Step Methodology Commands

| Command | Level | Purpose | "Back to 1984" Example |
|---|---|---|---|
| `/constitution` | Global | Writing constitution | 5 major principles (historical authenticity, etc.) |
| `/specify` | Variable | Specification definition | Spec for the whole book/a volume/a section |
| `/clarify` | Variable | Clarify decisions | 5 key questions |
| `/plan` | Variable | Creative plan | Plan for the whole book/a volume/a section |
| `/tasks` | Variable | Task decomposition | 47 tasks (preparation + writing) |
| `/write` | Execution | Writing | Write Chapter 8 / Rewrite Chapter 5 |
| `/analyze` | Validation | Comprehensive validation | Smart dual mode (framework/content) |

### Tracking and Validation Commands

| Command | Level | Purpose | "Back to 1984" Example |
|---|---|---|---|
| `/track-init` | Initialization | Initialize tracking system | Generate tracking data based on 7 chapters |
| `/track` | Tracking | View progress | View at any time |
| `/track --check` | Validation | Deep validation | Every 5-10 chapters |
| `/track --fix` | Fixing | Auto-fix | After finding problems |
| `/checklist` | Validation ⭐ | Quality checklist | Spec quality (before writing) + Content validation (after writing) |
| `/plot-check` | Validation | Plot check | Individual plot validation |
| `/timeline` | Validation | Timeline check | Time logic validation |
| `/relations` | Validation | Relationship check | Character relationship validation |
| `/world-check` | Validation | Worldview check | Setting consistency validation |

### Auxiliary Commands

| Command | Level | Purpose | "Back to 1984" Example |
|---|---|---|---|
| `/expert` | Consultation | Expert advice | When encountering difficulties |

---

## VI. Multi-Thread Management Guide

### 6.1 Problem Scenario

**A real user's confusion**:
> "The interweaving of the main plot and subplots is very difficult to explain to the AI how to maintain in parallel, and how to intersect and reveal previous clues at the appropriate time. Especially when the plot setting is constantly changing, it's a disaster."

**Three core pain points**:
1. **Parallel progression**: The AI doesn't know which clues to advance in this chapter.
2. **Intersection timing**: The AI improvises, and the intersection points are not controlled.
3. **Consistency after modification**: Change one place, and everything else is messed up.

### 6.2 Solution: No New Commands Needed

**Core idea**: Enhance **clue management** capabilities within the existing `/specify` → `/plan` → `/tasks` → `/track` flow.

#### Step 1: `/specify` - Define All Clues

Add a **Chapter 5: Clue Management Specification** to `specification.md`, containing 5 tables:

**5.1 Clue Definition Table** - Defines the ID, type, and conflict of all clues:
```markdown
| Clue ID | Clue Name | Type | Priority | Start/End Chapters | Core Conflict |
|---|---|---|---|---|---|
| PL-01 | Family Line | Main Support | P0 | 1-300 | Poverty vs. Changing Destiny |
| PL-02 | Love Line-Lin Wan | Main | P0 | 5-270 | Innocence vs. Reality |
```

**5.2 Clue Pacing Plan** - Plans the activity level of each clue in different volumes:
```markdown
| Clue ID | Volume One | Volume Two | Volume Three | Volume Four |
|---|---|---|---|---|
| PL-01 | ⭐⭐⭐ Active | ⭐⭐ Medium | ⭐ Background | ⭐⭐⭐ Active |
```

**5.3 Clue Intersection Point Plan** - Pre-plan intersection timing (to avoid AI improvisation):
```markdown
| Intersection ID | Chapter | Involved Clues | Intersection Content | Expected Effect |
|---|---|---|---|---|
| X-001 | 40 | PL-01+PL-02 | First time earning money helps the family, Lin Wan is grateful | The two lines intersect, emotions heat up |
```

**5.4 Foreshadowing Management Table** - Manages the setup and reveal of foreshadowing:
```markdown
| Foreshadowing ID | Setup Chapter | Involved Clues | Content | Reveal Chapter |
|---|---|---|---|---|
| F-001 | 5 | PL-01+PL-02 | Lin Wan's family is in difficulty | 40 |
```

**5.5 Clue Modification Decision Matrix** - A checklist for checking the impact of modifying a clue.

#### Step 2: `/plan` - Mark Active Clues for Each Chapter Section

Add "Active Clues" and "Intersection Point" columns to the chapter section table in `creative-plan.md`:

```markdown
| Chapter Section | Content | Key Events | **Active Clues** | **Intersection Point** |
|---|---|---|---|---|
| Chapters 1-10 | Time Travel and Adaptation | ... | PL-01⭐⭐⭐ | None |
| Chapters 11-25 | Determination and Action | ... | PL-01⭐⭐, PL-02⭐⭐⭐ | None |
| Chapters 26-50 | Exam Prep and Growth | ... | PL-01⭐⭐, PL-02⭐, PL-03⭐⭐⭐ | X-001(Chapter 40) |
```

#### Step 3: `/tasks` - Mark Involved Clues for Each Chapter

Add an "Involved Clues" field to each writing task in `tasks.md`:

```markdown
- [ ] **W040** - Chapter 40: The First Pot of Gold
  - **Core Task**: Chen Rui earns a large sum of money for the first time.
  - **Involved Clues**:
    - PL-01(Family Line) ⭐⭐⭐ Main progression
    - PL-02(Love Line-Lin Wan) ⭐⭐ Auxiliary
  - **Intersection Point**: X-001(Family + Love)
  - **Foreshadowing Reveal**: F-001(Helping Lin Wan's family)
  - **Must Include**: Earning money, helping family, Lin Wan's gratitude, emotions heating up.
```

#### Step 4: `/track --init` - Automatic Tracking Generation

When running `/track --init`, it automatically populates `plot-tracker.json` from Chapter 5 of `specification.md`:

```json
{
  "plotlines": {
    "subplots": [
      {
        "id": "PL-01",
        "name": "Family Line",
        "status": "active",
        "upcomingNodes": [
          {"chapter": 40, "node": "First time helping family (X-001)"}
        ]
      }
    ]
  },
  "intersections": [
    {"id": "X-001", "chapter": 40, "plotlines": ["PL-01", "PL-02"]}
  ]
}
```

### 6.3 Usage Example: "Back to 1984"

#### Example 1: Initial Planning of Multiple Clues

```bash
/specify

📝 User Input:
【Situation】
"Back to 1984" has 5 clues: Family line, Friendship line, Love line (Lin Wan), Love line (Jiang Chenxi), and Career line.
The current `specification.md` lacks a clue management specification.

【Modification Intent】
Add a Chapter 5 to `specification.md`, defining the IDs, pacing, 7 intersection points, and 5 foreshadowing for the 5 clues.

【Needs Update】
- `specification.md`: Add Chapter 5

【Expected Output】
`specification.md` v1.2.0 with a complete clue management specification.
```

The AI will generate a Chapter 5 with 5 tables, clarifying the plan for all clues.

#### Example 2: Advancing Clues During Writing

```bash
/write Chapter 40

📝 User Input:
【Situation】
Chapter 40 is the intersection point X-001 (Family line + Love line), and needs to reveal foreshadowing F-001.

【Writing Intent】
This chapter involves 3 clues:
- PL-05(Career Line)⭐⭐⭐: Earning the first pot of gold.
- PL-01(Family Line)⭐⭐⭐: Taking the money home.
- PL-02(Love Line-Lin Wan)⭐⭐: Helping Lin Wan's family.

【Must Complete】
1. Write the process of earning money (climax of the career line).
2. Parents' shock and emotion (deepening of the family line).
3. Helping Lin Wan's family reveals F-001 (progression of the love line).
4. The two lines intersect, emotions heat up.

【Expected Output】
4000-word chapter + automatic update of `plot-tracker.json`.
```

#### Example 3: Impact Assessment After Modifying a Clue

```bash
/specify

📝 User Input:
【Situation】
The original plan was for Lin Wan to come to the city in Chapter 150, now I want to move it to Chapter 100.

【Modification Intent】
Adjust PL-02 (Lin Wan line):
1. Change intersection point X-004 from Chapter 150 to Chapter 100.
2. Change the activity level of the Lin Wan line in Volume Two from ⭐⭐ to ⭐⭐⭐.

【Impact Assessment】
According to the 5.5 modification decision matrix, check the impact:
- Section 5.2: Pacing adjustment for Volume Two.
- Section 5.3: X-004 is moved up.
- Section 5.4: F-002 may need to be set up earlier.
- `creative-plan.md`: The chapter sections for 96-150 need to be adjusted.
- `tasks.md`: Tasks W096-W150 need to be re-planned.

【Needs Update】
- `specification.md` sections 5.2, 5.3.
- Output an impact assessment report.

【Expected Output】
Updated `specification.md` + a list of the scope of impact.
```

Then, run `/plan` and `/tasks` sequentially to synchronize the changes based on the impact assessment.

#### Example 4: Verifying Multi-Clue Consistency

```bash
/track --check

📝 User Input:
【Situation】
The first 50 chapters are written, I need to verify if the progress of multiple clues is consistent with the plan.

【Check Content】
1. Are all clues progressing according to the pacing in spec section 5.2?
2. Has intersection point X-001 (Chapter 40) been completed?
3. Has foreshadowing F-001 been revealed?
4. Has any clue been dormant for more than 20 chapters?

【Expected Output】
Consistency check report:
- ✅ Completed clue nodes.
- ⚠️ Content that deviates from the plan.
- ❌ Missed intersection points or foreshadowing.
- 💡 Suggestions.
```

### 6.4 Solving the Three Major Pain Points

| Pain Point | Solution | Specific File |
|---|---|---|
| **Parallel progression** | `tasks.md` marks "Involved Clues" for each chapter | W040 task is marked PL-01⭐⭐⭐, PL-02⭐⭐ |
| **Intersection timing** | `specification.md` section 5.3 pre-plans | X-001 is set for Chapter 40 to avoid AI improvisation |
| **Modification consistency** | 5.5 modification decision matrix + `/track --check` | When modifying PL-02, the scope of impact is automatically prompted |

### 6.5 Core Advantages

✅ **No new commands needed** - Completely uses the existing 7 commands.
✅ **Conforms to the SDD methodology** - Clue management is distributed in `specify` → `plan` → `tasks` → `track`.
✅ **Supported by writing theory** - Concepts from Story Grid's Grid Spreadsheet, Save the Cat's B Story.
✅ **Solves real pain points** - Based on actual user needs.

---

## Final Words

**The SDD methodology is not dogma, but an empowering tool.**

It helps you:
- ✅ Transform vague inspiration into a clear work.
- ✅ Quickly adjust when deviations occur.
- ✅ Maintain consistency at different granularities.
- ✅ Manage complex multi-clue writing.
- ✅ Continuously produce high-quality content.

**Remember the core**:
> Specification-driven + Hierarchical recursion + Allow deviation + Frequent validation

**The success experience of "Back to 1984"**:
- A clear constitution (5 major principles).
- A clear specification (1.2 million word goal + 5 clue plan).
- Flexible plan adjustments (allowing clue deviations).
- Effective tracking and validation (7 chapters maintaining high quality).

May you create wonderful works using the SDD methodology!

---

*"Professionalism begins with method, excellence comes from perseverance."*
