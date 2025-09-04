# Project Structure & Conventions

## Repository Organization

### Root Level Structure
```
├── .claude/                    # Claude Code configuration and agents
│   ├── agents/                # Specialized agents for complex tasks
│   ├── commands/              # Custom commands for spec-driven workflow
│   ├── steering/              # This directory - project guidance documents
│   ├── specs/                 # Generated specifications
│   └── templates/             # Templates for various document types
├── reference/                  # Multi-language DDD reference implementations
│   ├── DDD4rchitectureDotNet/ # Primary .NET DDD implementation
│   ├── DDD4rchitectureJ/      # Java Spring Boot DDD implementation
│   ├── DDD4rchitecturePy/     # Python DDD implementation
│   └── [Future: Golang, Node.js/TypeScript]
├── Guidelines.md              # **READ FIRST** - Comprehensive architectural guidance
└── CLAUDE.md                  # Repository-wide instructions for Claude Code
```

### Language Implementation Structure
Each language implementation follows the same layered architecture:

```
├── Core/Architecture/          # Framework-agnostic DDD building blocks
├── Domain/                    # Pure business logic and domain models
├── Application/               # Use cases, commands, queries, handlers
├── Infrastructure/            # External concerns (data, messaging, etc.)
├── WebAPI/Presentation/       # HTTP endpoints and presentation logic
└── test/                     # Comprehensive test suites
    ├── unit/                 # Domain and application logic tests
    ├── integration/          # Infrastructure and cross-layer tests
    ├── architecture/         # Architecture and design rule tests
    └── e2e/                  # End-to-end API tests
```

## Naming Conventions

### General Principles
- **Domain Terminology**: Use business domain vocabulary consistently across all languages
- **Intention-Revealing**: Names should clearly express purpose and behavior
- **Language-Specific**: Follow platform conventions while maintaining domain consistency
- **Cross-Language Alignment**: Same concepts should have recognizable names across implementations

### DDD Component Naming
- **Aggregates**: Nouns representing business concepts (Order, Customer, Product)
- **Domain Services**: Verbs describing business processes (OrderPricingService, InventoryAllocationService)
- **Commands**: Imperative verbs (CreateOrderCommand, UpdateCustomerAddressCommand)
- **Queries**: Descriptive phrases (GetOrderByIdQuery, SearchCustomersQuery)
- **Events**: Past tense verbs (OrderCreatedEvent, PaymentProcessedEvent)
- **Value Objects**: Descriptive nouns (Money, Address, ProductCode)
- **Specifications**: Business rule descriptions (CustomerHasValidCreditSpecification)

### File and Directory Naming
- **Consistent Casing**: Follow language conventions (PascalCase for .NET, camelCase for Java, snake_case for Python)
- **Clear Hierarchy**: Directory structure reflects architectural layers
- **Test Organization**: Mirror source structure in test directories
- **Shared Concepts**: Same logical groupings across all language implementations

## Coding Standards

### Test Standards - ALL LANGUAGES
**Test method naming conventions strictly follow the `Should_ExpectedBehavior_When_Condition` pattern for readability and clarity.**

**Each test method clearly delineates test phases using comment markers:**
- `// Given`: Test preconditions and setups  
- `// When`: Execution of the functionality under test
- `// Then`: Verification of outcomes and assertions

Example:
```csharp
[Fact]
public void Should_CreateOrder_When_ValidCustomerAndProducts()
{
    // Given
    var customerId = new CustomerId(Guid.NewGuid());
    var products = new List<ProductId> { new ProductId(Guid.NewGuid()) };
    
    // When  
    var result = Order.Create(customerId, products);
    
    // Then
    result.IsSuccess.Should().BeTrue();
    result.Value.CustomerId.Should().Be(customerId);
}
```

### Language-Specific Standards

#### .NET Standards
- **Coding Style**: Follow Microsoft C# Coding Conventions
- **Async/Await**: Use async/await for I/O operations
- **LINQ**: Leverage LINQ for collection operations
- **Nullable Reference Types**: Enable and use nullable reference types
- **XML Documentation**: Document public APIs with XML comments

#### Java Standards  
- **Coding Style**: Follow Google Java Style Guide
- **Stream API**: Use Stream API for collection processing
- **Optional**: Use Optional<T> for potentially null values
- **Lombok**: Use Lombok annotations to reduce boilerplate
- **JavaDoc**: Document public APIs with JavaDoc comments

