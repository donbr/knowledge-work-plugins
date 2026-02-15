# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a collection of **plugins** for Claude Cowork and Claude Code. Plugins extend Claude's capabilities with domain-specific skills, slash commands, and external tool integrations via MCP servers. Each plugin targets a specific job function (sales, data, legal, bio-research, etc.).

## Plugin Architecture

Every plugin follows this structure:

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json           # Required manifest (name, version, description)
├── commands/                  # Slash commands (*.md with YAML frontmatter)
├── skills/                    # Domain knowledge (subdirectories with SKILL.md)
│   └── skill-name/
│       ├── SKILL.md           # Main skill file
│       └── references/        # Detailed reference material
├── .mcp.json                  # MCP server connections
├── CONNECTORS.md              # Documents tool placeholders
└── README.md
```

**Key conventions:**
- All files are markdown and JSON — no code, no build steps
- Use kebab-case for directory and file names
- Skills use progressive disclosure: lean SKILL.md (under 3,000 words), detailed content in `references/`
- Commands are instructions FOR Claude, written as directives
- Skill frontmatter descriptions must include specific trigger phrases users would say

## Component Types

| Component | Location | Purpose |
|-----------|----------|---------|
| **Skills** | `skills/*/SKILL.md` | Domain expertise Claude loads on-demand |
| **Commands** | `commands/*.md` | User-initiated slash commands (e.g., `/sales:call-summary`) |
| **MCP Servers** | `.mcp.json` | External service integrations |

## MCP Server Configuration

`.mcp.json` defines external tool connections:

```json
{
  "mcpServers": {
    "service-name": {
      "type": "http",
      "url": "https://mcp.service.com/mcp"
    }
  }
}
```

## Placeholder Pattern (`~~`)

Plugins intended for distribution use `~~category` placeholders to be tool-agnostic:
- `~~chat` for Slack/Teams/Discord
- `~~project tracker` for Linear/Asana/Jira
- `~~CRM` for HubSpot/Salesforce/Close

Document all placeholders in `CONNECTORS.md`.

## Skill File Format

Skills have YAML frontmatter with `name` and `description`, then markdown body:

```markdown
---
name: skill-name
description: >
  Brief description with trigger phrases like "prep me for my call with [company]"
---

# Skill Name

[Workflow instructions, output formats, execution steps]
```

## Command File Format

Commands have YAML frontmatter with `description` and optional `argument-hint`:

```markdown
---
description: Brief description of what the command does
argument-hint: "<required input>"
---

# /command-name

[Instructions for Claude to follow when command is invoked]
```

## Creating New Plugins

Use the `cowork-plugin-management` plugin which contains the `create-cowork-plugin` skill with full component schemas and a guided five-phase workflow.

## Validating Plugins

```bash
claude plugin validate path/to/plugin-name/.claude-plugin/plugin.json
```

## Installing Plugins in Claude Code

```bash
claude plugin marketplace add anthropics/knowledge-work-plugins
claude plugin install plugin-name@knowledge-work-plugins
```
