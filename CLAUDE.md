# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a **Claude Code plugin** (`demo-showcase`) that automatically explores a web application, captures screenshots using Playwright, and generates a polished HTML showcase page with descriptions. It is not a standalone app — it extends Claude Code with a `/demo-showcase` slash command.

## Plugin Architecture

```
.claude-plugin/plugin.json    # Plugin manifest (name, version, description)
commands/demo-showcase.md     # Slash command definition (entry point)
skills/app-showcase/
  SKILL.md                    # Detailed skill instructions for the workflow
  references/
    capture-script-template.md  # Playwright script template
    html-template.md            # HTML showcase page template
```

- **Command** (`commands/demo-showcase.md`): Defines the `/demo-showcase` slash command with YAML frontmatter (allowed tools, argument hints). This is the user-facing entry point that orchestrates the full workflow.
- **Skill** (`skills/app-showcase/SKILL.md`): Contains the detailed 7-step workflow (detect app → setup Playwright → explore → capture → describe → assemble → clean up). The command references this skill for guidance.
- **References**: Templates used during generation — a Playwright capture script base and an HTML page template with `{{PLACEHOLDER}}` variables.

## Key Conventions

- All generated output goes into a `demo-showcase/` directory in the target project (not this repo)
- The plugin never modifies the target project's source code — only appends to `.gitignore`
- Screenshots are captured at 1280x800 viewport, PNG format
- Playwright is installed temporarily (`--no-save`) and cleaned up after capture
- The HTML template uses CSS custom properties and is fully self-contained (no external dependencies)
