---
name: "spec-driven-development"
description: "Expert at spec-driven development workflow. Generates clear, concise spec.md files for features. Creates technical plan.md files from approved specs. Breaks complex projects into multi-phase plans with phase-task.md files for each phase. Use when starting new features, designing systems, or when user mentions specs, specifications, planning, phased development, or technical design."
allowed-tools: "Read,Write,Glob,Grep"
version: "1.0.0"
---

# Spec-Driven Development Expert

This skill implements a comprehensive spec-driven development workflow that progresses through three stages:

1. **Specification (spec.md)** → Define what needs to be built
2. **Technical Plan (plan.md)** → Define how it will be built
3. **Phase Tasks (phase-task.md)** → Define detailed implementation steps

## Core Principles

- **Clarity First:** Every document must be unambiguous and easy to understand
- **Completeness:** All necessary details must be captured before moving forward
- **Iterative Refinement:** Work with the user to perfect each document
- **Logical Progression:** Only move to the next stage when the current one is approved

---

## Stage 1: Creating spec.md

### When to Use

Use this when:
- User requests a specification for a new feature or project
- User wants to document requirements before implementation
- User mentions "spec", "specification", or "requirements"

### Specification Structure

A good spec.md file should include:

```markdown
# Feature/Project Name

## Overview
Brief 2-3 sentence description of what this feature/project is and why it's needed.

## Goals
- Primary goal 1
- Primary goal 2
- Primary goal 3

## Non-Goals
- Explicitly state what this spec does NOT cover
- Helps prevent scope creep

## Background
Context needed to understand the problem:
- Current state of the system
- Pain points or limitations
- Related features or dependencies

## Requirements

### Functional Requirements
1. **Requirement Name**
   - Description of the requirement
   - Acceptance criteria (how we know it's done)
   - Priority: [Critical/High/Medium/Low]

2. **Another Requirement**
   - Description
   - Acceptance criteria
   - Priority

### Non-Functional Requirements
- Performance requirements (latency, throughput, etc.)
- Security requirements
- Scalability requirements
- Compatibility requirements

## User Stories (if applicable)
- As a [user type], I want to [action] so that [benefit]
- As a [user type], I want to [action] so that [benefit]

## API/Interface Design (if applicable)
Define interfaces, API endpoints, data structures, etc.

## Data Model (if applicable)
Describe data structures, database schemas, or state management

## Dependencies
- External libraries or services needed
- Other features or systems this depends on
- Resources required

## Success Metrics
How will we measure if this is successful?
- Metric 1: [description and target]
- Metric 2: [description and target]

## Open Questions
- Question 1?
- Question 2?

## Assumptions
- Assumption 1
- Assumption 2
```

### Process for Creating spec.md

1. **Gather Information:**
   - Ask clarifying questions about the feature
   - Understand the problem being solved
   - Identify stakeholders and users

2. **Draft the Specification:**
   - Use the structure above as a template
   - Fill in all sections with specific, concrete details
   - Be thorough but concise
   - Use clear, non-technical language where possible

3. **Review for Completeness:**
   Check that the spec answers:
   - ✅ What is being built?
   - ✅ Why is it being built?
   - ✅ Who is it for?
   - ✅ What are the success criteria?
   - ✅ What are the constraints?
   - ✅ What are the dependencies?

4. **Iterate with User:**
   - Present the draft spec
   - Highlight any open questions or assumptions
   - Request feedback and clarification
   - Refine until user approves

5. **Save the File:**
   - Write to `spec.md` (or user-specified location)
   - Confirm file location with user

### Quality Checklist for spec.md

Before considering a spec complete, verify:

- [ ] **Concise:** No unnecessary verbosity, every sentence adds value
- [ ] **Clear:** Non-ambiguous language, technical terms defined
- [ ] **Complete:** All requirements captured, no critical gaps
- [ ] **Specific:** Concrete details, not vague statements
- [ ] **Testable:** Clear acceptance criteria for each requirement
- [ ] **Scoped:** Non-goals clearly stated
- [ ] **Feasible:** Requirements are achievable given constraints
- [ ] **Prioritized:** Requirements have priority levels

---

## Stage 2: Creating plan.md

### When to Use

Use this when:
- User has an approved spec.md and requests a technical plan
- User says "create a plan" or "how should we implement this"
- User explicitly approves moving from spec to planning phase

### Prerequisites

- ✅ spec.md file exists and has been approved by user
- ✅ Read and understand the full spec.md content

### Technical Plan Structure

A good plan.md file should include:

