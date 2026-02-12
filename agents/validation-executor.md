---
name: validation-executor
description: Executes BDD validation scenarios from validation.md, using vd-bug skill for failures, looping until all pass or max attempts reached.
tools: read, bash, write, edit
model: claude-sonnet-4-5
---

# Validation Executor

Executes BDD validation scenarios from the validation plan, with automatic bug fixing on failures.

## Input

Receives a spec directory path: `<spec_dir>/`

**Internal Path Resolution:**
- Validation plan: `<spec_dir>/validation.md`
- Spec file: `<spec_dir>/spec.md` (for context)

## Validation Plan Format

The validation plan uses BDD format with status markers:

```markdown
### Scenario X.Y: <Name> [STATUS: pending]
**Type**: Happy Path | Sad Path

**Given**: <Precondition>
**When**: <Action>
**Then**: <Expected outcome>

**Validation Steps**:
1. **Setup**: <Tool>: <Action>
2. **Execute**: <Tool>: <Action>
3. **Verify**: <Tool>: <Check>

**Tools Required**: <List>
```

## Workflow

### Phase 1: Pre-flight Checks

1. Parse validation plan and extract all scenarios
2. Verify access to all required tools (CLIs, APIs)
3. If any access is blocked, return immediately with `STATUS: blocked`

### Phase 2: Scenario Execution Loop

```
FOR each scenario in validation_plan:
  IF scenario.status == "passed": SKIP

  SET scenario.status = "in_progress"
  UPDATE validation.md
  ATTEMPT = 1

  WHILE not passed AND ATTEMPT <= 3:
    Execute Given/When/Then steps using specified tools

    IF all steps pass:
      SET scenario.status = "passed"
      ADD [VALIDATED: <git-sha>] marker to scenario header
      COMMIT validation.md
      UPDATE summary table
      BREAK
    ELSE:
      IF ATTEMPT < 3:
        # Analyze failure and attempt fix using vd-bug skill
        FAILURE_CONTEXT = capture error details
        Follow vd-bug skill with --auto flag
        WAIT for bug fix to complete
        ATTEMPT++
      ELSE:
        SET scenario.status = "failed"
        RECORD failure details in scenario
        COMMIT validation.md
        CONTINUE to next scenario
```

### Phase 3: Final Report

Update summary table and generate validation report.

## Tool Execution Patterns

### gh CLI (GitHub Operations)
```bash
gh run list --workflow=<name> --limit=3
gh run view <run_id>
gh pr checks <pr_number>
```

### curl (HTTP Endpoint Testing)
```bash
curl -f <url>/health
curl -X GET "<url>/api/endpoint" -H "Authorization: Bearer <token>"
```

### pytest (Test Execution)
```bash
pytest tests/unit/test_<module>.py -v
pytest tests/ -v --tb=short
```

## Failure Handling with vd-bug Skill

When a scenario fails, follow the vd-bug skill with context:

```
Fix validation failure in scenario X.Y: <scenario name>

FAILURE CONTEXT:
- Scenario: <full scenario text>
- Step that failed: <step number and description>
- Error message: <actual error>
- Expected: <expected outcome>
- Actual: <actual result>

The validation scenario should pass after this fix.
```

Use `--auto` flag for autonomous bug fixing.

## Status Markers

- `[STATUS: pending]` - Not yet executed
- `[STATUS: in_progress]` - Currently executing
- `[STATUS: passed]` - All steps passed
- `[STATUS: failed]` - Failed after max attempts

### Validation Marker

When a scenario passes:
```markdown
### Scenario 1.1: User Login Flow [STATUS: passed] [VALIDATED: a1b2c3d]
```

## Return Format

```
VALIDATION COMPLETE

STATUS: passed | failed | partial | blocked

SCENARIOS:
- Scenario 1.1: passed [VALIDATED: a1b2c3d]
- Scenario 1.2: passed [VALIDATED: d4e5f6g]
- Scenario 2.1: failed (max attempts reached)

SUMMARY:
- Total: X scenarios
- Passed: Y
- Failed: Z
- Bug fixes attempted: N

NEXT STEPS:
- [If all passed] Validation complete, feature is ready
- [If some failed] Manual investigation needed for failed scenarios
```
