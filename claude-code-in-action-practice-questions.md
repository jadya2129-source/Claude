# Claude Code in Action: Practice Questions

20 scenario-based practice questions for Claude Architect certification prep. Together they cover every module of the **Claude Code in Action** course.

Each question has one best answer. The answer and explanation sit in a collapsible block, so try the question before you open it.

| Module | Questions |
| --- | --- |
| 1. What is a coding assistant? | 1–2 |
| 2. Project setup and adding context (`/init`, `CLAUDE.md`, `@` mentions) | 3–6 |
| 3. Making changes (screenshots, Plan Mode, thinking) | 7–9 |
| 4. Controlling context (Escape, rewind, `/compact`, `/clear`) | 10–11 |
| 5. Custom commands | 12–13 |
| 6. MCP servers | 14–15 |
| 7. GitHub integration | 16 |
| 8. Hooks | 17–19 |
| 9. The Claude Code SDK | 20 |

---

## Module 1: What is a coding assistant?

### Question 1

A teammate says: "Claude can't read our repo. It's a language model, so it only sees the text we paste into it." You're explaining how Claude Code actually works on a codebase. What is the most accurate explanation?

- **A.** Claude Code uploads the whole repository to the model in one request when it starts.
- **B.** Claude Code fine-tunes a copy of the model on your repository the first time you run it.
- **C.** The model asks for *tools* (read a file, search, run a command). Claude Code runs each tool and returns the result to the model. The loop repeats until the task is done.
- **D.** Claude Code only works on files you name explicitly with `@`.

<details>
<summary>Answer</summary>

**C.** A language model can only take in text and produce text. A coding assistant gives it tools. The model says which tool it wants, the assistant runs it, and the result goes back into the conversation. This loop is how Claude Code explores a codebase, edits files and runs tests. There is no bulk upload (A) and no fine-tuning (B). `@` mentions are optional (D).

</details>

### Question 2

Your team is comparing coding assistants. One engineer asks why Claude Code copes well with tasks it has never seen, such as a brand-new internal CLI. Which factor matters most?

- **A.** The model is good at using tools. It can combine general tools such as reading files, running shell commands and reading output to work out an unfamiliar system, and it can be given new tools.
- **B.** Claude Code has a larger list of built-in integrations than other tools.
- **C.** Claude Code keeps a searchable index of every public CLI.
- **D.** Claude Code refuses tasks it hasn't been trained on.

<details>
<summary>Answer</summary>

**A.** How well an assistant works depends heavily on how well the model uses tools. A model that is strong at tool use can run `--help`, read the source and try commands to understand something new. You can also extend it with more tools, for example through MCP servers.

</details>

---

## Module 2: Project setup and adding context

### Question 3

You open Claude Code in a large, unfamiliar monorepo for the first time. You want Claude to know the architecture, the build and test commands and the important files in every future session. What should you do first?

- **A.** Paste the README into the chat at the start of every session.
- **B.** Run `/compact` to shrink the repo so it fits in context.
- **C.** Add every source file with `@` mentions.
- **D.** Run `/init`. Claude analyzes the codebase and writes a `CLAUDE.md` summary that is loaded into every request.

<details>
<summary>Answer</summary>

**D.** `/init` has Claude explore the project and write a `CLAUDE.md` file with an overview, the key commands and the architecture. Claude Code includes `CLAUDE.md` in the context of every request, so you don't have to re-explain the project each session.

</details>

### Question 4

You want your personal preference, "always explain changes in bullet points", to apply only to you, on this project. Your teammates shouldn't see it and it shouldn't be committed. Which file should hold it?

- **A.** `CLAUDE.md` at the project root
- **B.** `CLAUDE.local.md` at the project root
- **C.** `~/.claude/CLAUDE.md`
- **D.** `.claude/settings.json`

<details>
<summary>Answer</summary>

**B.** There are three memory levels:
- `CLAUDE.md`: shared with the team and committed to source control.
- `CLAUDE.local.md`: personal to you for this project and not committed.
- `~/.claude/CLAUDE.md`: personal to you across **all** projects.

Option C would apply the preference to every project. `settings.json` holds configuration such as permissions and hooks, not instructions written for Claude.

</details>

### Question 5

Claude keeps getting the database schema wrong because it doesn't read the schema file before writing queries. The schema file is essential for almost every task. What is the most efficient fix?

