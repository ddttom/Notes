# Markdown Linting Guide for AI Assistants

This guide explains how to write markdown files that pass the markdownlint rules configured in this project.

## Quick Reference

Before committing any markdown file, always run:

```bash
npm run lint:md:fix
```

To check for issues without fixing:

```bash
npm run lint:md
```

## Key Rules to Follow

### 1. Blank Lines Around Headings (MD022)

**Always add a blank line before AND after every heading.**

❌ **Wrong:**

```markdown
Some text here.
## Heading
Next paragraph starts immediately.
```

✅ **Correct:**

```markdown
Some text here.

## Heading

Next paragraph starts immediately.
```

### 2. Blank Lines Around Lists (MD032)

**Always add a blank line before AND after every list.**

❌ **Wrong:**

```markdown
Here are some items:
- Item 1
- Item 2
- Item 3
Next paragraph here.
```

✅ **Correct:**

```markdown
Here are some items:

- Item 1
- Item 2
- Item 3

Next paragraph here.
```

### 3. Heading Hierarchy

**Don't skip heading levels. Follow the proper hierarchy: H1 → H2 → H3.**

❌ **Wrong:**

```markdown
# Main Title

### Subsection (skipped H2)
```

✅ **Correct:**

```markdown
# Main Title

## Section

### Subsection
```

### 4. Consistent List Markers

**Use consistent markers for unordered lists. This project prefers `-` (dash).**

❌ **Wrong:**

```markdown
- Item 1
* Item 2
+ Item 3
```

✅ **Correct:**

```markdown
- Item 1
- Item 2
- Item 3
```

## Complete Example

Here's a complete example showing proper markdown formatting:

```markdown
# Document Title

This is the introduction paragraph.

## First Section

This section has some content.

### Subsection

Here's a list of items:

- First item
- Second item
- Third item

After the list, we continue with regular text.

## Second Section

Another section with content.

### Another Subsection

More content here.

#### Deep Subsection

Even deeper content.

## Code Example

Code blocks should have language specified and be surrounded by blank lines.
```

## Disabled Rules in This Project

The following rules are disabled in `.markdownlint.json`:

- **MD013**: Line length limit (disabled - no line length restrictions)
- **MD033**: Inline HTML (allowed - you can use HTML in markdown)
- **MD041**: First line must be H1 (disabled - files can start differently)
- **MD024**: Duplicate headings (allowed if not siblings - same heading names OK in different sections)

## Common Patterns

### Lists Within Lists

When nesting lists, maintain proper indentation:

```markdown
- Top level item
  - Nested item
  - Another nested item
- Another top level item
```

### Lists with Multiple Paragraphs

```markdown
- First item with a long description.

  This is a continuation of the first item.

- Second item
```

### Headings Followed by Lists

Always add blank line between heading and list:

```markdown
## Items

- Item 1
- Item 2
```

### Multiple Lists in Sequence

Add blank lines between different lists:

```markdown
First list:

- Item A
- Item B

Second list:

- Item 1
- Item 2
```

## Integration with Git Workflow

According to the project's step commit workflow, markdown linting should be done at:

1. **Step 2**: Run `npm run lint:md:fix` as part of general linting
2. **Step 7**: Run `npm run lint:md:fix` on modified documentation
3. **Step 12**: Run `npm run lint:md:fix` before final commit

## Troubleshooting

### If Auto-fix Doesn't Work

Some issues can't be auto-fixed. Common manual fixes:

1. **Heading hierarchy**: Manually adjust heading levels
2. **Duplicate headings**: Rename or restructure sections
3. **Complex nesting**: Manually adjust indentation

### Running Linter on Specific Files

```bash
markdownlint path/to/file.md
markdownlint path/to/file.md --fix
```

### Checking Specific Rules

```bash
markdownlint --help
```

## AI Assistant Checklist

When creating or editing markdown files:

- [ ] Add blank line before every heading
- [ ] Add blank line after every heading
- [ ] Add blank line before every list
- [ ] Add blank line after every list
- [ ] Use `-` for unordered lists consistently
- [ ] Follow heading hierarchy (no skipped levels)
- [ ] Run `npm run lint:md:fix` before completing the task
- [ ] Verify with `npm run lint:md` that no errors remain

## Configuration File

The project's markdown linting configuration is in `.markdownlint.json`:

```json
{
  "default": true,
  "MD013": false,
  "MD033": false,
  "MD041": false,
  "MD024": {
    "siblings_only": true
  },
  "MD007": {
    "indent": 2
  }
}
```

This configuration balances readability with practical markdown writing needs.