#### Python Standards
- **Coding Style**: Follow PEP 8 Style Guide
- **Type Hints**: Use comprehensive type hints throughout
- **Docstrings**: Follow Google/NumPy docstring conventions
- **Abstract Base Classes**: Use ABC for interface definitions
- **Dataclasses**: Use dataclasses for simple data containers

## Layer Dependencies & Architecture Rules

### Strict Dependency Rules
- **Domain** → Core/Architecture (only)
- **Application** → Domain + Core/Architecture  
- **Infrastructure** → Application + Domain + Core/Architecture
- **WebAPI/Presentation** → Application + Infrastructure + Domain + Core/Architecture

**Critical Rule**: No circular dependencies between layers. Higher layers can depend on lower layers, never the reverse.

### Domain Layer Purity
- **No Infrastructure Dependencies**: Domain layer must remain pure
- **Framework Agnostic**: No references to web frameworks, databases, or external services
- **Business Logic Focus**: Contains only domain entities, value objects, services, and events
- **Interface Definitions**: Define repository interfaces, but no implementations

### Application Layer Responsibilities  
- **Use Case Orchestration**: Command and query handlers coordinate domain operations
- **Transaction Management**: Define transaction boundaries
- **Domain Event Handling**: Process domain events within bounded context
- **DTO Definitions**: Define data transfer objects for external communication

## File Organization Patterns

### Source Code Organization
- **Feature Folders**: Group related domain concepts together
- **Layer Separation**: Clear physical separation between architectural layers  
- **Shared Kernel**: Common concepts in Core/Architecture layer
- **Test Mirroring**: Test structure mirrors source structure exactly

### Configuration and Settings
- **Environment-Specific**: Separate configurations for development, testing, production
- **Secret Management**: No secrets in source control, use environment variables
- **Database Connections**: Configurable connection strings and providers
- **Feature Flags**: Support for feature toggles and A/B testing

## Documentation Standards

### Code Documentation
- **Domain Concepts**: Focus documentation on business rules and domain concepts
- **API Documentation**: Comprehensive OpenAPI/Swagger specifications
- **Architecture Decisions**: Document significant decisions in ADR format
- **Setup Instructions**: Clear, step-by-step setup and deployment guides

### Specification Documentation
- **Requirements**: Business requirements in executable specification format
- **Acceptance Criteria**: Clear, testable acceptance criteria for all features
- **Domain Glossary**: Shared vocabulary across all implementations
- **Integration Contracts**: API contracts and event schemas

## Review and Contribution Processes

### Code Review Requirements
- **Architectural Compliance**: Verify adherence to layer dependencies and DDD patterns
- **Test Coverage**: Ensure comprehensive test coverage across all layers
- **Domain Model Protection**: Verify domain model remains pure and encapsulated
- **Cross-Language Consistency**: Ensure implementations align with other language versions
- **Performance Considerations**: Review for acceptable performance characteristics
- **Security Review**: Check for security vulnerabilities and proper data handling

### Contribution Workflow
1. **Issue Creation**: Clear problem statement and acceptance criteria
2. **Design Review**: Architectural design review for significant changes
3. **Implementation**: Follow spec-driven development workflow
4. **Testing**: Comprehensive test suite with all test types
5. **Documentation**: Update specifications and architectural documentation
6. **Review Process**: Peer review focusing on architectural compliance
7. **Integration**: Automated CI/CD pipeline validation
8. **Deployment**: Staged deployment with monitoring and rollback capability

### Quality Gates
- **Build Success**: All language implementations must build successfully
- **Test Coverage**: Maintain minimum coverage thresholds for critical paths
- **Architecture Tests**: Automated verification of architectural constraints
- **Performance Baselines**: Performance regression testing
- **Security Scanning**: Automated security vulnerability scanning
- **Documentation Currency**: Ensure documentation remains current with implementation

## Spec-Driven Development Integration

### Workflow Integration
- **Specification First**: All features begin with business specifications
- **Task Breakdown**: Clear task organization following specification structure
- **Agent Utilization**: Leverage specialized agents for complex architectural tasks
- **Template Usage**: Use established templates for consistent documentation
- **Iterative Refinement**: Continuous refinement based on feedback and validation

### Template Organization
- **Bug Analysis & Reports**: Structured bug investigation and reporting
- **Design Documents**: Technical design and architecture planning
- **Requirements**: Business requirements and acceptance criteria
- **Task Planning**: Implementation task breakdown and organization
- **Verification**: Testing and validation procedures

This structure ensures consistency across all language implementations while allowing for language-specific idioms and best practices. All changes should align with these conventions to maintain the educational and reference value of the repository.