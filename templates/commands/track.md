---
name: track
description: Comprehensively track the novel's creation progress and content
argument-hint: [--brief | --plot | --stats | --check | --fix]
allowed-tools: Read(//spec/tracking/**), Read(spec/tracking/**), Read(//stories/**), Read(stories/**), Bash(find:*), Bash(wc:*), Bash(grep:*), Bash(*)
model: claude-sonnet-4-5-20250929
scripts:
  sh: .specify/scripts/bash/track-progress.sh
  ps: .specify/scripts/powershell/track-progress.ps1
---

# Comprehensive Progress Tracking

Provides a full display of the various progress and statuses of the novel's creation.

## Tracking Dimensions

1.  **Writing Progress** - Word count, chapters, completion rate
2.  **Plot Development** - Main plot progress, subplot status
3.  **Timeline** - Story time progression
4.  **Character Status** - Character development and location
5.  **Foreshadowing Management** - Setup and resolution status

## Usage

Execute the script {SCRIPT} [options]:
- No arguments - Display the full tracking report
- `--brief` - Display brief information
- `--plot` - Display only plot tracking
- `--stats` - Display only statistics
- `--check` - **[Enhanced]** Perform a deep consistency check (including character validation)
- `--fix` - **[New]** Automatically fix simple issues found

## Data Sources

Integrates information from multiple tracking files:
- `progress.json` - Writing progress
- `spec/tracking/plot-tracker.json` - Plot tracking
- `spec/tracking/timeline.json` - Timeline
- `spec/tracking/relationships.json` - Relationship network
- `spec/tracking/character-state.json` - Character status
- `spec/tracking/validation-rules.json` - **[New]** Validation rules (for --check and --fix)

## Example Output

```
📊 Novel Creation Comprehensive Report
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📖 "Chronicles of the Ming Dynasty's Splendor"

✍️ Writing Progress
  Completed: 60/240 chapters (25%)
  Word Count: 162,000/800,000
  Current: Volume 2 "Courtroom Intrigues"

📍 Plot Status
  Main Plot: The Great Reform [Initial phase in the court]
  Subplot: Romance [Getting to know each other]

⏰ Timeline
  Story Time: Spring of the 30th year of the Wanli Era
  Time Span: 5 months

👥 Main Characters
  Li Zhongyong: Hanlin Academy Compiler @ Beijing
  Shen Yuqing: Adopted daughter of Zhang Juzheng [Active]

⚡ Pending Items
  Foreshadowing: 3 unresolved
  Conflict: Reformists vs. Conservatives [Escalating]

✅ Consistency Check: Passed
```

## Enhanced Features

### Data Consistency Validation

Basic Check (executed by default):
- Consistency between plot-tracker.json and outline.md
- Time logic of timeline.json
- Relationship conflicts in relationships.json
- Location reasonableness in character-state.json

### Deep Validation Mode (--check)

When using the `--check` parameter, a programmatic deep validation is performed:

#### Internal Task Flow (automatic execution)
```markdown
# Phase 1: Basic Validation [Parallel Execution]
- [x] T001 [P] Execute plot consistency check (plot-check logic)
- [x] T002 [P] Execute timeline validation (timeline logic)
- [x] T003 [P] Execute relationship validation (relations logic)
- [x] T004 [P] Execute world-building validation (world-check logic)

# Phase 2: Deep Character Validation
- [x] T005 Load validation rules from validation-rules.json
- [x] T006 Scan for character names in all chapters
- [x] T007 Compare with character-state.json for name consistency
- [x] T008 Check if forms of address comply with relationships.json
- [x] T009 Validate if character behavior is consistent with their profile

# Phase 3: Generate Comprehensive Report
- [x] T010 Summarize all validation results
- [x] T011 Mark severity of issues
- [x] T012 Generate fix suggestions
```

#### Validation Report Example
```
📊 Deep Validation Report
━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ Passed items: 15/18

❌ Issues found (3):
1. [High] Chapter 3: Protagonist's name "Li Ming" should be "Li Zhongyong"
2. [Medium] Chapter 7: Incorrect form of address for Shen Yuqing, used "Senior Brother"
3. [Low] Chapter 12: Unexplained time skip in the timeline

🔧 Autofixable: 2
📝 Needs manual confirmation: 1

Run /track --fix to automatically fix simple issues.
```

### Automatic Fix Mode (--fix)

When using the `--fix` parameter, it automatically fixes issues based on the validation report:

#### Autofix Scope
1.  **Character Name Errors** - Automatically replace based on validation-rules.json
2.  **Fixed Form of Address Errors** - Automatically correct to the right form
3.  **Simple Typos** - Correct obvious typos

#### Fix Process
```markdown
# Internal Fix Tasks (automatic execution)
- [x] F001 Read the issue list from the validation report
- [x] F002 [P] Fix character name error in Chapter 3
- [x] F003 [P] Fix form of address error in Chapter 7
- [x] F004 Generate fix report
- [x] F005 Update tracking files
```

#### Fix Report Example
```
🔧 Automatic Fix Report
━━━━━━━━━━━━━━━━━━━
✅ Fixed: 2 issues
- Chapter 3: "Li Ming" → "Li Zhongyong"
- Chapter 7: "Senior Brother" → "Young Master"

⚠️ Needs manual handling: 1 issue
- Chapter 12: Time skip needs additional explanation

Fix complete! It is recommended to run /track --check again for verification.
```

### Intelligent Analysis and Suggestions

1.  **Progress Analysis**
    -   Compare planned progress with actual progress
    -   Predict completion time
    -   Identify writing bottlenecks

2.  **Content Analysis**
    -   Foreshadowing coverage (set up/resolved)
    -   Character appearance frequency
    -   Conflict intensity curve

3.  **Actionable Suggestions**
    Based on the analysis, provide:
    -   Next writing priorities
    -   Foreshadowing to be addressed
    -   Relationship lines to be strengthened
    -   Timeline adjustment suggestions

### Visualized Report

Generate a structured report:
```
📊 Comprehensive Tracking Report
━━━━━━━━━━━━━━━━━━━━━━━━━━
[Progress Bar] ████████░░░░░░░ 25%

🎯 Next Step Suggestions:
1. Resolve the "Ancient Bronze Mirror" foreshadowing before Chapter 65
2. Strengthen the direct conflict between the protagonist and the antagonist
3. Supplement the timeline details for Volume 2

⚠️ Needs Attention:
- Character B has not appeared for 5 chapters
- Subplot progress is lagging
- The time skip in Chapter 45 needs explanation
```

### Data Export

Supports exporting tracking data as:
- A full report in Markdown format
- Raw data in JSON format
- Visual charts (relationship graphs, timelines)
