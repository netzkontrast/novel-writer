---
name: relations
description: Manage and track changes in character relationships
argument-hint: [update | show | history | check] [Character] [Relationship] [Target Character]
allowed-tools: Read(//spec/tracking/relationships.json), Read(spec/tracking/relationships.json), Write(//spec/tracking/relationships.json), Write(spec/tracking/relationships.json), Bash(find:*), Bash(*)
model: claude-sonnet-4-5-20250929
scripts:
  sh: .specify/scripts/bash/manage-relations.sh
  ps: .specify/scripts/powershell/manage-relations.ps1
---

# Character Relationship Management

Tracks and manages the dynamics of relationships between characters to ensure their development is logical.

## Functions

1.  **Relationship Network** - Maintains a relationship graph between characters.
2.  **Relationship Changes** - Records the evolution of relationships.
3.  **Faction Management** - Tracks the conflicts and cooperation between various factions.
4.  **Emotional Tracking** - Manages the emotional development between characters.

## Usage

Execute the script {SCRIPT} [action] [parameters]:
- `update` - Update a character relationship.
- `show` - Display the relationship network.
- `history` - View the history of relationship changes.
- `check` - Validate relationship logic.

Example:
```
{SCRIPT} update Li_Zhongyong allies Shen_Yuqing --chapter 61 --note "Helped upon first entering the Hanlin Academy"
# PowerShell:
{SCRIPT} -Command update -A Li_Zhongyong -Relation allies -B Shen_Yuqing -Chapter 61 -Note "Helped upon first entering the Hanlin Academy"
```

## Data Storage

Relationship data is stored in `spec/tracking/relationships.json`:
```json
{
  "characters": {
    "Protagonist": {
      "allies": ["Character A", "Character B"],
      "enemies": ["Character C"],
      "love_interest": ["Character D"],
      "unknown": ["Character E"]
    }
  },
  "factions": {
    "Reformists": ["Protagonist", "Character A"],
    "Conservatives": ["Character C", "Character F"]
  }
}
```

## Example Output

```
👥 Character Relationship Network
━━━━━━━━━━━━━━━━━━━━
Protagonist: Li Zhongyong
├─ 💕 Love Interest: Shen Yuqing
├─ 🤝 Ally: Zhang Juzheng (hidden)
├─ 📚 Mentor: Matteo Ricci
├─ ⚔️ Enemy: Shen Shixing's faction
└─ 👁️ Monitored by: Eastern Depot

Factional Conflicts:
Reformists ←→ Conservatives
Donglin Party ←→ Eunuch Faction

Recent Changes (Chapter 60):
- Shen Yuqing: Stranger → Mutual Attraction
- Zhang Juzheng: Unknown → Mentor-Student Relationship
```
