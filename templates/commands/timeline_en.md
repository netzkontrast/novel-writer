---
name: timeline
description: Manage and validate the story timeline
argument-hint: [add | check | show | sync]
allowed-tools: Read(//spec/tracking/timeline.json), Read(spec/tracking/timeline.json), Write(//spec/tracking/timeline.json), Write(spec/tracking/timeline.json), Read(//stories/**/content/**), Read(stories/**/content/**), Bash(find:*), Bash(*)
model: claude-sonnet-4-5-20250929
scripts:
  sh: .specify/scripts/bash/check-timeline.sh
  ps: .specify/scripts/powershell/check-timeline.ps1
---

# Timeline Management

Maintains the story's timeline to ensure chronological consistency.

## Functions

1.  **Time Logging** - Tracks the time point of each chapter.
2.  **Parallel Events** - Manages multiple plotlines happening simultaneously.
3.  **Historical Comparison** - Compares with real historical events (for historical novels).
4.  **Logical Validation** - Checks the reasonableness of time spans.

## Usage

Execute the script {SCRIPT}, which supports the following operations:
- `add` - Add a time node.
- `check` - Validate chronological continuity.
- `show` - Display a timeline overview.
- `sync` - Synchronize parallel events.

## Timeline Data

Timeline information is stored in `spec/tracking/timeline.json`:
- In-story time (year/month/day).
- Chapter correspondence.
- Important event markers.
- Time span calculations.

## Example Output

```
📅 Story Timeline
━━━━━━━━━━━━━━━━━━━━
Current Time: Spring of the 30th year of the Wanli Era

Chapter 1  | Winter of the 29th year of Wanli | Transmigration event
Chapter 4  | First month of the 30th year of Wanli | Traveling north for the exam
Chapter 6  | Second month of the 30th year of Wanli | Metropolitan examination
Chapter 8  | Third month of the 30th year of Wanli | Palace examination
Chapter 61 | Fourth month of the 30th year of Wanli | [To be written]

⏱️ Time Span: 5 months
🔄 Parallel Events: Japanese invasion of Korea
```
