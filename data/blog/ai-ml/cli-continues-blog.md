---
title: 'Resume AI Coding Sessions Across Any CLI - cli-continues'
date: '2026-02-23'
tags: ['ai', 'developer-tools', 'cli', 'productivity', 'open-source']
draft: false
summary: 'cli-continues lets developers resume AI coding sessions across different CLI tools without losing context. Seamlessly switch assistants while preserving conversation history, file edits, and workflow state.'
images: ['/static/images/cli-continues-card.png']
type: ['Blog']
---

# cli-continues – Resume AI Coding Sessions Across Any CLI

Modern AI-assisted development rarely lives inside a single tool. You might start coding with one assistant, hit a rate limit, then jump to another — only to realize you’ve lost all your context.

That friction is exactly what **cli-continues** aims to eliminate.

**cli-continues** is a lightweight open‑source command-line utility that lets you *continue* your AI coding sessions across differentCLI tools like Claude Code, GitHub Copilot CLI, Gemini CLI, Codex, Cursor Agent, and more — without manually copying prompts or re-explaining your project.

In short: it helps your AI coding context travel with you.

---

## Why cli-continues Exists

If you regularly work with multiple AI coding tools, you’ve probably experienced:

- Interrupted sessions due to rate limits  
- Losing conversation history when switching tools  
- Repeating the same explanations to different models  
- Manually pasting long chat logs or diffs  

cli-continues solves this by extracting structured context from one tool and injecting it into another.

Your workflow stays continuous.

---

## What Gets Transferred

cli-continues doesn’t just move chat messages. It captures:

- Recent conversation history  
- File edits and patches  
- Working directory information  
- Shell commands  
- Tool metadata and reasoning blocks  

This richer context allows the next AI assistant to understand *what you were doing*, not just *what you said*.

---

## Key Features

### Cross-tool session handoff
Resume sessions between multiple AI CLIs without starting over.

### Automatic session discovery
Scans supported tools and finds existing sessions automatically.

### Interactive picker
Choose sessions using a simple terminal UI when multiple matches exist.

### Scriptable mode
List or resume sessions programmatically for automation workflows.

### Minimal setup
Runs instantly with NPX or a single global install.

---

## Getting Started

You can try cli-continues without installing anything:

```bash
npx continues
```

Or install globally:

```bash
npm install -g continues
```

Common commands:

```bash
continues
```

Launch interactive session picker.

```bash
continues list
```

List all detected sessions.

```bash
continues resume <id> --in gemini
```

Resume a specific session in another tool.

---

## How It Works (High Level)

Each supported AI CLI stores session data differently — JSON, SQLite, or custom formats.

cli-continues:

1. Scans those locations
2. Extracts meaningful context
3. Structures it into a transferable format
4. Feeds it into your target tool

While there’s no universal session API yet, cli-continues provides a practical bridge between ecosystems.

---

## Who Is This For?

cli-continues is ideal for:

- Developers using multiple AI assistants
- Teams experimenting with different models
- Anyone hitting rate limits mid-debug
- Power users building agent-based workflows

If continuity matters to your coding flow, this tool pays for itself instantly.

---

## Final Thoughts

AI coding tools are powerful — but fragmented.

cli-continues brings continuity to that fragmentation.

Instead of restarting conversations or rebuilding context, you simply move forward.

It’s a small utility with a big impact on developer productivity.

---

Project repository:
https://github.com/yigitkonur/cli-continues

Feel free to explore, contribute, or adapt it into your own AI workflows.
