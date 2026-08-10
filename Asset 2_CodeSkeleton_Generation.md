# Objective

Generate implementation-ready technical specifications and Spring Boot code skeletons from user stories.

The solution must support different client technology standards.

If client standards are provided,
follow them.

Otherwise use default Spring Boot best practices.

---

## Input

### User Stories
<Refer to User Stories created for E1 only>

### Optional Client Coding Standards
{{CLIENT_CODING_STANDARDS}}

Examples:

- Java Version
- Spring Boot Version
- Package Structure
- Kafka Standards
- API Standards
- Logging Standards
- Monitoring Standards
- Security Standards
- Testing Standards
- Database Standards
- Naming Conventions

If empty use:

Java 21
Spring Boot 3.x
JUnit 5
Mockito
SLF4J
Logback
Kafka Client
Constructor Injection
Layered Architecture

---

## Stage 1
User Story → Technical Specification

Generate one specification per user story.

Spec IDs:

TechnicalSpec_<StoryID>

Each specification must contain:

- Spec ID
- Linked Story ID
- Functional Overview
- Technical Flow
- Source Systems
- Target Systems
- Input Data
- Output Data
- APIs Required
- Kafka Topics Required
- Database Requirements
- Validation Rules
- Error Handling
- Logging Requirements
- Monitoring Requirements
- Security Considerations
- Components Required

---

## Stage 2
Technical Specification → Code Skeleton

Generate one code skeleton per specification.

Code IDs:

CodeSkeleton_<StoryID>

For each provide:

- Linked Spec ID

- Project Structure

- Controller

- Service Interface

- Service Implementation

- DTO Classes

- Entity Classes

- Repository Layer

- Kafka Producer

- Kafka Consumer

- Configuration Classes

- Exception Handler

- Logging Statements

- Unit Tests

- TODO markers

---

## Default Architecture

If client standards are not provided use:

com.client.project

├── controller
├── service
├── service.impl
├── repository
├── entity
├── dto
├── kafka.consumer
├── kafka.producer
├── exception
├── config
├── mapper
└── test

Use:

- Constructor Injection
- Lombok
- SLF4J Logging
- REST APIs
- Spring Validation
- JPA Repository
- Kafka Template
- Global Exception Handler

---

## Output Structure

CodeGenerationAssets/

├── TechnicalSpecs/
│   ├── TechnicalSpec_E1-US1.md
│   ├── TechnicalSpec_E1-US2.md
│   └── ...
│
├── CodeSkeletons/
│   ├── CodeSkeleton_E1-US1.md
│   ├── CodeSkeleton_E1-US2.md
│   └── ...
│
└── TraceabilityMatrix.md

Traceability Matrix:

Story ID → Spec ID
Spec ID → Code Skeleton ID