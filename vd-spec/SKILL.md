# Create Specification Skill

Creates a new specification file for the project following the VD workflow standards.

## Usage

When asked to create a spec:
- Parse the feature name and description from the user's request
- Create a dated spec directory structure
- Generate a comprehensive specification file

## Instructions

### 1. Parse Request

Extract:
- Feature name (for directory naming)
- Brief description (for spec content)

### 2. Create Directory Structure

Create directory: `specs/YYYY-MM-DD_<feature_name>/`
- Use today's date in YYYY-MM-DD format
- Replace spaces in feature_name with underscores
- Make directory names lowercase

### 3. Analyze Project

Before creating the spec:

```bash
# Check existing specs structure
find specs/ -name "*.md" -type f | head -3

# Analyze source structure
ls -la src/
```

### 4. Create Specification

Create `specs/YYYY-MM-DD_<feature_name>/spec.md` with:

## Template Structure

```markdown
# [Feature Name] Specification

**Created**: [Today's Date]
**Status**: Draft

## Overview

[Brief description of the feature and why it's needed]

## Objectives

- [Primary goal]
- [Secondary goals...]

## Current State Analysis

### Existing Code
[What exists that relates to this feature]

### Gap Analysis
[What's missing that needs to be built]

## Architecture Design

### Core Components
[Brief description of main components]

### Integration Points
[How this integrates with existing code]

### Data Flow
[Include mermaid diagram if helpful]

## Configuration & Deployment

### Environment Variables
| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| NEW_VAR | Description | value | Yes/No |

### Dependencies
**pyproject.toml updates**:
```toml
[tool.poetry.dependencies]
new-package = "^1.0.0"
```

### Deployment Files to Update
- `.env.example` - Add NEW_VAR documentation
- `.github/workflows/ci.yml` - Add secrets if needed
- [Platform specific config] - Production environment

## Implementation Phases

### Phase 1: [Core Functionality Name]
**Description**: [What this phase accomplishes]

**Core Functionality**:
- [Minimal feature set]

**Unit Test Requirements**:

Tests to Write:

1. **test_should_[action]_[context]**
   - Input: [Description]
   - Expected: [Outcome]

**Acceptance Criteria**:
- [ ] [Measurable criterion]
- [ ] [Another criterion]

### Phase 2: [Enhancement Name]
[Continue pattern...]

### Phase [N]: Clean the House
**Description**: Post-implementation cleanup and documentation maintenance

**Tasks**:
1. Remove dead code
2. Update README.md
3. Update CLAUDE.md or AGENTS.md
4. Resolve TODOs

**Acceptance Criteria**:
- [ ] No commented-out code blocks remain
- [ ] Documentation reflects implementation
- [ ] All TODOs resolved or documented
```

## Key Principles

### Direct Integration
- Enhance existing code rather than creating parallel systems
- No backward compatibility unless explicitly requested
- Follow existing patterns and conventions

### Phase Design
- **Incremental**: Start with smallest buildable core
- **Buildable**: Each phase builds on previous
- **Testable**: Include comprehensive test requirements
- **Measurable**: Specific acceptance criteria

### Phase Formatting
For `/implement_phase` compatibility:
- Format: `### Phase N: [Name]`
- Word "Phase" MUST be present
- Completed phases: `### Phase N: [Name] [COMPLETED: git-sha]`

### No Code in Spec
- Architecture descriptions only
- No implementation details
- Mark testable components clearly
- Use mermaid diagrams where helpful

## Output

After creating the spec:
1. Show the created file path
2. Display the spec content
3. Suggest next steps:
   - Review with spec-review skills
   - Generate tests with vd-spec-tests
   - Start implementation with vd-implement-phase