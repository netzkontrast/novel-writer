# Novel Translation Plugin

## Introduction

The Novel Translation Plugin is a translation plugin designed for bringing Chinese novels to an international audience. It can intelligently translate Chinese novels into authentic English, suitable for publication on platforms like Medium, Reddit, and Wattpad.

## Features

- 🌍 **Intelligent Localization Translation** - More than just translation, it's cultural adaptation.
- 📚 **Maintains Narrative Style** - Stays faithful to the original while making it easy for English readers to understand.
- 🎯 **Platform Optimization** - Adjusts the translation style for different platforms.
- 🔄 **Batch Processing** - Supports batch translation of an entire novel.
- ✨ **Professional Terminology Handling** - Intelligently handles names, places, and proper nouns.

## Installation

Ensure you have novel-writer-cn installed:

```bash
npm install -g novel-writer-cn
```

The plugin will be automatically installed with the main program.

## Usage

### 1. Basic Translation Commands

```bash
# Translate a single chapter
/translate-chapter

# Batch translate
/translate-batch

# Translate and optimize for Medium style
/translate-medium

# Translate and optimize for Reddit style
/translate-reddit
```

### 2. Preparation

Before using the translation function, please prepare:

1. **Original Text File** - Supports Chinese novel files in .txt or .md format.
2. **Chapter Division** - It is recommended to have each chapter in a separate file for easy management.
3. **List of Names and Places** - Optional, used to unify the translation of proper nouns.

### 3. Usage Example

#### Translate a Single Chapter

```bash
# In the novel project directory
novel translate-chapter

# The system will prompt:
# 1. Select the chapter file to be translated.
# 2. Select the target platform (Medium/Reddit/General).
# 3. Confirm the translation settings.
```

#### Batch Translation

```bash
# Translate the entire novel
novel translate-batch

# The system will:
# 1. Scan all chapter files.
# 2. Translate them in order.
# 3. Save them to the translated/ directory.
```

### 4. Advanced Features

#### Expert Mode

For in-depth optimization, you can enable the translation expert mode:

```bash
# Enable expert guidance
/translate-expert

# The expert will provide:
# - Suggestions for handling cultural differences.
# - Localization of idioms and slang.
# - Analysis of the target audience.
# - SEO optimization suggestions.
```

#### Terminology Management

Create a `translation-glossary.md` file to manage proper nouns:

```markdown
# Translation Glossary

## Names
- 林天 -> Lin Tian
- 苏小小 -> Su Xiaoxiao

## Places
- 青云山 -> Azure Cloud Mountain
- 万剑宗 -> Ten Thousand Swords Sect

## Techniques/Skills
- 九天雷诀 -> Nine Heavens Thunder Art
- 破天一剑 -> Sky-Breaking Sword Strike
```

### 5. Output Format

The translated files will be saved in the `translated/` directory:

```
project/
├── chapters/          # Original chapters
├── translated/        # Translation results
│   ├── chapter_01_en.md
│   ├── chapter_02_en.md
│   └── ...
└── translation-glossary.md  # Glossary
```

## Platform Adaptation Notes

### Medium Style
- Concise and clear language.
- A narrative pace suitable for Western readers.
- Emphasizes story and readability.

### Reddit Style
- More colloquial.
- Retains the "cool" features of web novels.
- Appropriately incorporates pop culture references.

### Wattpad Style
- Teen-reader friendly.
- Delicate emotional descriptions.
- Sets up a cliffhanger at the end of the chapter.

## Frequently Asked Questions

### Q: What if the translation speed is slow?
A: Quality comes first. It is recommended to process in batches, 5-10 chapters at a time.

### Q: The translation of proper nouns is inconsistent?
A: Use the glossary feature to uniformly manage the translation of proper nouns.

### Q: How to handle culturally specific content?
A: Enable expert mode to get localization suggestions, or add translator's notes.

### Q: Does it support other languages?
A: Currently, we are focusing on Chinese-to-English translation. Other languages are in the planning stage.

## Best Practices

1. **Translate by Chapter** - Process 5-10 chapters at a time for easy quality control.
2. **Terminology First** - Establish a glossary first to ensure consistency.
3. **Platform Customization** - Choose the appropriate style based on the publication platform.
4. **Manual Review** - It is recommended to have a human review important chapters after the AI translation.
5. **Reader Feedback** - Continuously optimize based on feedback from overseas readers.

## Technical Notes

- Based on the deep understanding capabilities of Claude AI.
- Context-aware translation strategy.
- Maintains the flavor of the original text while adapting to the target language and culture.

## Changelog

### v1.0.0 (2025-09-22)
- Initial release
- Supports Chinese-to-English translation
- Four translation commands
- Expert mode
- Terminology management function

## Support

If you encounter any problems, please submit an issue:
https://github.com/wordflowlab/novel-writer/issues

## License

MIT License
