---
name: vd-spec-review-implementation
description: Review specification for implementation clarity and technical decisions. Use when reviewing specs for implementation questions and test coverage gaps.
---

# Spec Review - Implementation

Review a specification and its test specification for implementation clarity and technical decisions.

## Arguments

- spec_dir: Path to the spec directory containing spec.md and tests.md
- --auto-apply: Enable intelligent auto-apply mode

**Internal Path Resolution:**
- Spec file: `<spec_dir>/spec.md`
- Test spec file: `<spec_dir>/tests.md`

## Description

This performs **Phase 2: Implementation Review** - detailed analysis of implementation clarity and technical decisions. Run after completing spec-review-design (Phase 1).

### Locate PATTERNS.md

Before starting, locate PATTERNS.md:
1. `shared_docs/PATTERNS.md` (project)
2. `~/.pi/agent/shared_docs/PATTERNS.md` (global)

### Implementation Review Focus Areas

1. **Implementation Questions**: Areas that need clarification before implementation
2. **Test Coverage Gaps**: Missing test scenarios or unclear requirements
3. **Phase Alignment**: Ensure test phases align with implementation phases

## Output Format

Present Implementation Questions in table format (starting at Section 5 to continue from Phase 1):

```markdown
**Section X - Question: [What needs to be decided]**

| Option | Description | Benefits | Tradeoffs | Complexity |
|--------|-------------|----------|-----------|------------|
| A | Brief description | Key benefit | Main tradeoff | Low |
| B | Brief description | Key benefit | Main tradeoff | Medium |
| C | Brief description | Key benefit | Main tradeoff | High |

**💡 Recommendation:** Option [X] - [Clear reasoning]

**Additional Details:**
- **Option A Benefits:** • Benefit 1 • Benefit 2
- **Option A Tradeoffs:** • Tradeoff 1 • Tradeoff 2
```

## Process Flow

### Manual Mode (Default)
1. Present Implementation Questions (sections 5+)
2. User Choice: Accept all, discuss individually, or skip
3. Apply decisions to both spec and test spec files
4. Commit changes

### Auto-Apply Mode (`--auto-apply`)

**Auto-apply** (safe decisions):
- Existing implementation patterns in codebase
- Simple and consistent approaches
- Performance optimizations within existing architecture
- Standard error handling patterns
- Conventional naming and structure

**Needs approval**:
- New architectural patterns
- API design choices
- Data structure decisions
- Complexity vs simplicity tradeoffs
- Breaking changes to existing interfaces
- Technology/library choices

## Applying Decisions

For each approved section, edit the spec files and commit:

```bash
git add "<spec_dir>/spec.md" "<spec_dir>/tests.md"
git commit -m "Apply spec review recommendation from section X"
```

## Section Numbering

**Important**: Section numbering continues from Phase 1 (Design Review):
- Phase 1 sections: 1-4
- Phase 2 sections: 5, 6, 7, etc.

This allows seamless tracking across both review phases.
