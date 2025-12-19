# Project State

**Last Updated**: 2025-12-19

This document captures the current state of the notes repository (current status only, not historical).

## Repository Overview

**Type**: Development notes and guidelines repository
**Purpose**: Document coding standards, architectural patterns, and best practices for AI-assisted web application development

## Documentation Status

### Core Files (Recently Restructured)

- **CLAUDE.md** - AI assistant guidance
- **README.md** - Repository overview
- **Starter.md** - Project setup and coding standards
- **Vibe coding backend.md** - Backend architecture guidelines
- **Vibe coding commit step.md** - Git workflow
- **Things to avoid.md** - UI/UX anti-patterns for AI
- **Todo.md** - Development tasks
- **Prd updates.md** - Product requirements
- **Tad.md** - Business model notes

### Recently Added

- **LEARNINGS.md** - AI learning notes
- **PROJECTSTATE.md** - This file
- **CHANGELOG.md** - Change tracking
- **.claude/** directory - Claude Code configuration
- **markdown-lint.md** - Comprehensive markdown linting guide

### Markdown Linting System (NEW)

- **markdownlint-cli** installed via npm
- **.markdownlint.json** - Linting configuration
- **.markdownlintignore** - Files to exclude
- **npm scripts**: `lint:md` and `lint:md:fix`
- **Claude Code integration**:
  - `/md-fix` command for quick linting
  - `md-fix` skill for automated workflow
  - `pre-commit.sh` hook to check markdown before commits
- **All markdown files** now pass linting with zero errors

## Recent Changes

**2025-12-19 (Latest)**: Completed infrastructure setup tasks for agentsetup configuration. Removed completed infrastructure section from Todo.md.

**2025-12-18**: Added comprehensive markdown linting system with markdownlint-cli, configuration files, AI assistant guide, Claude Code command/skill/hook integration. All markdown files fixed and passing linting.

**2025-12-18**: Restructured all documentation from terse bullet points into well-organized, structured documents with clear headers and expanded explanations.

## Outstanding Tasks

From Todo.md:

- Security: Gate, Semgrep, CodeQL, OWASP ZAP
- Testing: Postman-like API testing tools
- Code Quality: Separate AI for code review

**Completed**:

- ✅ Infrastructure setup (agentsetup .gitignore, symlinks, bin directory)
- ✅ .gitignore (added to ignore node_modules, .claude/settings.local.json)
- ✅ Markdown linting (markdownlint-cli with full integration)

## Repository Philosophy

Defines patterns for:

- Multi-tenant backend architecture
- Message queue-based communication
- AI-friendly UI/UX design
- Simple, maintainable code
- Frontend/backend separation

## Current Workflow

Using "step commit" process for systematic commits with documentation and quality checks.
