# Development Notes & Guidelines

This repository contains coding standards, architectural guidelines, and development practices for building AI-friendly web applications with a focus on maintainability, security, and multi-tenant architecture.

## Contents

### Core Guidelines

- **[Starter.md](Starter.md)** - Foundational coding standards and project setup principles
  - JavaScript/CSS standards
  - Code organization rules
  - Core library approach
  - Project folder structure

- **[Vibe coding backend.md](Vibe%20coding%20backend.md)** - Backend architecture and implementation guidelines
  - Multi-tenant architecture patterns
  - Message queue-based communication
  - Connector subscription model
  - Security and logging requirements
  - Function state tracking and deployment strategy

- **[Things to avoid.md](Things%20to%20avoid.md)** - UI/UX anti-patterns for AI interaction
  - Patterns that hinder AI agent interaction
  - Best practices for AI-friendly interfaces

- **[Vibe coding commit step.md](Vibe%20coding%20commit%20step.md)** - Git workflow and commit process
  - Step-by-step commit procedure
  - Documentation maintenance workflow

### Planning Documents

- **[Todo.md](Todo.md)** - Development tasks and infrastructure improvements
- **[Prd updates.md](Prd%20updates.md)** - Product requirements and feature planning
- **[Tad.md](Tad.md)** - Business model and engagement notes

## Key Principles

### Architecture

- **Frontend/Backend Separation**: Communication via message queue with structured GraphQL API
- **Schema-Driven Development**: Schema index is the collaboration point between developer and AI
- **Multi-Tenant by Design**: Each tenant has isolated database and configuration
- **Connector Pattern**: Backend functions are modular, subscription-based connectors

### Code Quality

- Simple, plain JavaScript - avoid unnecessary third-party libraries
- Configuration over hard-coding
- Maintainability over clever code
- Thread-safe, injection-proof backend code
- Shared core library between frontend and backend

### Security

- SQL injection protection
- Multi-tenant data isolation
- Role-based admin access
- Comprehensive audit logging
- Connector subscription validation

### AI-First Design

- Clear, immediate feedback (no hidden errors or delayed responses)
- Avoid animations and loaders that confuse AI agents
- Make all information visible and obvious
- Design for both human and AI interaction

## For AI Assistants

If you're an AI assistant working in this codebase, please read [CLAUDE.md](CLAUDE.md) for detailed guidance on implementing these principles.

## Development Workflow

When making changes:

1. Follow the coding standards in Starter.md
2. Implement backend changes according to Vibe coding backend.md patterns
3. Use the step commit process from Vibe coding commit step.md
4. Update documentation as you go

## License

Private development notes.
