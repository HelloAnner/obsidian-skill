---
name: obsidian
description: >
  Operate the user's Obsidian vault from the terminal. Read notes, enrich content
  using context_dir background material, generate Excalidraw diagrams, manage
  frontmatter properties, and publish to KMS/Feishu/Notion/GitHub via obsidian CLI.
  Use when the user asks to work with their notes, create or edit Obsidian files,
  generate diagrams, or publish content.
---

# Obsidian Vault Skill

Operate the Obsidian vault at `/Users/anner/notes` via terminal. The vault name is `Work`.

## Personal Preferences — `self/`

**Before creating or placing any note, read all `.md` files in the `self/` directory.** This folder contains the vault owner's personal note-taking preferences: directory structure, naming conventions, topic routing rules, and more. These preferences are the ground truth for where and how to organize notes.

- `self/` is gitignored — it holds private configuration, not shared with the skill repo
- If you cannot determine the correct location for a note, check the latest vault directory structure first (`ls` the vault), then consult `self/` preferences
- When the vault structure changes (new folders, reorganized topics), update the relevant files in `self/`

## Obsidian CLI

The `obsidian` CLI binary is at:

```
/Volumes/Macintosh\ HD/Applications/Obsidian.app/Contents/MacOS/obsidian
```

Requires Obsidian to be running. Run `obsidian help` for the latest command list. See [obsidian-cli.md](references/obsidian-cli.md) for full reference.

## Vault Layout

```
/Users/anner/notes/
├── Work/
│   ├── Fine AI/                  # AI product development
│   │   ├── Moss/                 # Moss agent platform
│   │   │   ├── 开发文档/         # Dev docs (with context_dir, kms, feishu)
│   │   │   ├── 会议记录/         # Meeting notes
│   │   │   └── 反思/             # Reflections
│   │   └── ...
│   ├── Fine 工作日报/             # Daily work logs
│   ├── Fine 技术研究/             # Technical research
│   ├── Fine 模块分析/             # Module analysis
│   ├── Fine 客户问题/             # Customer issues
│   ├── Templates/                # Note templates
│   ├── Excalidraw/               # Excalidraw diagrams
│   ├── attachments/              # Shared attachments
│   └── ...
```

Each note folder may have its own `attachments/` subdirectory for local images and files.

## Core Workflows

### 1. Write & Edit Notes

Use [Obsidian Flavored Markdown](references/obsidian-markdown.md) with wikilinks, embeds, callouts, and YAML frontmatter. Reference: [callouts](references/callouts.md), [embeds](references/embeds.md).

### 2. Manage Properties

Every note has YAML frontmatter with platform publish targets and workflow metadata. See [vault-properties.md](references/vault-properties.md) for the full property system.

Key properties:

- `kms` / `feishu` / `notion` / `github` — publish target URLs
- `kms_url` / `feishu_url` / `notion_url` — auto-filled result URLs
- `context_dir` — local directory with source material for this note
- `readme` — README generation pointer

### 3. Enrich Notes from Context — `context_dir` is Critical

**When a note has a `context_dir` property, you MUST explore that directory before working on the note.** This directory is the primary information source — the note is a condensed product of that context. Without reading it, you lack the foundation to write, edit, or enrich the note accurately.

Workflow: check for `CLAUDE.md` in `context_dir` first (fastest way to grasp the big picture) → explore the code/docs/architecture → then synthesize into the note. See [context-enrichment.md](references/context-enrichment.md).

### 4. Generate Diagrams

Use the local `excalidraw-diagram` skill (`~/.claude/skills/excalidraw-diagram-skill/`) as the primary tool for Excalidraw generation. Fall back to Mermaid for simple inline diagrams. See [excalidraw.md](references/excalidraw.md).

### 5. Publish

Trigger the `obsidian-publish-everywhere` plugin via CLI to publish notes to KMS, Feishu, Notion, GitHub. See [publish-workflow.md](references/publish-workflow.md).

### 6. Use Templates

Match new notes to the appropriate template from `Work/Templates/`. See [note-templates.md](references/note-templates.md).

### 7. Bases & Formulas

Create `.base` files for database-like views. See [bases-formulas.md](references/bases-formulas.md).

## Key Conventions

- **Internal links**: Always use `[[wikilinks]]` for vault notes, `[text](url)` for external URLs only
- **Embeds**: `![[Note Name]]`, `![[image.png|600]]`, `![[diagram.excalidraw]]`
- **Frontmatter**: Every note must have YAML frontmatter (`---` delimited)
- **Attachments**: Place in the note's local `attachments/` folder
- **Language**: Note content follows the language of the existing note (Chinese for work notes, English for technical docs as appropriate)
- **Note placement**: Consult `self/` preferences for topic routing and naming conventions before creating notes
