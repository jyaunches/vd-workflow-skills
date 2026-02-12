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

Use the subagent tool to invoke the review-executor agent:

```
Invoke subagent: review-executor
Task: Execute review phase for spec directory: <spec_dir>

Run these steps in sequence:
1. vd-spec-simplify <spec_dir> --auto-apply
2. vd-spec-tests <spec_dir>
3. vd-validation-plan <spec_dir>
4. vd-validation-review <spec_dir>  ← PAUSES FOR USER APPROVAL
5. vd-spec-review-design <spec_dir> --auto-apply
6. vd-spec-review-implementation <spec_dir> --auto-apply
```

### Step 4: Execute Implementation Phase

Use the subagent tool to invoke the feature-writer agent:

```
Invoke subagent: feature-writer
Task: Implement feature from spec directory: <spec_dir>

Process phases sequentially:
1. Find phases without [COMPLETED:] markers
2. Execute vd-implement-phase <spec_dir> --auto for each
3. When all phases complete, run vd-check-work <spec_dir>
```

### Step 5: Execute Validation Phase

Use the subagent tool to invoke the validation-executor agent:

```
Invoke subagent: validation-executor
Task: Execute validation for spec directory: <spec_dir>

Loop through scenarios:
1. Parse BDD scenarios from validation.md
2. Execute each scenario
3. On failure: use vd-bug --auto (up to 3 attempts)
4. On pass: add [VALIDATED: sha] marker
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