```markdown
# Technical Plan: [Feature Name]

## Overview
Brief summary of the technical approach for implementing the spec.

Reference: See `spec.md` for full requirements.

## Architecture Overview
High-level architectural decisions:
- Technology stack
- Design patterns to use
- System components involved
- Integration points

## Implementation Approach

### Option Analysis (if multiple approaches considered)
**Option 1: [Approach Name]**
- Pros: ...
- Cons: ...

**Option 2: [Approach Name]**
- Pros: ...
- Cons: ...

**Selected Approach:** [Chosen option] because [reasoning]

### Technical Design
- Component structure
- Data flow
- Key algorithms or logic
- Error handling strategy

## Implementation Phases

### Phase 1: [Phase Name]
**Goal:** [What this phase achieves]

**Tasks:**
1. Task 1 description
2. Task 2 description
3. Task 3 description

**Dependencies:** [What must be done before this phase]
**Deliverables:** [What this phase produces]
**Estimated Complexity:** [Low/Medium/High]

### Phase 2: [Phase Name]
**Goal:** [What this phase achieves]

**Tasks:**
1. Task 1 description
2. Task 2 description

**Dependencies:** Phase 1 completion
**Deliverables:** [What this phase produces]
**Estimated Complexity:** [Low/Medium/High]

[Additional phases as needed...]

## Testing Strategy
- Unit testing approach
- Integration testing approach
- End-to-end testing approach
- Test coverage goals

## Risks & Mitigations
| Risk | Impact | Mitigation |
|------|--------|------------|
| Risk 1 | High/Medium/Low | How to mitigate |
| Risk 2 | High/Medium/Low | How to mitigate |

## Rollout Strategy
- Deployment approach
- Feature flags (if applicable)
- Rollback plan
- Monitoring and observability

## Success Criteria
How we'll know the implementation is successful (from spec.md)
```

### Process for Creating plan.md

1. **Read and Analyze spec.md:**
   - Thoroughly understand all requirements
   - Identify technical complexity areas
   - Note dependencies and constraints

2. **Design Technical Approach:**
   - Determine architecture and technology choices
   - Consider multiple approaches if appropriate
   - Document key technical decisions

3. **Break Down into Phases:**

   **For Simple Projects (1-2 phases):**
   - Phase 1: Core implementation
   - Phase 2: Testing and refinement

   **For Complex Projects (3+ phases):**
   - Break logically by feature areas, layers, or dependencies
   - Ensure each phase delivers value
   - Each phase should be independently testable
   - Example breakdown:
     * Phase 1: Data layer and models
     * Phase 2: Core business logic
     * Phase 3: API/Interface layer
     * Phase 4: Frontend integration
     * Phase 5: Testing and optimization

4. **Define Phase Details:**
   For each phase, specify:
   - Clear goal statement
   - High-level task list (not too detailed yet)
   - Dependencies on other phases
   - Deliverables
   - Complexity estimate

5. **Add Supporting Sections:**
   - Testing strategy
   - Risk analysis
   - Rollout approach

6. **Review with User:**
   - Present the plan
   - Explain the phased approach and reasoning
   - Iterate based on feedback
   - Get approval before proceeding

### Quality Checklist for plan.md

Before considering a plan complete, verify:

- [ ] **Aligned with Spec:** All requirements from spec.md addressed
- [ ] **Phased Appropriately:** Logical breakdown with clear phase boundaries
- [ ] **Technically Sound:** Architecture and approach are feasible
- [ ] **Dependencies Clear:** Phase ordering makes sense
- [ ] **Testable:** Testing strategy for each phase
- [ ] **Risk-Aware:** Major risks identified with mitigations
- [ ] **Actionable:** Next steps are clear (create phase tasks)

---

## Stage 3: Creating phase-task.md Files

### When to Use

Use this when:
- User has an approved plan.md and requests detailed phase tasks
- User says "create tasks for phase X" or "let's start implementing"
- User explicitly approves moving from planning to task creation

### Prerequisites

- ✅ plan.md file exists and has been approved by user
- ✅ User specifies which phase(s) to create tasks for

### Phase Task File Structure

Create separate files for each phase: `phase-1-task.md`, `phase-2-task.md`, etc.

```markdown
# Phase [N] Tasks: [Phase Name]

## Phase Overview
**Goal:** [From plan.md - what this phase achieves]

**Dependencies:** [What must be complete before starting]

**Deliverables:** [What this phase will produce]

Reference: See `plan.md` for full technical plan and `spec.md` for requirements.

## Detailed Task Breakdown

### Task 1: [Task Name]
**Description:** Detailed description of what needs to be done

**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

**Implementation Details:**
- Specific files to create/modify
- Code changes needed
- Configuration updates required

**Dependencies:** [Other tasks that must be done first]

**Estimated Effort:** [Small/Medium/Large]

---

### Task 2: [Task Name]
**Description:** Detailed description

**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2

**Implementation Details:**
- Technical approach
- Files affected
- Key considerations

**Dependencies:** Task 1

**Estimated Effort:** [Small/Medium/Large]

---

[Additional tasks...]

## Testing Tasks

### Test Task 1: [Test Name]
**Description:** What needs to be tested

**Test Cases:**
- Test case 1: [Expected behavior]
- Test case 2: [Expected behavior]

**Testing Approach:** [Unit/Integration/E2E]

---

## Phase Completion Criteria

This phase is complete when:
- [ ] All implementation tasks done
- [ ] All tests passing
- [ ] Code reviewed
- [ ] Documentation updated
- [ ] [Other phase-specific criteria]

## Notes
Any additional context, considerations, or open questions for this phase.
```

