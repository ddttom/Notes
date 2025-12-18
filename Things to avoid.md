# UI/UX Anti-Patterns for AI Interaction

This document outlines UI/UX patterns that should be avoided when designing for AI agent interaction.

## Avoid These Patterns

### Hidden or Delayed Feedback
- **Toasts and fades** - Information disappears before AI can process it
- **Below-the-fold error messages** - AI may not scroll to find errors
- **Hiding divs until previous div completes** - Creates confusion about page state
- **Error messages only after submit** - Provide validation errors as users go
- **Information hidden in disclaimers** - AI cannot see hidden content

### Animation and Loading Indicators
- **Animations** - Not a good experience for AI agents
- **Floating loaders** - AI cannot interpret loading states
- **Spinners** - Provide unclear feedback to AI agents

### Single Page Application (SPA) Issues
- **Too much SPA** - Makes it difficult for AI to understand page changes
- **Lack of clear response pages** - AI needs clear navigation boundaries
- **Dynamic content without clear indicators** - State changes must be obvious

### Console-Only Information
- **Don't assume AI will read console logs**
- **All important information must be visible in the UI**
- **Error states should be explicitly rendered**

## Best Practices for AI-Friendly UI

### Visibility and Clarity
- Make everything visible and obvious
- Provide immediate, in-context feedback
- Clear response pages instead of pure SPA navigation
- Display all critical information directly in the UI

### Intelligent Adaptation
- **Detect AI bot usage** - Know when you are being used by an AI agent
- **Adjust UI for AI engagement** - "Personalization for robots"
- **Accelerate AI workflows** - Remove unnecessary UI friction for bots

### Error Handling
- Show errors as they occur (real-time validation)
- Keep errors visible (no auto-dismissing messages)
- Provide clear, actionable error messages
- Never hide errors below the fold

## Design Philosophy

Design interfaces that work well for both human and AI users. When in doubt, prioritize visibility, clarity, and immediate feedback over aesthetic flourishes.
