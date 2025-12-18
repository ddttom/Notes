# CLAUDE.md

This file provides guidance to AI Assistants when working with code in this repository.

## Repository Purpose

This is a notes repository containing coding standards, architectural guidelines, and development practices for building web applications. The notes define a philosophy for AI-assisted development with specific patterns for frontend/backend architecture.

## Core Development Philosophy

### Coding Standards

**JavaScript/Frontend:**

- Use simple, plain JavaScript - do not add third-party libraries without explicit user permission
- CSS3 with variables and master styles
- Block-level additions must be aware of master styles
- All configuration variables at the top of code - no hard-coded text or constants
- Maintainability over clever code; simplicity over complexity

**Code Organization:**

- Look for opportunities to add shared functions to core library
- Core library is shareable between backend and frontend
- Consider refactoring opportunities during development

### Project Structure

Standard project layout should include:

- `frontend/` - Frontend application code
- `backend/` - Backend services and APIs
- `msg-q/` - Message queue implementation with GraphQL action commands
- `api/` - API definitions and schema
- Core library folder (shared utilities)

## Architectural Patterns

### Frontend-Backend Separation

**Frontend:**

- Should not have knowledge of which connectors are in use
- Only knows that connectors are available and the expected schema
- Receives structured responses from backend via message queue

**Backend:**

- Should not know what the frontend will do with results
- Strictly follows the schema
- UI-agnostic and multitenant
- Communicates via message API with structured JSON
- All backend code must be thread-safe and injection-proof

**Schema/API Layer:**

- The schema index is where developer and AI collaborate to build APIs
- Message queue and connectors communicate through this layer

### Backend Architecture Details

**Multi-tenancy:**

- Each tenant has a separate database (avoiding leakage)
- Each tenant has a global Config JSON object for configuration and whitelabel overrides
- Multiple owners per tenant possible, with one admin owner who can delegate
- Developer admin can be promoted to super admin with full audit logging

**Security:**

- All actions must be checked for SQL injections or bad actor intents
- Admin functions restricted to tenant owners only
- Full audit logging for admin operations

**Connectors:**

- Used by backend, not frontend
- Can only execute if tenant has subscribed to the connector
- Return "not subscribed" error if tenant lacks subscription
- Subscription state must be checked before execution

**Data Transformation:**

- Connectors must use data transformation objects for tracing and debugging

**Logging:**

- All errors logged to central log
- All action messages logged to tenant log
- Tenant logs cycled at midnight, retained for one week
- Separate action channel for tenant and developer administration

**Function State Tracking:**

- Backend functions tracked with states: dummy, erroring, under dev, potential release, under user test, release, reverted
- Maintain index of backend functions and their states
- Initial prototype uses dummy functions returning "to be implemented" marked with #TODO

**Environment Management:**

- Separate folders for potential backend code and build process
- Deployment environments: local dev, local test, local qa, hosted test, hosted live
- Build process allows developer to select what will be released

### Database Approach

- Use NoSQL database approach to avoid SQL migration problems
- Each tenant has separate database for isolation

## UI/UX Guidelines for AI Interaction

**Things to Avoid:**

- Toasts, fades, item removal, below-the-fold error messages
- Hiding divs until previous div complete
- Error messages only after submit - provide errors as you go
- Too much SPA (Single Page Application) - have clear response pages
- Animation, floating loaders, and spinners (poor AI experience)
- Hiding important information in disclaimers

**Best Practices:**

- Design for both human and AI bot usage
- Adjust UI to accelerate AI engagement ("personalization for robots")
- Make everything visible and obvious - don't assume AI will read console
- Clear, immediate feedback instead of delayed responses

## Design System

- Always maintain design rules that are whitelabelable
- Define button shapes, sizes, and breakpoints
- Use EDS shim with plusplus additions

## Git Workflow

The "step commit" workflow should be used for systematic commits:

1. Initial git commit of changes
2. Run lint and fix all errors
3. Git commit lint fixes
4. Check last modification dates of README.md, CLAUDE.md, and CHANGELOG.md
5. Consider modifications to those files based on recent changes
6. Review project documents mentioned in CLAUDE.md
7. Git commit documentation changes
8. Update LEARNINGS.md (contains what AI struggled to understand)
9. Update PROJECTSTATE.md (current state, not historical)
10. Update CHANGELOG.md
11. Commit and push

**Attribution:**

- Ensure proper attribution in git commits

## Development Tools

Consider investigating these tools:

- Gate, Semgrep, and CodeQL for security scanning
- OWASP ZAP for security testing
- Postman-like tools for API testing
- Separate AI for code review
