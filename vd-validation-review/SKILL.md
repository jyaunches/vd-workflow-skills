---
name: vd-validation-review
description: Review and approve validation plan before execution. Use when reviewing BDD scenarios for user approval.
---

# Validation Review

Present the validation plan for user review and approval before implementation begins.

**Internal Path Resolution:**
- Validation plan: `<spec_dir>/validation.md`

## Step 1: Read Validation Plan

```bash
SPEC_DIR="${1%/}"
VALIDATION_FILE="$SPEC_DIR/validation.md"

echo "=== Validation Plan ==="
cat "$VALIDATION_FILE"
```

## Step 2: Present Coverage Summary

Extract and present key metrics:

### Coverage Summary

| Metric | Count |
|--------|-------|
| **Happy Paths** | X scenarios |
| **Sad Paths** | Y scenarios |
| **Total** | Z scenarios |

### Tools Required

- Tool 1: Used for X scenarios
- Tool 2: Used for Y scenarios

## Step 3: User Review

**IMPORTANT: This step requires user interaction.**

Present the validation plan and ask user to confirm:

> **Review Checklist:**
> - [ ] All critical user flows are covered (happy paths)
> - [ ] Obvious failure modes are tested (sad paths)
> - [ ] No missing edge cases that should be validated
> - [ ] Tools specified are available and appropriate
> - [ ] Scenarios are executable (not vague or impossible)

### Review Options

Ask the user to choose:

1. **Approve as-is** - The validation plan is complete and ready
2. **Add scenarios** - Specify additional scenarios to include
3. **Remove scenarios** - Identify scenarios that are unnecessary
4. **Modify scenarios** - Request changes to specific scenarios
5. **Regenerate** - Start over with different focus

## Step 4: Handle User Feedback

### If "Approve as-is"

Commit the approved validation plan:

```bash
git add "$VALIDATION_FILE"
git commit -m "Approve validation plan for $(basename $SPEC_DIR)"
```

### If modifications requested

Update the validation plan based on feedback, then re-present for approval.

## Step 5: Final Approval

Once approved:

```markdown
## Validation Plan Approved

**Spec Directory**: <spec_dir>
**Validation Plan**: <spec_dir>/validation.md

**Coverage**:
- Happy Paths: X
- Sad Paths: Y
- Total: Z scenarios

**Status**: APPROVED - Ready for implementation

**Next Steps**:
1. Implementation phase will proceed
2. After implementation, validation-executor will run these scenarios
3. Failed scenarios trigger bug fixes (up to 3 attempts)
4. Passed scenarios get [VALIDATED: sha] markers
```

## Why User Review Matters

- **Coverage completeness**: You know your feature better than automated generation
- **Tool availability**: Confirm the tools are actually accessible
- **Priority alignment**: Ensure critical paths are covered
- **Feasibility check**: Some scenarios may be impossible to execute automatically