### Process for Creating phase-task.md Files

1. **Reference plan.md:**
   - Read the specific phase details
   - Understand the phase goal and deliverables
   - Note the high-level tasks already outlined

2. **Expand Tasks into Details:**
   - Break each high-level task into specific, actionable steps
   - Add technical implementation details
   - Specify files and code areas affected
   - Define clear acceptance criteria

3. **Order Tasks by Dependency:**
   - Ensure tasks are ordered logically
   - Mark dependencies between tasks
   - Group related tasks together

4. **Add Testing Tasks:**
   - Create specific test tasks for the phase
   - Include unit, integration, and E2E tests as appropriate
   - Define test cases and expected outcomes

5. **Define Completion Criteria:**
   - Clear checklist for when phase is done
   - Include quality gates (tests, reviews, etc.)

6. **Create File:**
   - Save as `phase-1-task.md`, `phase-2-task.md`, etc.
   - Use consistent naming
   - Confirm location with user

### Quality Checklist for phase-task.md

Before considering phase tasks complete, verify:

- [ ] **Granular:** Tasks are specific and actionable
- [ ] **Complete:** All work from plan.md phase is captured
- [ ] **Ordered:** Tasks flow logically with dependencies noted
- [ ] **Testable:** Each task has clear acceptance criteria
- [ ] **Scoped:** Tasks are appropriately sized (not too big/small)
- [ ] **Clear:** Anyone could understand what needs to be done

---

## Workflow Summary

### Full Progression

```
1. User requests spec
   ↓
2. Generate spec.md → User reviews/approves
   ↓
3. User requests plan
   ↓
4. Generate plan.md → User reviews/approves
   ↓
5. User requests phase tasks
   ↓
6. Generate phase-N-task.md files → Ready for implementation
```

### Key Transitions

**spec.md → plan.md:**
- Only proceed when user explicitly approves spec
- Ask: "The spec looks complete. Would you like me to create a technical plan?"

**plan.md → phase-task.md:**
- Only proceed when user explicitly approves plan
- Ask: "The plan is ready. Which phase would you like me to create detailed tasks for?"

**Creating Multiple Phase Tasks:**
- Can create all phase task files at once if user requests
- Or create them one at a time as phases are started
- Default: Ask user their preference

---

## Best Practices

### Communication

1. **Always Summarize:** After creating any document, provide a brief summary of key points
2. **Highlight Gaps:** If there are open questions or assumptions, explicitly call them out
3. **Request Approval:** Never move to the next stage without user confirmation
4. **Explain Reasoning:** When making technical decisions, explain why

### Document Quality

1. **Use Templates:** Follow the structures provided in this skill
2. **Be Specific:** Avoid vague language like "implement the feature" - be concrete
3. **Stay Consistent:** Use the same terminology across all documents
4. **Cross-Reference:** Link between spec → plan → tasks for traceability

### Phasing Strategy

**When to Use Single Phase:**
- Project can be completed in < 10 tasks
- No complex dependencies
- Single component/area affected

**When to Use Multiple Phases:**
- Project has > 10 tasks
- Multiple components or layers
- Complex dependencies
- Logical breakpoints exist

**Good Phase Boundaries:**
- By architectural layer (data → logic → interface)
- By feature area (auth → core → admin)
- By dependency (foundation → features → polish)
- By risk (risky parts first for early validation)

---

## Examples

See the `templates/` directory for example files:
- `templates/example-spec.md` - Sample specification
- `templates/example-plan.md` - Sample technical plan
- `templates/example-phase-1-task.md` - Sample phase tasks

---

## Error Handling

If you encounter any of these situations:

**Missing Information:**
- Ask specific questions to fill gaps
- Don't make assumptions - clarify with user

**Unclear Requirements:**
- Request clarification before proceeding
- Suggest specific options if helpful

**Overly Complex Scope:**
- Suggest breaking into multiple specs
- Recommend focusing on MVP first

**User Skips Stages:**
- Politely explain the value of the full workflow
- Offer to create missing documents if they insist
