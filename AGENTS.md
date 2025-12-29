# Agent Instructions for Translation

This document outlines the process for translating all Markdown files in this repository from Chinese to English.

## Objective

The goal is to create an English version of every `.md` file.

## Translation Workflow

1.  **Consult `todo.md`**: Use the `todo.md` file in the root directory as a tracker. Identify an untranslated file (any unchecked item).

2.  **Translate Content**: Read the content of the selected Markdown file and translate it from Chinese to English.

3.  **Preserve Formatting**: It is crucial to maintain the original Markdown formatting, including headers, lists, bold/italic text, links, and code blocks.

4.  **Handle Code Blocks**: Do not translate code snippets, commands, or file paths inside code blocks. However, you should translate any comments within the code blocks to enhance understanding.

5.  **Create New File**: Save the translated content in a **new file**. The new file should have the same name as the original, but with `_en` appended before the `.md` extension.
    -   **Example**: `README.md` should be saved as `README_en.md`.

6.  **Update Tracker**: After creating and saving the translated file, update the `todo.md` file by marking the corresponding file as completed.
    -   **Example**: Change `- [ ] ./README.md` to `- [x] ./README.md`.

7.  **Repeat**: Continue this process until all files listed in `todo.md` have been translated and marked as complete.

## Final Review

After all files have been translated, perform a final review to ensure consistency in terminology and style across all the new `_en.md` files.
