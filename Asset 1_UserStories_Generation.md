# AI Asset 1: Automated User Story Generation from Business Requirement to Epics and User Stories

## Objective

Generate a business-to-delivery artifact pack by:

1. Converting the Business Requirement into exactly 2 business-focused epics.
2. Converting each epic into developer-ready user stories.

This asset is intended for requirement decomposition only and must maintain full traceability from the Business Requirement to every generated artifact.

---

## Input

### Business Requirement

Refer to:

<Business_Requirement.md>

### Client Standards

Refer to:
<Client_Standards.md>


---

## Core Rules

### Rule 1: Business Requirement First

Use only information present in the provided Business Requirement.

Do not introduce:

- Additional business scope
- New functionality
- New integrations
- New systems
- New user roles
- Future enhancements
- Technical solutions
- Domain assumptions

If a requirement is not present in the Business Requirement, it must not appear in the output.

---

### Rule 2: Zero Hallucinations

Every generated artifact must be traceable to a real requirement from the Business Requirement.

Each Epic and User Story must include a:

Refer to:

<Business_Requirement.md>

that points to the actual requirement, section, business rule, or statement from the source document.

Do not create:

- Fake requirement IDs
- Fake BRD references
- Inferred requirements
- Unsupported acceptance criteria

If a generated item cannot be traced back to a requirement:

```text
Generation Stopped:
Unsupported requirement detected.
Item cannot be traced to Business_Requirement.md.
```

---

### Rule 3: No Assumptions

Do not:

- Create assumptions
- Fill gaps
- Estimate missing information
- Infer expected behavior

If information is missing:

```text
Not specified in Business Requirement.
```

---

## Stage 1: Business Requirement → Epics

Generate exactly 2 business-focused epics.

Epic IDs:

```text
E1
E2
```

The epics must represent business outcomes, not technical implementation tasks.

### For each Epic provide

- Epic ID
- Epic Name
- Epic Title
- Epic Description
- Business Objective
- Business Value
- Key Features
- Dependencies
- Success Criteria
- BRD Reference

---

## Stage 2: Epics → User Stories

Generate exactly 5 user stories for each epic.

Total Stories:

```text
10
```

### Story IDs

For Epic E1

```text
E1-US1
E1-US2
E1-US3
E1-US4
E1-US5
```

For Epic E2

```text
E2-US1
E2-US2
E2-US3
E2-US4
E2-US5
```

### For each story provide

- Story ID
- Story Title
- Linked Epic ID
- User Story
- Business Value
- Acceptance Criteria
- Technical Dependencies
- Risks
- BRD Reference

### User Story Format

```text
As a <role>
I want <capability>
So that <business outcome>
```

---

## User Story Quality Rules

Every story must:

- Map to exactly one epic
- Be independently buildable
- Be implementation-focused
- Be estimable
- Be testable
- Have business value
- Have traceability
- Have a valid BRD reference

Apply INVEST principles:

- Independent
- Negotiable
- Valuable
- Estimable
- Small
- Testable

---

## Output Structure

Return all outputs using the following structure:

```text
UserStoryGenerationAsset/

├── Epics.md
├── Epic_E1_<ShortName>/
│   └── UserStories/
│       └── UserStories_E1.md
├── Epic_E2_<ShortName>/
│   └── UserStories/
│       └── UserStories_E2.md
└── TraceabilityMatrix.md
```

---

## File Output Format

Separate every file using:

```text
---
FILE: <relative/path/filename.md>
```

Example:

```text
---
FILE: UserStoryGenerationAsset/Epics.md
```

Then provide the contents of that file.

Do not include explanatory text outside the file outputs.

---

## Traceability Matrix

Create:

```text
TraceabilityMatrix.md
```

Format:

| Epic ID | Epic Name | Story IDs | BRD Reference |
|----------|------------|------------|------------|

Rules:

- Every epic must appear once.
- Every story must appear once.
- Every story must map to exactly one epic.
- Every row must contain a valid BRD Reference.

---

## Output Scope

Generate only:

- Epics
- User Stories
- Traceability Matrix

Do not generate:

- Assumptions
- Technical Specifications
- Low Level Design
- Architecture Documents
- Sequence Diagrams
- Source Code
- Code Skeletons
- Test Scripts
- Deployment Artifacts

The output must remain strictly grounded in the provided Business_Requirement.md.
`