---
name: edit-test-docx
description: Edit the Word document at C:\Desktop\Claude\Test.docx and record the change in CHANGELOG.md. Use when the user asks to edit, update, modify, or add to Test.docx, "the test doc", or "the Word test document".
allowed-tools: Read, Edit, Bash(python:*), Bash(python3:*), Bash(py:*), Bash(pip install python-docx), Bash(cp:*), Bash(ls:*), Bash(git diff:*)
---

# Edit Test.docx

Target file: `C:\Desktop\Claude\Test.docx`. In Git Bash or WSL use `/c/Desktop/Claude/Test.docx` or `/mnt/c/Desktop/Claude/Test.docx`.

## 1. Check the file

- Confirm the file exists. If it doesn't, stop and tell the user. Don't create a new document unless they ask for one.
- If `C:\Desktop\Claude\~$Test.docx` exists, the document is open in Word. Ask the user to close it before editing, or the save will fail or be overwritten.
- Make a backup first: copy it to `Test.backup.docx` in the same folder.

## 2. Read the document

A `.docx` file is a zip of XML, so the Read tool can't open it directly. Use python-docx (`pip install python-docx` if it's missing):

```python
from docx import Document
doc = Document(r"C:\Desktop\Claude\Test.docx")
for i, p in enumerate(doc.paragraphs):
    print(i, p.style.name, "|", p.text)
for t, table in enumerate(doc.tables):
    for r, row in enumerate(table.rows):
        print("table", t, "row", r, [c.text for c in row.cells])
```

## 3. Make the change

- Change only what the user asked for.
- Keep the existing formatting. Edit text in place through the paragraph's runs rather than replacing the whole paragraph, and give new paragraphs the same style as their neighbours (for example `doc.add_paragraph(text, style=existing.style)`, or insert after an existing paragraph).
- For tables, add rows by copying an existing row's format, and fill in every cell.
- Save back to the same path: `doc.save(r"C:\Desktop\Claude\Test.docx")`.

## 4. Verify

Re-open the saved file with the script from step 2 and confirm the change is there and nothing else moved. If something went wrong, restore `Test.backup.docx` and tell the user.

## 5. Update the changelog

- Open `CHANGELOG.md` at the repository root. If it doesn't exist, create it with this header:

  ```markdown
  # Changelog

  All notable changes to this project are documented in this file.
  The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

  ## [Unreleased]
  ```

- Add one bullet under `## [Unreleased]` in the right subsection (`### Added`, `### Changed` or `### Removed`), creating the subsection if needed. Example:
  `- Test.docx: added a "Regression tests" section with 3 test cases.`
- Don't rewrite or reorder existing entries.

## 6. Report

Tell the user what changed in Test.docx, where the backup is, and the changelog line you added.
