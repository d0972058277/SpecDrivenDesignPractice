# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a "SpecDrivenDesignPractice" repository containing comprehensive Domain-Driven Design (DDD) architecture implementations across multiple languages. The repository serves as a reference for practicing specification-driven design principles with established architectural patterns and guidelines.

## Project Structure

The repository contains:
- `Guidelines.md` - **Comprehensive architectural guidance document (READ THIS FIRST)**
- `reference/` - Multi-language DDD reference implementations
  - `DDD4rchitectureDotNet/` - Primary .NET DDD reference implementation
  - `DDD4rchitectureJ/` - Java Spring Boot DDD implementation
  - `DDD4rchitecturePy/` - Python DDD implementation
- Specialized agents available for complex architectural tasks

## Essential Architectural Principles

**ALWAYS follow these core DDD principles from Guidelines.md:**

### 1. Strategic Design First
- Begin with bounded context analysis before implementing tactical patterns
- Define consistency boundaries based on transactional requirements
- Maintain ubiquitous language alignment between business and code

### 2. Required Layer Structure
```
├── Core/Architecture    # Framework-agnostic DDD building blocks
├── Domain              # Pure business logic and domain models
├── Application         # Use cases, commands, queries, handlers
├── Infrastructure      # External concerns (data, messaging, etc.)
└── WebAPI/Presentation # HTTP endpoints and presentation logic
```

### 3. Layer Dependencies (STRICT)
- Domain → Core/Architecture only
- Application → Domain + Core/Architecture
- Infrastructure → Application + Domain + Core/Architecture
- **NO circular dependencies between layers**

### 4. Mandatory DDD Building Blocks
- **Aggregate Roots**: Strong-typed identity, domain event handling, invariant enforcement
- **CQRS**: Separate command/query handlers with mediator pattern
- **Event-Driven**: Domain events (internal) + Integration events (cross-boundary)
- **Functional Error Handling**: Result<T> pattern for all operations
- **Specifications**: Complex domain rules in dedicated specification classes

## Development Standards

### Testing Strategy (REQUIRED)
- **Unit Tests**: Domain logic, specifications, handlers
- **Integration Tests**: Database, external services, cross-layer
- **Architecture Tests**: Layer dependencies, naming conventions
- **End-to-End Tests**: Complete business scenarios

### Code Quality Gates
- All domain logic must be pure (no infrastructure dependencies)
- Commands/queries through mediator pattern only
- Functional error handling (no exceptions for business logic)
- Strong typing with meaningful value objects
- Domain events for all state changes

### Specification-Driven Development
1. Write specifications first (business requirements)
2. Implement domain model to satisfy specifications
3. Add application services and handlers
4. Validate with comprehensive test suite
5. Review against architectural compliance

## Cross-Language Consistency

When working with multiple implementations:
- Follow same DDD patterns across all languages
- Maintain consistent aggregate boundaries
- Use language-specific functional programming libraries
- Apply same testing strategies adapted to each platform

## Quality Compliance

Before completing any work, ensure:
- [ ] Layer dependency rules followed
- [ ] Domain model remains pure
- [ ] CQRS pattern properly implemented
- [ ] Domain events published for state changes
- [ ] Functional error handling used
- [ ] Comprehensive tests added
- [ ] Code follows language-specific style guides

## Reference Documentation

- **Primary Reference**: `Guidelines.md` - Complete architectural guidance
- **Implementation Examples**: `reference/DDD4rchitectureDotNet/` (.NET primary)
- **Cross-Platform Examples**: `reference/DDD4rchitectureJ/` (Java), `reference/DDD4rchitecturePy/` (Python)

## Specialized Agents

Use these agents for complex tasks:
- `architector` - Distributed systems architecture, DDD design, complex system integration
- `backend-systems-architect` - Backend framework selection, performance optimization
- `project-analyzer-architect` - Codebase analysis and project structure guidance