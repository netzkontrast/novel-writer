# Novel Analysis Prompt for Gemini

## How to Use
1. Copy the full prompt below.
2. Replace [X] with the actual number of chapters.
3. Paste your novel's content after the prompt.
4. Gemini will return structured suggestions that can be directly used in Novel Writer.

---

## Full Prompt (Copy Directly)

```
I need you to analyze the first 10 chapters of my novel and provide suggestions for improvement. Please strictly follow the JSON format below and do not add any extra explanations.

Return this JSON structure directly (filled with actual content):

{
  "version": "1.0",
  "source": "Gemini",
  "analysis_date": "2025-01-24",
  "chapters_analyzed": {
    "start": 1,
    "end": 10,
    "total_words": "actual word count"
  },
  "suggestions": {
    "style": {
      "priority": "high/medium/low",
      "items": [
        {
          "type": "issue_type",
          "current": "current_issue",
          "suggestion": "specific_improvement_suggestion",
          "examples": ["specific_chapter_location"],
          "impact": "expected_improvement_effect"
        }
      ]
    },
    "characters": {
      "priority": "high/medium/low",
      "items": [
        {
          "character": "character_name",
          "issue": "existing_issue",
          "suggestion": "improvement_suggestion",
          "development_curve": "growth_curve",
          "chapters_affected": ["array_of_affected_chapter_numbers"]
        }
      ]
    },
    "plot": {
      "priority": "high/medium/low",
      "items": [
        {
          "type": "plot_type",
          "location": "chapter_location",
          "status": "current_status",
          "suggestion": "handling_suggestion",
          "importance": "high/medium/low"
        }
      ]
    },
    "worldbuilding": {
      "priority": "high/medium/low",
      "items": [
        {
          "aspect": "worldview_aspect",
          "issue": "issue_description",
          "suggestion": "supplementary_suggestion",
          "reference_chapters": ["array_of_related_chapters"]
        }
      ]
    },
    "dialogue": {
      "priority": "high/medium/low",
      "items": [
        {
          "character": "character_name",
          "issue": "dialogue_issue",
          "suggestion": "improvement_plan",
          "examples": ["issue_example"],
          "alternatives": ["alternative_solution"]
        }
      ]
    }
  },
  "overall_rating": {
    "plot_coherence": 8,
    "character_development": 7,
    "world_building": 6,
    "writing_style": 7,
    "reader_engagement": 8
  },
  "key_improvements": [
    "most_important_improvement_1",
    "most_important_improvement_2",
    "most_important_improvement_3"
  ]
}

Please analyze the following novel content:
[Paste your novel chapters here]
```

## Example Dialogue

### User Input
```
I need you to analyze the first 10 chapters of my novel and provide suggestions for improvement. Please strictly follow the JSON format below and do not add any extra explanations.

[JSON format template...]

Please analyze the following novel content:
Chapter 1: xxx
Chapter 2: xxx
...
```

### What Gemini Should Return
```json
{
  "version": "1.0",
  "source": "Gemini",
  "analysis_date": "2025-01-24",
  "chapters_analyzed": {
    "start": 1,
    "end": 10,
    "total_words": 42000
  },
  "suggestions": {
    "style": {
      "priority": "high",
      "items": [
        {
          "type": "narrative_voice",
          "current": "Inconsistent third-person perspective",
          "suggestion": "Maintain a consistent third-person limited perspective",
          "examples": ["Chapter 3, Paragraph 2", "Beginning of Chapter 7"],
          "impact": "Improves narrative coherence"
        }
      ]
    },
    ...
  }
}
```

## Using the Returned Suggestions

1. Copy the full JSON returned by Gemini.
2. In Claude Code, execute:
```
/constitution refine [paste JSON]
```
3. The system will automatically integrate the suggestions into your creative constitution.

## Important Notes
- Ensure that Gemini returns a pure JSON format.
- If the returned content has extra text, only copy the JSON part.
- It is recommended to analyze every 10 chapters to maintain an iterative improvement process.
