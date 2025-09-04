# Domain-Driven Design Architecture Guidelines

This document establishes the highest-level guiding principles for specification-driven design practice across all language implementations in this repository.

## Repository Architecture Philosophy

### Strategic Design Principles

1. **Bounded Context First**: Always begin with strategic DDD analysis to identify bounded contexts and domain boundaries before implementing tactical patterns.

2. **Consistency Boundaries**: Define aggregates and services based on transactional consistency requirements, not on data relationships or convenience.

3. **Domain Model Protection**: The domain model must remain pure and free from infrastructure concerns. All external dependencies must be abstracted through interfaces.

4. **Ubiquitous Language**: Maintain strict alignment between business terminology and code vocabulary across all implementations.

## Architectural Layering Standards

### Required Layer Structure
All implementations must follow this layering pattern:

```
├── Core/Architecture Layer      # Framework-agnostic DDD building blocks
├── Domain Layer                 # Pure business logic and domain models
├── Application Layer            # Use cases, commands, queries, handlers
├── Infrastructure Layer         # External concerns (data, messaging, etc.)
└── WebAPI/Presentation Layer    # HTTP endpoints and presentation logic
```

### Layer Dependencies
- **Domain** → Core/Architecture (only)
- **Application** → Domain + Core/Architecture
- **Infrastructure** → Application + Domain + Core/Architecture
- **WebAPI/Presentation** → Application + Infrastructure + Domain + Core/Architecture

**Critical Rule**: No circular dependencies between layers. Higher layers can depend on lower layers, never the reverse.

## Core DDD Building Blocks

### Aggregate Root Implementation Standards

#### Mandatory Characteristics
1. **Identity Management**: Strong-typed identity with appropriate ID generation strategy
2. **Domain Event Handling**: Internal event collection with `AddDomainEvent()` and `ClearDomainEvents()` methods
3. **Encapsulation**: All business logic encapsulated within aggregate boundaries
4. **Immutable Creation**: Factory methods for creating aggregates in valid states

#### Cross-Language Consistency
- **.NET**: Inherit from `AggregateRoot<TId>` with CSharpFunctionalExtensions integration
- **Java**: Extend `AggregateRoot<UUID>` with Vavr functional programming support  
- **Python**: Extend generic `AggregateRoot[TId]` with type hints and ABC enforcement

### Entity Guidelines
1. **Identity-Based Equality**: Entities are equal if their IDs are equal, regardless of other properties
2. **Immutable Identity**: Entity IDs must never change after creation
3. **Lifecycle Management**: Clear creation, modification, and state transition patterns

### Value Object Standards
1. **Immutability**: Value objects must be immutable after creation
2. **Structural Equality**: Equality based on all component values
3. **Self-Validation**: Built-in validation through factory methods or constructors
4. **Specification Pattern**: Complex validation logic encapsulated in specifications

## CQRS and Mediator Pattern

### Command and Query Separation
1. **Commands**: Represent business intentions that modify state
   - Must return `Result<T>` or equivalent for error handling
   - Should be named with imperative verbs (CreateOrder, UpdateCustomer)
   - Include all necessary data for the operation

2. **Queries**: Retrieve data without side effects
   - Must be idempotent and side-effect free
   - Return strongly-typed DTOs/ViewModels
   - Named with descriptive nouns (GetOrderById, ListCustomers)

### Handler Implementation
- One handler per command/query for clear responsibility separation
- Handlers must be stateless and dependency-injected
- Error handling through functional programming patterns (Result<T>, Either<L,R>, Try<T>)

## Event-Driven Architecture

### Domain Events
1. **Naming Convention**: Past tense verbs describing what happened (OrderCreated, CustomerUpdated)
2. **Payload Design**: Include aggregate ID and relevant data, avoid complex object graphs
3. **Timing**: Events raised during aggregate operations, dispatched after successful persistence
4. **Handler Isolation**: Each event handler must be independent and idempotent

### Integration Events
1. **Cross-Boundary Communication**: For communication between bounded contexts
2. **Schema Evolution**: Design events with forward/backward compatibility in mind
3. **Reliability Patterns**: Implement Outbox pattern for reliable event publishing

## Testing Strategy and Standards

### Test Organization
```
test/
├── unit/                    # Domain and application logic tests
├── integration/            # Infrastructure and cross-layer tests  
├── architecture/           # Architecture and design rule tests
└── e2e/                   # End-to-end API tests
```

### Testing Approaches by Layer

#### Domain Layer Testing
- **Focus**: Business logic validation and domain rules
- **Style**: Arrange-Act-Assert (AAA) pattern
- **Coverage**: 100% coverage of business-critical paths
- **Mocking**: Minimal; prefer real objects for domain testing

#### Application Layer Testing  
- **Focus**: Use case orchestration and command/query handling
- **Dependencies**: Mock external services and repositories
- **Error Scenarios**: Test all failure paths and edge cases
- **Integration**: Verify proper domain event handling

#### Infrastructure Testing
- **Database Tests**: Use test containers for real database interactions
- **External Services**: Use mocks or test doubles with contract testing
- **Configuration**: Test with various configuration scenarios

### Test Naming Conventions
- **Unit Tests**: `Should_[ExpectedBehavior]_When_[Condition]`
- **Integration Tests**: `[Scenario]_Should_[ExpectedOutcome]`
- **Architecture Tests**: `[Component]_Should_[ArchitecturalRule]`

## Code Style and Quality Standards

### Naming Conventions

