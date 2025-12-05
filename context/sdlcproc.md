# Software Development Lifecycle Context

This document defines the SDLC process and required design artifacts for application development. Claude must generate these artifacts before implementation begins.

---

## Overview

All application development follows a design-first approach. Before writing any implementation code, the following artifacts must be created and reviewed:

1. User Stories
2. Business Requirements
3. Non-Functional Requirements
4. Architecture Decisions

---

## Required Design Artifacts

### 1. User Stories

User stories capture functionality from the end-user perspective. Each user story must follow this format:

```
**US-[ID]: [Title]**

As a [type of user],
I want [goal/desire],
So that [benefit/value].

**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

**Priority:** [High | Medium | Low]
**Estimated Effort:** [S | M | L | XL]
```

**Guidelines:**
- Write stories from the user's perspective, not the system's
- Each story should be independently deliverable
- Include measurable acceptance criteria
- Group related stories into epics when appropriate

---

### 2. Business Requirements

Business requirements define what the system must accomplish to meet business objectives. Use this format:

```
**BR-[ID]: [Title]**

**Description:**
[Clear statement of the business need]

**Business Justification:**
[Why this requirement exists and what business value it provides]

**Success Metrics:**
- [Measurable outcome 1]
- [Measurable outcome 2]

**Dependencies:**
- [Related requirements or external dependencies]

**Constraints:**
- [Business rules or limitations]
```

**Guidelines:**
- Focus on business outcomes, not technical solutions
- Each requirement should be traceable to business objectives
- Include quantifiable success metrics where possible
- Document any regulatory or compliance requirements

---

### 3. Non-Functional Requirements

Non-functional requirements define system qualities and constraints. Organize by category:

```
**NFR-[ID]: [Title]**

**Category:** [Performance | Security | Scalability | Reliability | Usability | Maintainability | Compliance]

**Requirement:**
[Specific, measurable requirement statement]

**Measurement:**
[How this requirement will be verified]

**Priority:** [Critical | High | Medium | Low]
```

**Required Categories to Address:**

| Category | Consider |
|----------|----------|
| Performance | Response times, throughput, resource usage |
| Security | Authentication, authorization, data protection, audit |
| Scalability | Load handling, growth capacity, elasticity |
| Reliability | Availability, fault tolerance, recovery |
| Usability | Accessibility, user experience, documentation |
| Maintainability | Code quality, modularity, testing |
| Compliance | Regulatory requirements, standards adherence |

---

### 4. Architecture Decisions

Architecture decisions document significant technical choices using Architecture Decision Records (ADRs):

```
**ADR-[ID]: [Title]**

**Status:** [Proposed | Accepted | Deprecated | Superseded]

**Context:**
[What is the issue or situation that motivates this decision?]

**Decision:**
[What is the change being proposed or decided?]

**Consequences:**

*Positive:*
- [Benefit 1]
- [Benefit 2]

*Negative:*
- [Tradeoff 1]
- [Tradeoff 2]

*Risks:*
- [Risk 1 and mitigation]

**Alternatives Considered:**
1. [Alternative 1] - [Why not chosen]
2. [Alternative 2] - [Why not chosen]
```

**When to Create an ADR:**
- Technology or framework selection
- Architectural patterns (monolith vs microservices, event-driven, etc.)
- Data storage decisions
- Integration approaches
- Security architecture
- Infrastructure choices

---

## Artifact Generation Process

### Step 1: Discovery
When a new feature or application is requested:
1. Clarify the business context and objectives
2. Identify stakeholders and end users
3. Understand constraints and existing systems

### Step 2: Generate Artifacts
Create artifacts in this order:
1. **Business Requirements** — Define what the business needs
2. **User Stories** — Break down into user-facing functionality
3. **Non-Functional Requirements** — Define quality attributes
4. **Architecture Decisions** — Document technical approach

### Step 3: Review and Validate
Before implementation:
- Ensure all artifacts are complete
- Verify traceability between artifacts
- Confirm acceptance criteria are testable
- Validate architecture decisions address NFRs

---

## Traceability Matrix

Maintain relationships between artifacts:

| User Story | Business Requirement | NFRs Addressed | ADRs Applied |
|------------|---------------------|----------------|--------------|
| US-001 | BR-001, BR-002 | NFR-001 | ADR-001 |

---

## Output Format

When generating design artifacts, create separate markdown files:

```
/docs
  /requirements
    business-requirements.md
    non-functional-requirements.md
  /user-stories
    epic-[name].md
  /architecture
    decisions/
      adr-001-[title].md
      adr-002-[title].md
    overview.md
```

---

## Implementation Rules

1. **No code before design** — All required artifacts must exist before implementation begins
2. **Incremental refinement** — Artifacts can be updated as understanding improves
3. **Traceability required** — Every implementation should trace back to a user story
4. **Decisions are permanent** — ADRs are immutable; supersede rather than modify

---

## Quick Reference Prompts

Use these prompts to request artifact generation:

- "Generate user stories for [feature]"
- "Define business requirements for [capability]"
- "Document non-functional requirements for [system]"
- "Create an ADR for [technical decision]"
- "Generate all design artifacts for [application/feature]"
