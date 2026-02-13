---
description: Execute the complete vd_workflow from spec to implementation to validation
---

# Execute VD Workflow: $@

Execute the complete automated three-phase workflow for the specified spec directory.

## Arguments

Parse from: **$@**
- First argument: `<spec_dir>` - Path to spec directory
- Remaining: Optional extra instructions

## Spec Directory Structure

The `<spec_dir>` argument points to:
```
<spec_dir>/
├── spec.md          # Main specification
├── tests.md         # Test specification (generated during review)
└── validation.md    # Validation plan (generated during review)
```

## Workflow Overview

The workflow executes in three phases using the subagent tool:

### Phase 1: Review (review-executor agent)
1. Simplify Specification - YAGNI/complexity review
2. Generate Test Specification - Create tests.md
3. Generate Validation Plan - Create validation.md with BDD scenarios
4. Validation Review - **User approval of validation plan**
5. Design Review - Review design patterns
6. Implementation Review - Review implementation decisions

### Phase 2: Implementation (feature-writer agent)
7. Implement Phases - TDD-based phase implementation
8. Check Work - Validate acceptance criteria and test coverage

### Phase 3: Validation (validation-executor agent)
9. Execute Validation - Run BDD scenarios from validation.md
10. Fix Issues - Use vd-bug skill for failures (3 attempts per scenario)
11. Mark Complete - Add `[VALIDATED: sha]` markers

## Execution

### Step 1: Check Prerequisites

```bash
[ -d "specs" ] && echo "specs/: FOUND" || echo "specs/: NOT FOUND"
```

### Step 2: Locate PATTERNS.md

```bash
if [ -f "shared_docs/PATTERNS.md" ]; then
  echo "Using: shared_docs/PATTERNS.md"
elif [ -f "$HOME/.pi/agent/shared_docs/PATTERNS.md" ]; then
  echo "Using: ~/.pi/agent/shared_docs/PATTERNS.md"
else
  echo "WARNING: PATTERNS.md not found"
fi
```

### Step 3: Execute Review Phase

Use the **subagent tool** to invoke the review-executor agent:

**Subagent Tool Parameters:**
- **agent**: `review-executor`
- **task**: 
```
Execute review phase for spec directory: <spec_dir>

Run ALL 6 steps in sequence by loading and following each skill:
1. Load ~/.pi/agent/skills/vd-spec-simplify/SKILL.md - Execute for <spec_dir> --auto-apply
2. Load ~/.pi/agent/skills/vd-spec-tests/SKILL.md - Execute for <spec_dir>
3. Load ~/.pi/agent/skills/vd-validation-plan/SKILL.md - Execute for <spec_dir>
4. Load ~/.pi/agent/skills/vd-validation-review/SKILL.md - Execute for <spec_dir> ← PAUSES FOR USER APPROVAL
5. Load ~/.pi/agent/skills/vd-spec-review-design/SKILL.md - Execute for <spec_dir> --auto-apply
6. Load ~/.pi/agent/skills/vd-spec-review-implementation/SKILL.md - Execute for <spec_dir> --auto-apply

Internal paths:
- spec.md at <spec_dir>/spec.md
- tests.md at <spec_dir>/tests.md
- validation.md at <spec_dir>/validation.md
```

**Wait for review phase to complete before continuing.**

### Step 4: Execute Implementation Phase

Use the **subagent tool** to invoke the feature-writer agent:

**Subagent Tool Parameters:**
- **agent**: `feature-writer`
- **task**:
```
Implement feature from spec directory: <spec_dir>

LOOP through ALL phases until complete:
1. Find phases without [COMPLETED:] markers: grep -n "^## Phase" "<spec_dir>/spec.md" | grep -v "\[COMPLETED:"
2. For each incomplete phase:
   - Load ~/.pi/agent/skills/vd-implement-phase/SKILL.md
   - Execute for <spec_dir> --auto
   - Verify [COMPLETED: sha] marker added
   - LOOP BACK to step 1
3. When NO incomplete phases remain:
   - Load ~/.pi/agent/skills/vd-check-work/SKILL.md
   - Execute for <spec_dir>
   - Output completion summary

Internal paths:
- Spec: <spec_dir>/spec.md
- Test spec: <spec_dir>/tests.md
```

**Wait for implementation phase to complete before continuing.**

### Step 5: Execute Validation Phase

Use the **subagent tool** to invoke the validation-executor agent:

**Subagent Tool Parameters:**
- **agent**: `validation-executor`
- **task**:
```
Execute validation for spec directory: <spec_dir>

Validation plan: <spec_dir>/validation.md

LOOP through each scenario:
1. Parse BDD scenarios from validation.md
2. Execute each scenario using specified tools
3. On failure: Load ~/.pi/agent/skills/vd-bug/SKILL.md and execute with --auto (up to 3 attempts)
4. On pass: add [VALIDATED: sha] marker to scenario header
5. Update summary table
6. Continue until all scenarios pass or max attempts reached

Return final validation report.
```

## State Tracking

**During Review Phase:**
- `git log --oneline -N` - See review changes
- `ls <spec_dir>/` - See generated artifacts

**During Implementation Phase:**
- `grep "\[COMPLETED:" <spec_dir>/spec.md` - See completed phases

**During Validation Phase:**
- `grep "\[VALIDATED:" <spec_dir>/validation.md` - See validated scenarios

---

**Starting workflow execution for**: $@