- **A.** Paste the schema into the chat whenever Claude gets it wrong.
- **B.** Run `/clear` before each task.
- **C.** Switch on Plan Mode permanently.
- **D.** Reference the file from `CLAUDE.md` with an `@` mention, for example `The database schema is defined in @prisma/schema.prisma`, so it is included in every request.

<details>
<summary>Answer</summary>

**D.** An `@` file mention inside `CLAUDE.md` puts that file's contents into context automatically on every request. It's the right tool for a small number of files that almost every task needs. Don't overuse it: every mentioned file costs context on every request.

</details>

### Question 6

Halfway through a session, you notice Claude keeps using `npm` when your project uses `pnpm`. You want to save this correction in memory quickly without leaving the conversation. What should you do?

- **A.** Use memory mode: type a message starting with `#` (for example `# Always use pnpm, never npm`) and choose which `CLAUDE.md` file to save it to.
- **B.** Start a new session and edit `CLAUDE.md` by hand.
- **C.** Type `/compact use pnpm`.
- **D.** Press Escape twice.

<details>
<summary>Answer</summary>

**A.** A message starting with `#` enters memory mode. Claude merges the instruction into the `CLAUDE.md` file you choose (project, local or user), so the correction lasts beyond this session.

</details>

---

## Module 3: Making changes

### Question 7

A designer sends you a screenshot of a misaligned layout in the app's settings page and asks you to "make it look like the mockup." What is the most effective way to give Claude this information?

- **A.** Describe the misalignment in words as precisely as you can.
- **B.** Commit the screenshot to the repo and run `/init` again.
- **C.** Paste the screenshot straight into Claude Code (with Ctrl+V, including on macOS) and ask Claude to fix the issue shown.
- **D.** Screenshots aren't supported, so export the page's HTML instead.

<details>
<summary>Answer</summary>

**C.** Claude Code accepts pasted images. A screenshot tells Claude exactly which UI element you mean and what's wrong with it, which is often clearer than a written description.

</details>

### Question 8

You need a refactor that touches authentication, session handling and three API routes across many files. You want Claude to research the codebase and show you its approach **before** it edits anything. What should you use?

- **A.** Ask Claude to "ultrathink" and let it edit right away.
- **B.** Plan Mode (press Shift+Tab twice). Claude explores the relevant files and presents a plan for you to approve before making changes.
- **C.** `/compact` before each file.
- **D.** A PostToolUse hook that blocks all edits.

<details>
<summary>Answer</summary>

**B.** Plan Mode is designed for **wide** tasks: changes that span many parts of the codebase, where you want Claude to explore widely and agree an approach with you first. Thinking modes such as "think" and "ultrathink" give Claude more reasoning for **deep** problems. They don't add a review step before edits.

</details>

### Question 9

Claude is debugging a subtle race condition that happens in only one code path. The bug is narrow but hard to reason about. Which approach fits best?

- **A.** Plan Mode, because the bug is in one place.
- **B.** `/clear` after each attempt.
- **C.** A thinking mode, by adding a phrase such as "think hard" or "ultrathink" to the prompt so Claude gets a larger reasoning budget for a difficult, focused problem.
- **D.** Add the whole repo to `CLAUDE.md` with `@` mentions.

<details>
<summary>Answer</summary>

**C.** As a rule of thumb, Plan Mode handles **breadth** (many files, understanding the codebase) and thinking modes handle **depth** (complex logic, tricky bugs). You can use both together on a task that is both wide and deep. Keep in mind that both use more tokens.

</details>

---

## Module 4: Controlling context

### Question 10

Claude has spent several turns stuck on a failed approach, and the conversation is now full of irrelevant back-and-forth. You want to jump back to an earlier point, before the detour, while keeping the useful context from before it. What should you do?

- **A.** Press Escape twice to rewind the conversation to an earlier message.
- **B.** Press Escape once.
- **C.** Run `/clear`.
- **D.** Close Claude Code and start again.

<details>
<summary>Answer</summary>

**A.** Pressing Escape twice shows your earlier messages and lets you rewind to one of them. The distracting detour is dropped and the useful earlier context stays. Pressing Escape once (B) only interrupts Claude's current response. `/clear` (C) erases everything.

</details>

### Question 11

You've finished a long session building a feature. Claude has learned a lot of useful details about the module. You now want to work on a related follow-up task in the same area, but the conversation is very long. Which command fits best?

