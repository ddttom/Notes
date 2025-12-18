# AI Learning Notes

This document captures concepts and patterns that AI assistants struggled to understand during development sessions. This helps improve future AI interactions with this codebase.

## Session: 2025-12-18 - Documentation Restructuring

### Context Understanding

- **Challenge**: The notes were initially very terse and informal, making it difficult to extract complete requirements
- **Learning**: Converting brief notes into structured documentation requires inferring context and expanding on implied requirements
- **Example**: "Toasts, fades, item removal" needed to be expanded into "Toasts and fades - Information disappears before AI can process it"

### Workflow Execution

- **Challenge**: Understanding the step commit workflow when CHANGELOG.md doesn't exist yet
- **Learning**: The workflow should handle missing files gracefully and create them when necessary
- **Resolution**: Creating missing documentation files as part of the workflow

### File Organization

- **Challenge**: Understanding the relationship between informal notes and formal documentation
- **Learning**: This is a notes repository where:
  - Informal notes capture quick thoughts
  - CLAUDE.md provides formal AI guidance
  - README.md provides repository overview
  - Individual notes are expanded from brief bullets into structured documents

## General Patterns

### Documentation Style

- Notes start as brief bullet points
- Get expanded into structured sections with headers
- Should maintain clear hierarchy and formatting
- Avoid changing the fundamental meaning, only clarify and structure

### Git Workflow

- This repository follows the "step commit" process
- Documentation should be updated in sync with code changes
- Attribution is important for AI-assisted changes

## Session: 2025-12-18 - Markdown Linting Implementation

### NPM Package Installation Challenge

- **Challenge**: npm install was not properly installing markdownlint-cli locally despite appearing to succeed
- **Symptom**: `npm install markdownlint-cli` reported "up to date, audited 1 package" but node_modules remained nearly empty
- **Learning**: NPM configuration issues can cause silent failures where packages appear to install but don't actually populate node_modules
- **Resolution**: Installing globally with `npm install -g markdownlint-cli` worked, allowing npm scripts to function correctly
- **Pattern**: When local npm installs fail silently, global installation can be a workaround for development tools

### Markdown Linting in Examples

- **Challenge**: Showing code fence examples (```) within markdown code blocks causes nesting issues
- **Symptom**: Linter reports "MD040/fenced-code-language Fenced code blocks should have a language specified"
- **Learning**: Cannot nest code fences with identical delimiters (three backticks) inside markdown examples
- **Resolution**: Simplified the example to describe the requirement rather than show nested code fences
- **Pattern**: When documenting markdown syntax itself, avoid nesting identical fence types

### Claude Code Integration

- **Challenge**: Understanding the structure of Claude Code commands, skills, and hooks
- **Learning**: Three complementary automation types:
  - **Commands** (.md in .claude/commands/): User-invoked with /command-name, contain instructions
  - **Skills** (.json in .claude/skills/): Automated workflows with structured steps and tool access
  - **Hooks** (.sh in .claude/hooks/): Shell scripts triggered by events (pre-commit, post-tool-use, pre-push)
- **Pattern**: Commands are for user-initiated actions, skills for reusable automated workflows, hooks for event-driven automation

### Auto-fix Workflow Design

- **Challenge**: Designing a workflow that can both fix issues automatically and provide guidance for manual fixes
- **Learning**: Effective linting workflows need multiple steps:
  1. Check if setup exists (graceful degradation)
  2. Run auto-fix first
  3. Verify results
  4. Report detailed status
  5. Provide specific guidance for remaining issues
- **Pattern**: Auto-fix tools should always verify results and provide actionable feedback

## Future Considerations

- ✅ ~~Consider adding a linter configuration for markdown files~~ - COMPLETED: Added markdownlint-cli with full integration
- The relationship between different note files could be more explicitly documented
- Some notes (like "Tad.md") are personal business notes, others are technical guidelines
