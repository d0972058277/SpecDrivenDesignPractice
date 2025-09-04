# Technology Stack & Standards

## Current Language Implementations

### .NET Implementation
- **Framework**: .NET 8.0
- **ORM**: Entity Framework Core 8.0
- **CQRS**: MediatR for command/query mediation
- **Database**: MySQL with Entity Framework migrations
- **Messaging**: RabbitMQ via MassTransit for integration events
- **Background Jobs**: Hangfire for processing outbox messages
- **Testing**: xUnit, FluentAssertions, Moq
- **Functional Programming**: CSharpFunctionalExtensions for Result<T> pattern
- **API Documentation**: Swagger/OpenAPI
- **Container**: Docker Compose support

### Java Implementation  
- **Framework**: Spring Boot 3.2.0
- **Java Version**: Java 17
- **ORM**: Hibernate 6.4.0
- **CQRS**: PipelinR (MediatR equivalent for Java)
- **Database**: MySQL with Liquibase migrations
- **Functional Programming**: Vavr for functional patterns (Try<T>, Either<L,R>)
- **Testing**: JUnit, AssertJ
- **Build Tool**: Maven
- **API Documentation**: SpringDoc OpenAPI
- **Database Testing**: H2 in-memory database

### Python Implementation
- **Framework**: Custom DDD framework implementation
- **Architecture**: Clean Architecture with DDD patterns
- **Functional Programming**: Custom Result<T> and Either<L,R> implementations
- **Testing**: (Framework to be determined based on implementation)
- **Type Safety**: Type hints and ABC enforcement
- **CQRS**: Custom mediator pattern implementation

### Planned Implementations
- **Golang**: Planned future implementation
- **Node.js/TypeScript**: Planned future implementation

## Cross-Language Consistency Requirements

### Shared Architectural Patterns
- **CQRS**: Command/Query separation with mediator pattern
- **Event Sourcing**: Domain events (internal) + Integration events (cross-boundary)
- **Repository Pattern**: With Unit of Work for transactional consistency
- **Functional Error Handling**: Result<T> pattern across all languages
- **Aggregate Design**: Strong-typed identity, domain event handling, invariant enforcement
- **Specification Pattern**: Complex domain rules in dedicated specification classes

### Database and Persistence
- **Primary Database**: MySQL for production scenarios
- **Test Databases**: In-memory or containerized databases for testing
- **Migration Strategy**: Language-specific migration tools (EF, Liquibase, etc.)
- **Connection Management**: Connection pooling and proper resource disposal

## Performance Requirements

**沒有特殊的性能要求，因為可讀而產生小量的性能損耗是可接受的**

Performance philosophy prioritizes:
1. **Code Readability**: Clear, educational code over micro-optimizations
2. **Architectural Integrity**: Proper separation of concerns over performance shortcuts
3. **Acceptable Trade-offs**: Minor performance costs for maintainability and educational value are acceptable
4. **Baseline Standards**: Must meet reasonable response times for reference application scenarios
5. **Monitoring**: Basic performance monitoring to ensure implementations remain within acceptable bounds

## Development Tools & Standards

### Version Control
- **Git Workflow**: Feature branches with pull request reviews
- **Commit Standards**: Conventional commits with clear, descriptive messages
- **Branch Protection**: Require reviews and automated checks before merging

### Testing Requirements
- **Coverage**: Comprehensive testing across all architectural layers
- **Test Organization**: Clear separation of unit, integration, architecture, and e2e tests
- **Test Naming**: Follow `Should_ExpectedBehavior_When_Condition` pattern
- **Test Structure**: Use Given/When/Then comment markers for test phase delineation

### Documentation Standards
- **API Documentation**: OpenAPI/Swagger for all HTTP endpoints  
- **Code Documentation**: Focus on domain concepts and business rules
- **Architecture Documentation**: ADRs for significant decisions
- **Deployment Guides**: Clear setup and deployment instructions

## Infrastructure & DevOps

### Containerization
- **Docker**: Containerized applications for consistent development environments
- **Docker Compose**: Local development with dependent services (databases, message queues)
- **Container Registries**: Standard container publishing workflows

### CI/CD Requirements
- **Automated Testing**: Run all test suites on every commit
- **Build Validation**: Ensure successful builds across all language implementations
- **Quality Gates**: Code coverage, architectural compliance, security scanning
- **Deployment**: Automated deployment to development/staging environments

## Security Standards

### Application Security
- **Authentication**: OAuth 2.0/JWT for API authentication
- **Authorization**: Role-based access control at domain boundaries
- **Data Protection**: Encrypt sensitive data at rest and in transit
- **Input Validation**: Comprehensive input validation at API boundaries

### Development Security
- **Dependency Scanning**: Regular security updates for all dependencies
- **Secret Management**: No hardcoded secrets, use environment variables/key vaults
- **Code Analysis**: Static analysis security testing (SAST) in CI pipeline

## Technology Decision Criteria

When adding new technologies or frameworks:
1. **Educational Value**: Does it demonstrate important DDD/architectural concepts?
2. **Language Idioms**: Does it follow the target language's best practices?
3. **Maintenance Overhead**: Can it be reasonably maintained as a reference implementation?
4. **Cross-Language Consistency**: Can similar patterns be implemented in other target languages?
5. **Community Standards**: Is it a well-established, community-supported solution?