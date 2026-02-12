---
name: vd-implement-phase
description: Execute implementation phases from specification using test-driven development. Use when implementing feature phases with TDD workflow.
---

# Implement Phase

Execute implementation phases from the specified spec directory following rigorous test-driven development.

**Internal Path Resolution:**
- Spec file: `<spec_dir>/spec.md`
- Test spec file: `<spec_dir>/tests.md`

## Mode Detection

```bash
if [[ "$*" == *"--auto"* ]]; then
    echo "🚀 AUTO MODE: Will proceed without user approval"
else
    echo "📋 INTERACTIVE MODE: Will wait for approval at each phase"
fi
```

## ⚠️ CRITICAL: AUTO MODE BEHAVIOR

When `--auto` flag is present:
- **NEVER pause** for user confirmation
- **NEVER ask questions** or wait for user input
- **ALWAYS proceed** automatically through all 7 steps
- The ONLY reasons to stop: actual errors, test failures, or missing dependencies

## Phase Analysis

```bash
SPEC_DIR="${1%/}"
SPEC_FILE="$SPEC_DIR/spec.md"
TEST_SPEC_FILE="$SPEC_DIR/tests.md"

echo "\n=== Next Phase Analysis ==="
grep -n "^## Phase" "$SPEC_FILE" | grep -v "\[COMPLETED:" | head -1

echo "\n=== Completed Phases ==="
grep -n "\[COMPLETED:" "$SPEC_FILE" || echo "No completed phases found"
```

## 7-Step Phase Execution Workflow

### Step 1: Phase Review & Alignment
- Review the phase to understand scope and requirements
- Review how/where to write unit tests
- Prepare TDD approach
- **INTERACTIVE**: Wait for user confirmation
- **AUTO**: Immediately proceed to Step 2

### Step 2: Write Tests First (Red)
- Use test specification from tests.md
- Follow test guide exactly
- Ensure tests fail (red phase of TDD)
- Commit failing tests:
  ```bash
  git add tests/
  git commit -m "test: Add failing tests for Phase X"
  ```

### Step 3: Implement Code (Green)
- Write minimal code to make tests pass
- Follow existing patterns
- Only implement what's necessary

### Step 4: Commit Implementation
- Verify all tests pass
- Commit:
  ```bash
  git add .
  git commit -m "feat: Implement Phase X - [description]"
  ```

### Step 5: Validate Acceptance Criteria
- Review phase acceptance criteria
- Determine if additional test/implementation cycles needed
- Repeat steps 2-4 if criteria require more functionality

### Step 6: Phase Completion Review
- Verify completed phase against acceptance criteria
- **INTERACTIVE**: Present for review, get explicit agreement
- **AUTO**: Validate tests pass, proceed immediately

### Step 7: Update Specification
- Mark phase as completed:
  ```bash
  SHA=$(git rev-parse --short HEAD)
  # Edit spec file to add [COMPLETED: $SHA] to phase header
  git add "$SPEC_FILE"
  git commit -m "Mark Phase X as completed [$SHA]"
  ```

## Implementation Strategy

- **Direct integration** into existing files
- **No backward compatibility** - always direct integration
- **Follow existing patterns** and conventions
- **Use uv for dependencies** (never pip install)
- **Maintain type annotations** and dataclass patterns

## Phase Header Format

Before:
```markdown
## Phase 1: Core Data Structure
```

After:
```markdown
## Phase 1: Core Data Structure [COMPLETED: a1b2c3d]
```
