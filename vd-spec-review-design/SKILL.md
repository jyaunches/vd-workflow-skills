---
name: vd-spec-review-design
description: Review specification for design improvements and architectural alignment. Use when reviewing specs for Pythonic patterns, simplification, and reuse opportunities.
---

# Spec Review - Design

Review a specification and its test specification for design improvements and architectural alignment.

## Arguments

- spec_dir: Path to the spec directory containing spec.md and tests.md
- --auto-apply: Enable intelligent auto-apply mode for automated workflows

**Internal Path Resolution:**
- Spec file: `<spec_dir>/spec.md`
- Test spec file: `<spec_dir>/tests.md`

## Description

This performs **Phase 1: Design Review** of both a specification file and its test specification. Run after completing spec-simplify.

### Locate PATTERNS.md

Before starting, locate PATTERNS.md:
1. `shared_docs/PATTERNS.md` (project)
2. `~/.pi/agent/shared_docs/PATTERNS.md` (global)

Read PATTERNS.md to understand which recommendations align with project standards.

### Phase 1: Design Review Areas

1. **Simplify Design**: Identify overly complex approaches and suggest simpler alternatives
2. **Pythonic Patterns**: Recommend Python-specific patterns and libraries
3. **Architecture Alignment**: Find opportunities to reuse existing code
4. **Conciseness**: Suggest ways to make spec more focused

## Output Format

Present structured recommendations with numbered sections:

```markdown
### Section X: [Category]

**Current Approach:** [What the spec currently describes]

**Recommended Change:** [Specific suggestion]

**Rationale:** [Why this change is beneficial]

**Impact:** [How this affects other parts]
```

### Categories

- **Section 1 - Simplification Opportunities**: Reduce complexity
- **Section 2 - Pythonic Improvements**: Better Python features
- **Section 3 - Reuse Opportunities**: Existing code to leverage
- **Section 4 - Recommended Libraries**: Packages to simplify implementation

## Process Flow

### Manual Mode (Default)
1. Present Design Recommendations
2. User Choice: Accept all, discuss individually, or skip
3. Apply changes to both spec and test spec files
4. Commit changes

### Auto-Apply Mode (`--auto-apply`)

**Auto-apply** (safe recommendations):
- Pythonic improvements (dataclasses, type hints, context managers)
- Using existing module patterns
- Leveraging stdlib features
- Established libraries already in use
- Simplifications that reduce complexity

**Needs approval**:
- New architectural patterns
- New external dependencies
- Breaking API changes
- Major refactoring

## Applying Recommendations

For each approved section, edit the spec files directly and commit:

```bash
git add "<spec_dir>/spec.md" "<spec_dir>/tests.md"
git commit -m "Apply spec review recommendation from section X"
```

## Change Tracking

All design improvements are tracked via git commits:
- Each applied section creates a separate commit
- View changes: `git log --oneline`
- Detailed history: `git show <commit-sha>`
