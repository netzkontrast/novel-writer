# Checklists Directory

This directory contains all quality checklist files.

## 📋 Checklist Types

### Specification Quality Type (Issue-Generative)
Validates the quality of the planning documents themselves, similar to "unit tests for requirements":

- **Outline Quality** (`outline-quality.md`) - Checks the completeness, clarity, and consistency of outline.md
- **Character Clarity** (`character-clarity.md`) - Checks the character design documents
- **World Quality** (`world-quality.md`) - Checks the world-building documents
- **Creative Plan Quality** (`plan-quality.md`) - Checks the creative plan documents
- **Foreshadowing Management** (`foreshadowing-quality.md`) - Checks the definition and planning of foreshadowing

### Content Validation Type (Result-Reporting)
Scans written chapters to validate the actual content:

- **World Consistency** (`world-consistency-YYYYMMDD.md`) - Scans chapter content
- **Plot Alignment** (`plot-alignment-YYYYMMDD.md`) - Compares progress against the outline
- **Data Sync** (`data-sync-YYYYMMDD.md`) - Validates tracking data
- **Timeline** (`timeline-YYYYMMDD.md`) - Checks time logic
- **Writing State** (`writing-state-YYYYMMDD.md`) - Checks writing readiness

## 🎯 Naming Conventions

### Specification Quality Type
Fixed filenames, updated by overwriting:
```
[type]-quality.md
```

Examples:
- `outline-quality.md`
- `character-clarity.md`
- `world-quality.md`

### Content Validation Type
Includes a timestamp; a new file is generated each time:
```
[type]-YYYYMMDD.md
```

Examples:
- `world-consistency-20251011.md`
- `plot-alignment-20251011.md`
- `data-sync-20251011.md`

## 📖 Usage

### Generating a Checklist

Use the unified `/checklist` command:

```bash
# Specification quality check (before writing)
/checklist outline-quality
/checklist character-clarity
/checklist world-quality

# Content validation check (after writing)
/checklist world-consistency
/checklist plot-alignment
/checklist data-sync
```

### Checklist Format

All checklists use a unified Markdown format:

```markdown
# [Check Type] Checklist

**Check Time**: 2025-10-11 14:30:00
**Check Target**: [File or Scope]
**Check Dimension**: [Dimension Description]

---

## [Dimension 1]

- [ ] CHK001 Checklist item description [Tag]
- [x] CHK002 Checklist item description [Tag] ✓
- [!] CHK003 Checklist item description [Tag] ⚠️ Issue Found

## Issues Found

### CHK003
**Issue**: Detailed description
**Location**: File:Line Number
**Suggestion**: How to improve

---

## Next Actions

- [ ] Action Item 1
- [ ] Action Item 2
```

### Checkbox States

- `[ ]` - Not checked
- `[x]` - Checked, passed
- `[!]` - Checked, issue found (details required)

## 🔄 Workflow

### Planning Phase (Before Writing)
1. Complete planning documents like outline, characters, and world-building.
2. Run specification quality checklists.
3. Improve documents based on the check results.
4. Ensure planning quality is sufficient before starting to write.

### Writing Phase (During/After Writing)
1. Complete chapter writing.
2. Periodically run content validation checklists.
3. Discover and fix consistency issues.
4. Maintain a history to track improvements.

## 📊 Best Practices

1. **Dual-Assurance Mechanism**
   - Planning phase: Validate document quality.
   - Writing phase: Validate content consistency.

2. **Find and Fix Early**
   - Discover logical loopholes during the outline stage.
   - Avoid discovering problems late in the process.

3. **Regular Checks**
   - Run a content validation check every 5-10 chapters.
   - Run a specification quality check before major plot twists.

4. **Retain History**
   - Content validation checklists are saved by date.
   - Allows tracking the discovery and resolution process of issues.

5. **Clean Up Outdated Files**
   - Periodically clean up old checklists for resolved issues.
   - Keep check records for key milestones.

## 🆘 Troubleshooting

### Checklist Not Generated
- Check if the `/checklist` command is correct.
- Confirm that the relevant scripts have execution permissions.
- Check the error messages.

### All Checklist Items Fail
- Confirm the file path is correct.
- Check if the JSON file format is valid.
- Run the `jq` command to test the JSON file.

### Old Command Migration
Old commands are still available, but migration is recommended:
- `/world-check` → `/checklist world-consistency`
- `/plot-check` → `/checklist plot-alignment`

## 📚 Related Documents

- [Checklist Command Documentation](../../templates/commands/checklist.md)
- [Checklist Template](../../templates/checklist-template.md)
- [Check Scripts](../../scripts/bash/)

---

**Version**: 1.0
**Last Updated**: 2025-10-11
