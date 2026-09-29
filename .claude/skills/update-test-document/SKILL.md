---
name: update-test-document
description: Modify a test document (test plan, test cases, or test notes) and record the change in CHANGELOG.md. Use when the user asks to update, edit, or revise a test document, add or remove test cases, or change a test plan, and the change should be logged in the changelog.
allowed-tools: Read, Edit, Write, Glob, Grep, Bash(git diff:*), Bash(git status:*)
---

# Update Test Document

Make the requested change to a test document, then add a matching entry to `CHANGELOG.md`.

## 1. Find the test document

- If the user gave a path, use it.
- Otherwise search for likely candidates (`**/*test*.md`, `**/test-plan*`, `docs/**/test*`) and pick the one that matches the request. If more than one fits, ask which one.
- If none exists and the user wants a new one, create it at the path they name, or `docs/test-document.md` by default.

## 2. Modify the document

- Read the whole document before editing it.
- Make only the change that was asked for. Keep the existing headings, numbering, table layout and wording style.
- When adding test cases, follow the format of the existing ones: ID, title, preconditions, steps, expected result. Continue the existing ID sequence.
- When removing or renaming test cases, update any references to them elsewhere in the document.

## 3. Update the changelog

- Open `CHANGELOG.md` at the repository root. If it doesn't exist, create it with this header:

  ```markdown
  # Changelog

  All notable changes to this project are documented in this file.
  The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

  ## [Unreleased]
  ```

- Add one bullet under `## [Unreleased]`, in the right subsection (`### Added`, `### Changed` or `### Removed`). Create the subsection if it's missing.
- The bullet names the document and says what changed, for example:
  `- Test document (docs/test-document.md): added TC-014 covering login with an expired password.`
- Don't rewrite or reorder existing changelog entries.

## 4. Report

Run `git diff` and summarize both changes for the user: what changed in the test document and the changelog line that was added.
