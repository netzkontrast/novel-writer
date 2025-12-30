# External AI Suggestion Integration - Quick Guide

## 🚀 Integrate Suggestions in Three Steps

### Step 1: Get Suggestions
Copy the prompt below and give it to Gemini/ChatGPT:

```
Analyze the first 10 chapters of my novel and return suggestions in JSON format:
{
  "version": "1.0",
  "source": "Gemini",
  "analysis_date": "2025-01-24",
  "suggestions": {
    "style": {"priority": "high", "items": [list of suggestions]},
    "characters": {"priority": "medium", "items": [list of suggestions]},
    "dialogue": {"priority": "high", "items": [list of suggestions]}
  },
  "key_improvements": ["Improvement 1", "Improvement 2", "Improvement 3"]
}

Novel content: [Paste your chapters here]
```

### Step 2: Copy the Suggestions
Copy the JSON content returned by the AI.

### Step 3: Execute Integration
In Claude Code, run:
```
/constitution refine
[Paste JSON here]
```

## ✅ Done!
The system will automatically:
- Parse the suggestion content
- Update the relevant specification files
- Generate an integration report
- Record it in the improvement history

## 📝 Supported Suggestion Types

| Suggestion Type | Updated File | Example |
|---|---|---|
| Writing Style | writing-constitution.md | Perspective unity, pacing control |
| Character Optimization | character-profiles.md | Personality development, motivation strengthening |
| Dialogue Improvement | character-voices.md | Word choice standards, language characteristics |
| Plot Suggestion | plot-tracker.json | Foreshadowing payoff, conflict setup |
| World-building | world-setting.md | Rule refinement, detail supplementation |

## 🔍 View Integration Results
```bash
# View improvement history
cat spec/knowledge/improvement-log.md

# View updated specifications
cat .specify/memory/writing-constitution.md
```

## 💡 Best Practices

### Suggestion Frequency
- Analyze every 10 chapters.
- Analyze again after major revisions.
- Conduct a comprehensive evaluation periodically (monthly).

### Suggestion Priority
1.  **High Priority**: Implement immediately, as it affects subsequent writing.
2.  **Medium Priority**: Plan to implement, improve gradually.
3.  **Low Priority**: For reference, adopt selectively.

### Batch Processing
You can integrate suggestions from multiple AIs at the same time:
```
/constitution refine --merge
[Gemini's JSON]
---
[ChatGPT's JSON]
```

## ⚠️ Frequently Asked Questions

### Q: JSON format error?
A: Make sure you copy the complete JSON, including all curly braces.

### Q: The suggestions didn't take effect?
A: Check `improvement-log.md` to see if the integration was successful.

### Q: Want to undo a suggestion?
A: Check the git history; you can roll back the relevant files.

## 📚 Detailed Documentation
- [Full Prompt Template](./ai-suggestion-prompt-template.md)
- [Gemini-specific Template](./ai-suggestion-prompt-for-gemini.md)
- [Example Demonstrations](./suggestion-integration-examples.md)
- [PRD Document](./PRD-external-suggestion-integration.md)

---
*Version: 1.0 | Updated: 2025-01-24*