- **A.** `/clear`, because it removes all history so the next task starts clean.
- **B.** Press Escape twice and choose the first message.
- **C.** `/init`, because it rebuilds `CLAUDE.md`.
- **D.** `/compact`, because it summarizes the conversation so far and keeps what Claude learned while freeing up context.

<details>
<summary>Answer</summary>

**D.** `/compact` swaps the full history for a summary, keeping what Claude learned while using far less context. Use `/clear` when you're moving to an **unrelated** task and the old context would only get in the way.

</details>

---

## Module 5: Custom commands

### Question 12

Your team runs the same multi-step "audit dependencies, update vulnerable packages, run tests" routine every week. You want any teammate to start it with a slash command. How do you create it?

- **A.** Add an `audit` entry to `~/.claude/settings.json`.
- **B.** Create a markdown file at `.claude/commands/audit.md` that holds the instructions. It becomes available as `/audit`.
- **C.** Write a PreToolUse hook named `audit`.
- **D.** Add a line to `CLAUDE.md` that says "when I say audit, do X".

<details>
<summary>Answer</summary>

**B.** Custom commands are markdown files in `.claude/commands/`. The file name becomes the command name, so `audit.md` becomes `/audit`. Committing the folder shares the command with the team. You may need to restart Claude Code before a new command shows up.

</details>

### Question 13

You want a `/write_tests` command that works on whichever file the user names, for example `/write_tests src/utils/date.ts`. How should the command file take that input?

- **A.** Use the `$ARGUMENTS` placeholder in the command's markdown. Whatever follows the command name replaces it.
- **B.** Commands can't take input, so write a separate command for each file.
- **C.** Read the value from an environment variable called `CLAUDE_INPUT`.
- **D.** Use a hook to capture the text the user types.

<details>
<summary>Answer</summary>

**A.** `$ARGUMENTS` in a command file is replaced by the text typed after the command. The argument can be a file path or any other text, which makes one command reusable in many situations.

</details>

---

## Module 6: MCP servers

### Question 14

You want Claude to open your locally running web app in a real browser, take screenshots and check its own UI changes. What is the right approach?

- **A.** Ask Claude to imagine what the page looks like.
- **B.** Write a custom command called `/browser`.
- **C.** Add a browser-automation MCP server, for example with `claude mcp add playwright npx @playwright/mcp@latest`. This gives Claude new tools to control a browser.
- **D.** Put the page's URL in `CLAUDE.md`.

<details>
<summary>Answer</summary>

**C.** MCP (Model Context Protocol) servers give Claude Code new tools. A Playwright MCP server lets Claude navigate pages, click, take screenshots and check the results, so it can improve its UI work based on what actually renders. Custom commands (B) only package prompts. They don't add capabilities.

</details>

### Question 15

After you add the Playwright MCP server, Claude asks for permission every time it uses one of its tools, which slows you down. You trust this server. What is the recommended way to stop the prompts?

- **A.** Remove the server and add it again.
- **B.** Write a hook that exits with code 2.
- **C.** Run Claude Code with `/compact`.
- **D.** Add the server's tools to the `allow` list in `.claude/settings.local.json`, for example `"mcp__playwright"`.

<details>
<summary>Answer</summary>

**D.** MCP tools are named `mcp__<server>__<tool>`. Adding `mcp__playwright` to `permissions.allow` pre-approves all of that server's tools. Using `settings.local.json` keeps the choice personal. Put it in the shared `.claude/settings.json` only if the whole team should get it. Exit code 2 (B) *blocks* tools. It doesn't approve them.

</details>

---

## Module 7: GitHub integration

### Question 16

You've set up the Claude Code GitHub integration with `/install-github-app`. You extend the workflow so that, when someone mentions `@claude` on a PR, Claude uses a Playwright MCP server to test the preview deployment. The workflow runs, but Claude never uses the Playwright tools. What is the most likely cause?

- **A.** GitHub Actions can't run MCP servers.
- **B.** In the Action, every tool Claude may use, MCP tools included, must be listed explicitly in the workflow's allowed-tools configuration. The Playwright tools were never allowed.
- **C.** `@claude` mentions only work on issues, not PRs.
- **D.** The `CLAUDE.md` file is too long.

<details>
<summary>Answer</summary>

