=---
name: engineering-planner
description: Generate standardized technical implementation plans for engineering teams. Use this skill when the user requests to create an implementation plan, write a technical plan, plan out how to build a feature, or create documentation for an engineering task. Triggers include phrases like "create an implementation plan", "write a technical plan", "plan out how to build", "create an engineering plan", or "document the technical approach". This skill ensures consistent structure, appropriate detail level, clear separation between business and technical concerns, and comprehensive coverage of backend/frontend implementation, validations, and technical considerations. Always use this skill to maintain plan quality standards and prevent format inconsistencies across the engineering team.
---

# Engineering Plan Generator

## Overview

This skill helps you create comprehensive, standardized technical implementation plans that balance clarity with completeness. These plans are designed to be reviewed by engineers before implementation begins, catching architectural issues early and ensuring everyone understands the approach.

## When to Use This Skill

Use this skill whenever you need to create a technical implementation plan, including:
- Planning new feature implementations
- Documenting system design decisions
- Creating technical specifications for code reviews
- Outlining implementation approaches after whiteboard sessions
- Breaking down complex features into actionable engineering tasks

## Core Principles

### 1. Consistent Structure
Every plan follows the same structure to make reviews predictable and efficient. Engineers should know exactly where to find information about validations, edge cases, or technical decisions.

### 2. Two-Audience Design
- **Non-technical sections** (General Objective, What Are We Going to Do): Understandable by product managers and stakeholders
- **Technical sections** (Backend/Frontend Implementation, Technical Considerations): Detailed enough for engineers to implement

### 3. "What" Not "How"
Plans describe WHAT the system will do and how components interact, but NOT the specific code implementation. Focus on business logic flow and architectural decisions, not syntax.

### 4. Diagram-Driven Understanding
Include both:
- **High-level diagram**: Business flow for non-technical stakeholders
- **Technical flow diagram**: Specific modules, services, and methods for engineers

These diagrams help catch modeling issues before any code is written.

## Plan Structure

Follow this exact structure for every plan. Read `references/plan-template.md` for the complete template with all sections and their detailed requirements.

### Required Sections

1. **Feature/Task Name** - Clear title
2. **General Objective** - Business value in 2-3 sentences (non-technical)
3. **What Are We Going to Do** - Solution in plain language (non-technical)
4. **General Model/Flow Diagram** - High-level business flow (non-technical)
5. **Assumptions, Edge Cases, and Restrictions** - Explicit listing of constraints
6. **Technical Implementation Flow Diagram** - Detailed architecture diagram showing modules/services/methods
7. **Backend Implementation** - Detailed backend approach (if applicable)
8. **Frontend Implementation** - Detailed frontend approach (if applicable)
9. **Files to Create/Modify - Summary** - Complete file list
10. **Technical Considerations** - Grouped by concern (APIs, Auth, Error Handling, etc.)

**Note:** Do NOT include testing sections in the plan. Testing strategy is handled separately.

## Writing Guidelines

Before generating a plan, read `references/writing-guidelines.md` for comprehensive quality standards including:
- Section-specific guidelines
- Common mistakes to avoid
- Quality checklist
- Length guidelines
- Examples of good vs bad writing

### Key Writing Rules

**Be Specific About Architecture:**
- ✅ "Call OrganizationsService.findById(organizationId)"
- ❌ "Get the organization data"

**Focus on Business Logic:**
- ✅ "Validate that email domain matches organization's domain, then call Auth0 API to create user"
- ❌ "Use regex to check domain, then POST to /api/v2/users endpoint with axios"

**Include Complete Method Signatures:**
```
ServiceName.methodName
Method: methodName(param1: Type, param2: Type): Promise<ReturnType>
Logic:
1. Step-by-step business logic
2. Service calls with specific method names
3. Return processed result
Validations:
- All business rules
```

**Always Include Validations:**
Every operation should explicitly list what can go wrong and what gets validated.

**Use Mermaid for Diagrams:**
- Sequence diagrams for technical flows
- Flowcharts for business logic
- Keep them focused on the critical path

## Workflow

### Step 1: Understand Requirements
Before writing the plan, ensure you understand:
- The business problem being solved
- The existing architecture and patterns
- What users will be able to do
- Any constraints or restrictions
- Edge cases and assumptions

Ask clarifying questions if needed.

### Step 2: Load References
Always load both reference files before writing:
```
view references/plan-template.md
view references/writing-guidelines.md
```

### Step 3: Generate the Plan
Follow the template structure exactly. Include all required sections. For each section:
- Check the template for format requirements
- Review guidelines for that section type
- Write with appropriate detail level
- Focus on "what" not "how"

### Step 4: Create Diagrams
Include both required diagrams:

**General Model/Flow Diagram:**
- High-level, non-technical
- Shows user actions and outcomes
- Simple enough for stakeholders

**Technical Implementation Flow Diagram:**
- Detailed sequence diagram
- Shows specific modules, services, methods
- Includes API calls and data flow

### Step 5: Quality Check
Before finalizing, verify against the checklist in `writing-guidelines.md`:
- Non-technical sections are truly accessible
- Technical sections have specific method names
- All validations are listed
- Both diagrams are present and accurate
- File list is complete
- Technical decisions are justified
- Plan is complete but not verbose

## Common Pitfalls to Avoid

### Too Verbose
Don't over-explain obvious steps or include unnecessary detail. The example plan provided by the user is slightly too verbose - aim for more concise descriptions while maintaining completeness.

### Missing Diagrams
Both diagrams are required. The technical flow diagram is especially critical for catching architectural issues.

### Code in Plans
Plans should never include actual code. Describe logic, not implementation.

### Vague Descriptions
Be specific: "Call MembersService.deactivateMember(memberId, organizationId)" not "deactivate the member"

### Inconsistent Structure
If one feature has a "Validations" section, all features should have it.

## Output Format

Generate the plan as a Markdown document following the exact structure from `plan-template.md`. The plan should be:
- Complete enough for implementation without major questions
- Concise enough to review in 15-20 minutes
- Focused on architecture and business logic, not code syntax
- Consistent with other plans from the team

For medium-sized features, expect approximately 1,500-2,500 words total.

## References

This skill includes two comprehensive reference files:

**references/plan-template.md** - Complete template showing exact structure and format for every section
**references/writing-guidelines.md** - Detailed guidelines for writing quality, common mistakes, and best practices

Always review both references before generating a plan to ensure consistency and quality.