#### General Principles
1. **Domain Terminology**: Use business domain vocabulary consistently
2. **Intention-Revealing**: Names should clearly express purpose and behavior
3. **Language-Specific**: Follow platform conventions while maintaining domain consistency

#### Specific Guidelines
- **Aggregates**: Nouns representing business concepts (Order, Customer, Product)
- **Domain Services**: Verbs describing business processes (OrderPricingService, InventoryAllocationService)
- **Commands**: Imperative verbs (CreateOrderCommand, UpdateCustomerAddressCommand)
- **Queries**: Descriptive phrases (GetOrderByIdQuery, SearchCustomersQuery)
- **Events**: Past tense verbs (OrderCreatedEvent, PaymentProcessedEvent)

### Error Handling Standards

#### Functional Approach Required
All implementations must use functional error handling patterns:

- **.NET**: `CSharpFunctionalExtensions.Result<T>`
- **Java**: `Vavr.Try<T>` or `Either<Error, T>`
- **Python**: Custom `Result<T>` or `Either<L, R>` implementation

#### Error Categories
1. **Domain Errors**: Business rule violations (handled in domain layer)
2. **Application Errors**: Use case failures (handled in application layer)
3. **Infrastructure Errors**: External system failures (handled with retry/circuit breaker patterns)

### Dependency Injection and IoC

#### Container Configuration
- Register all application services with appropriate lifetime scopes
- Use interface-based registration for testability
- Implement health checks for external dependencies

#### Service Lifetimes
- **Domain Services**: Singleton (stateless business logic)
- **Application Handlers**: Scoped (per request/transaction)
- **Infrastructure Services**: Appropriate to resource management needs

## Specification-Driven Development Process

### Specification First Approach
1. **Business Requirements**: Start with business specifications and acceptance criteria
2. **Domain Modeling**: Create domain models that reflect business specifications
3. **Test-Driven**: Write tests that validate specifications before implementation
4. **Iterative Refinement**: Continuously refine based on business feedback

### Documentation Requirements
1. **Architecture Decision Records (ADRs)**: Document all significant architectural decisions
2. **Domain Glossary**: Maintain shared vocabulary across all implementations
3. **API Documentation**: OpenAPI/Swagger specifications for all endpoints
4. **Deployment Guides**: Infrastructure and deployment documentation

## Cross-Language Implementation Consistency

### Shared Concepts
Despite language differences, maintain consistency in:
- Domain model structure and behavior
- Event schemas and naming
- API contracts and HTTP semantics
- Error handling and response formats
- Testing approaches and coverage expectations

### Language-Specific Adaptations
While maintaining conceptual consistency, leverage language-specific strengths:
- **.NET**: C# async/await, LINQ expressions, reflection capabilities
- **Java**: Stream API, Spring ecosystem integration, JVM optimization
- **Python**: Duck typing, dynamic capabilities, scientific computing libraries

## Observability and Monitoring

### Metrics and Telemetry
1. **Business Metrics**: Track domain-relevant KPIs (orders created, revenue, conversion rates)
2. **Technical Metrics**: Response times, error rates, throughput, resource utilization
3. **Correlation IDs**: Trace requests across service boundaries

### Logging Standards
1. **Structured Logging**: JSON format with consistent field names
2. **Log Levels**: Appropriate use of DEBUG, INFO, WARN, ERROR, FATAL
3. **Contextual Information**: Include correlation IDs, user IDs, and business context

## Security and Compliance

### Data Protection
1. **Sensitive Data**: Encrypt PII and financial data at rest and in transit
2. **Access Control**: Implement proper authorization at all layers
3. **Audit Trails**: Log all business-critical operations with proper attribution

### API Security
1. **Authentication**: Strong authentication mechanisms (OAuth 2.0, JWT)
2. **Authorization**: Fine-grained permissions based on business rules
3. **Rate Limiting**: Protect against abuse and ensure fair usage

## Performance and Scalability

### Design for Scale
1. **Stateless Services**: Design for horizontal scaling
2. **Caching Strategies**: Implement appropriate caching at multiple levels
3. **Database Optimization**: Proper indexing and query optimization
4. **Async Processing**: Use asynchronous patterns for long-running operations

### Monitoring and Alerting
1. **SLA Monitoring**: Track and alert on service level agreements
2. **Capacity Planning**: Monitor resource usage and plan for growth
3. **Performance Baselines**: Establish performance benchmarks and detect regressions

---

## Compliance and Quality Gates

### Definition of Done
For any feature implementation to be considered complete:

1. ✅ **Domain Model**: Properly encapsulated with business rules enforced
2. ✅ **Testing**: Unit, integration, and architecture tests with adequate coverage  
3. ✅ **Documentation**: Updated specifications and API documentation
4. ✅ **Error Handling**: Proper functional error handling throughout
5. ✅ **Observability**: Appropriate logging, metrics, and tracing
6. ✅ **Security**: Security review completed and vulnerabilities addressed
7. ✅ **Performance**: Performance requirements validated
8. ✅ **Cross-Language Consistency**: Implementation aligns with other language versions

### Code Review Checklist
- [ ] Domain model properly protects business invariants
- [ ] CQRS separation maintained with no query side effects  
- [ ] Error handling uses functional patterns appropriately
- [ ] Tests cover business-critical scenarios with clear naming
- [ ] Dependencies flow in correct direction between layers
- [ ] Security considerations addressed appropriately
- [ ] Performance implications considered and documented
- [ ] Observability requirements implemented

This guidelines document serves as the authoritative reference for all DDD architecture implementations in this repository. All code, tests, and documentation should align with these principles to ensure consistency, quality, and maintainability across language implementations.