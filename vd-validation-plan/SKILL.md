---
name: vd-validation-plan
description: Generate BDD validation plan with Given/When/Then scenarios. Use when creating end-to-end validation scenarios for features.
---

# Validation Plan

Generate a comprehensive BDD-style validation plan for the specified spec directory.

**Internal Path Resolution:**
- Spec file: `<spec_dir>/spec.md`
- Test spec file: `<spec_dir>/tests.md`
- Output: `<spec_dir>/validation.md`

## Step 1: Analyze Specification and Test Spec

Read the spec and test spec to understand:
- Feature phases and acceptance criteria
- Unit test coverage (to avoid duplication)
- Integration points that need E2E validation

```bash
SPEC_DIR="${1%/}"
SPEC_FILE="$SPEC_DIR/spec.md"
TEST_SPEC_FILE="$SPEC_DIR/tests.md"

cat "$SPEC_FILE"
cat "$TEST_SPEC_FILE" 2>/dev/null || echo "No test spec found yet"
```

## Step 2: Discover Available Validation Tools

Check what validation tools are available:

```bash
# Check for test frameworks and CLIs
cat package.json 2>/dev/null | grep -E "(playwright|puppeteer|cypress|jest)" || true
grep -E "(pytest|httpx|playwright)" pyproject.toml 2>/dev/null || true
ls .github/workflows/*.yml 2>/dev/null | head -3 || true
which gh && echo "gh CLI available" || true
```

## Step 3: Generate BDD Scenarios

For each spec phase, generate BDD scenarios covering:

### Happy Paths
- Complete user journeys that exercise the feature end-to-end
- All major success flows

### Sad Paths (Obvious Failures)
- Invalid input - What happens with bad data?
- Auth failures - What happens without proper permissions?
- Missing data - What happens when required data is absent?
- Edge cases - Boundary conditions that could fail

### Scenario Structure

```markdown
### Scenario X.Y: <Descriptive Name> [STATUS: pending]
**Type**: Happy Path | Sad Path

**Given**: <Precondition - system state, user state, data setup>
**When**: <Action - what the user/system does>
**Then**: <Outcome - observable result that proves it works>

**Validation Steps**:
1. **Setup**: <Tool>: <Setup action>
2. **Execute**: <Tool>: <Trigger action>
3. **Verify**: <Tool>: <Check expected state>

**Tools Required**: <List of tools needed>
```

## Step 4: Generate Validation Plan File

Create `<spec_dir>/validation.md` with this format:

```markdown
# Validation Plan: <Feature Name>

Generated from: <spec_dir>/spec.md
Test Spec: <spec_dir>/tests.md

## Overview
**Feature**: <Brief description>
**Available Tools**: <Playwright, gh CLI, curl, pytest, etc.>

## Coverage Summary
- Happy Paths: <count> scenarios
- Sad Paths: <count> scenarios
- Total: <count> scenarios

---

## Phase 1: <Phase Name> - Validation Scenarios

### Scenario 1.1: <Happy Path Name> [STATUS: pending]
...

## Summary

| Phase | Happy | Sad | Total | Passed | Failed | Pending |
|-------|-------|-----|-------|--------|--------|---------|
| Phase 1 | X | Y | Z | 0 | 0 | Z |
| **Total** | **X** | **Y** | **Z** | **0** | **0** | **Z** |
```

## Step 5: Commit Validation Plan

```bash
git add "$SPEC_DIR/validation.md"
git commit -m "Add validation plan for $(basename $SPEC_DIR)"
```

## Tool Selection

| Validation Need | Tool |
|----------------|------|
| Browser UI interaction | Playwright |
| Database state verification | Direct SQL |
| API endpoint testing | curl / httpx |
| GitHub PR/workflow checks | gh CLI |
| File system verification | Bash |
| Running test suites | pytest / jest |