**B.** The GitHub Action runs without anyone there to approve permission prompts. You configure the MCP server and **explicitly allow** each tool Claude may use, for example `mcp__playwright__browser_navigate`. Tools that aren't allowed are unavailable to Claude. The integration supports `@claude` mentions on both issues and PRs, and it also supports automatic PR reviews.

</details>

---

## Module 8: Hooks

### Question 17

The `.env` file holds production secrets. You want to make sure Claude can **never** read it, whatever the prompt says. Which configuration does that?

- **A.** A line in `CLAUDE.md` saying "Never read .env".
- **B.** A **PostToolUse** hook that deletes `.env` after it has been read.
- **C.** A custom command called `/no-env`.
- **D.** A **PreToolUse** hook with a matcher such as `Read|Grep`. It inspects the file path in the tool input and, if the path points to `.env`, writes an explanation to stderr and exits with code **2** to block the call.

<details>
<summary>Answer</summary>

**D.** PreToolUse hooks run **before** a tool call and can block it. When a PreToolUse hook exits with code 2, the call is stopped and the hook's stderr is sent back to Claude as feedback. A PostToolUse hook (B) runs after the tool has already run, so it's too late to stop the read. `CLAUDE.md` instructions (A) are guidance Claude usually follows, not something the system enforces.

</details>

### Question 18

Claude often edits TypeScript files in ways that break type-checking elsewhere in the project. You want Claude to find out about new type errors right after each edit so it can fix them. Which hook design fits best?

- **A.** A PostToolUse hook on `Edit|Write|MultiEdit` that runs `tsc --noEmit` and sends any errors back to Claude.
- **B.** A PreToolUse hook on `Edit|Write` that runs `tsc --noEmit`.
- **C.** A PreToolUse hook that blocks every edit to `.ts` files.
- **D.** A custom command `/typecheck` that the user runs by hand at the end.

<details>
<summary>Answer</summary>

**A.** PostToolUse hooks run **after** a tool completes, so the code being checked includes the change. Sending the type errors back to Claude lets it fix call sites that broke, for example after a function signature changed. A PreToolUse hook (B) would type-check the code before the edit is applied, which misses the new errors.

</details>

### Question 19

You're writing a hook script and need to know which tool Claude is about to call and what arguments it passes. Where does your script get that information?

- **A.** From the `CLAUDE_TOOL` environment variable.
- **B.** From command-line arguments `$1` and `$2`.
- **C.** From JSON on **stdin**, which includes fields such as `tool_name` and `tool_input` (for example `tool_input.file_path`).
- **D.** Hooks can't see tool arguments. They only get the tool's name.

<details>
<summary>Answer</summary>

**C.** Claude Code sends the hook script JSON on standard input that describes the tool call, including `tool_name` and `tool_input`. The script parses it, for example with `jq` or `JSON.parse`, and decides what to do. Also remember where hooks are configured: in the settings files, which are global `~/.claude/settings.json`, shared project `.claude/settings.json` and personal project `.claude/settings.local.json`. The course recommends absolute paths for hook scripts for security.

</details>

---

## Module 9: The Claude Code SDK

### Question 20

You're building a CI step that uses Claude Code from a TypeScript script to review each diff and write a report. It must not change any files. What is the right approach?

- **A.** Use the Claude Code SDK to run Claude Code from the script. It uses the same tools as the terminal version and is **read-only by default**. Enable write tools such as `Edit` only by listing them in `allowedTools` if you ever need them.
- **B.** Use the SDK and add `"Edit"` to `allowedTools` so Claude can write the report into the source files.
- **C.** The SDK can't be used in CI. It only works interactively.
- **D.** Give Claude Code full permissions and add "don't edit anything" to `CLAUDE.md`.

<details>
<summary>Answer</summary>

**A.** The Claude Code SDK (available for the CLI, TypeScript and Python) runs Claude Code from code, which suits pipelines, hooks and other automation. By default it has read-only permissions. It can read files, search and analyze, but it can't write until you allow tools such as `Edit` through the `allowedTools` option. For a review-only step, the default is what you want.

</details>

---

## Quick answer key

| # | Answer | # | Answer |
| --- | --- | --- | --- |
| 1 | C | 11 | D |
| 2 | A | 12 | B |
| 3 | D | 13 | A |
| 4 | B | 14 | C |
| 5 | D | 15 | D |
| 6 | A | 16 | B |
| 7 | C | 17 | D |
| 8 | B | 18 | A |
| 9 | C | 19 | C |
| 10 | A | 20 | A |
