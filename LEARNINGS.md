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

## Future Considerations

- Consider adding a linter configuration for markdown files
- The relationship between different note files could be more explicitly documented
- Some notes (like "Tad.md") are personal business notes, others are technical guidelines
