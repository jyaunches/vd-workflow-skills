---
description: Create a new specification file for a feature using vd_workflow structure
---

# Create Specification: $@

I'll create a comprehensive specification for this feature following the vd_workflow standards.

## Arguments

Parse from: **$@**
- First argument: `<feature_name>` 
- Second argument: `"<description>"` (quoted)

## Analysis

Let me analyze the existing codebase structure:

```bash
find specs/ -name "*.md" -type f | head -3
```

```bash
ls -la src/
```

## Direct Integration Approach

- **Enhance Existing Code**: Modify current files rather than creating parallel systems
- **No Backward Compatibility**: Unless explicitly requested, new features become default behavior
- **Follow Existing Patterns**: Integrate with current architecture and conventions

## Specification Structure

The specification will include:

1. **Overview & Objectives**: Clear problem statement and goals
2. **Current State Analysis**: What exists vs. what's needed
3. **Architecture Design**: Implementation approach (no code, mermaid charts where appropriate)
4. **Configuration & Deployment Changes**: Environment variables, dependencies, deployment files
5. **Implementation Phases**: Incremental development phases with integrated acceptance criteria

## Phase Design Principles

1. **Incremental**: Phase 1 = smallest buildable core functionality
2. **Buildable**: Each phase builds on previous phases
3. **Testable**: Each phase includes comprehensive unit test requirements
4. **Measurable**: Each phase has specific acceptance criteria

## Phase Header Format (CRITICAL)

For implement-phase compatibility, phases MUST use:
```markdown
## Phase 1: [Descriptive Name]
## Phase 2: [Descriptive Name]
## Phase 3: [Descriptive Name] [COMPLETED: a1b2c3d]
```

## Final Phase: Clean the House

Every spec MUST include a final cleanup phase:
- Remove dead code
- Update README.md
- Update CLAUDE.md or AGENTS.md
- Resolve TODOs from implementation

## File Creation

Create specification in: `specs/YYYY-MM-DD_<feature_name>/`

**Directory Structure:**
```
specs/
└── YYYY-MM-DD_<feature_name>/
    ├── spec.md          # Main specification (created now)
    ├── tests.md         # Test specification (created by vd-spec-tests)
    └── validation.md    # Validation plan (created by vd-validation-plan)
```

---

**Creating detailed specification for**: $@
