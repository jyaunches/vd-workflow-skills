---
name: vd-validate-issue
description: Validate a GitHub issue or PR with executable validation steps. Use when verifying that a PR/issue works correctly through automated testing.
---

# Validate Issue/PR

Analyze a GitHub issue/PR and execute validation to verify it works correctly.

**CRITICAL**: This command EXECUTES validation using available tools - it does NOT just propose steps.

## Arguments

- `<issue_or_pr_number>`: The GitHub issue or PR number to validate

---

## Step 1: Fetch Issue/PR Details

First, get the context from GitHub:

```bash
# Try as PR first (most common), fall back to issue
gh pr view <number> --json title,body,files,commits,state,baseRefName,headRefName 2>/dev/null || \
gh issue view <number> --json title,body,state
```

**Extract:**
- Title and description
- Files changed (for PRs)
- Related commits
- Current state

---

## Step 2: Analyze What Needs Validation

Based on the PR/issue content, identify validation needs from file patterns:

| Pattern | Inferred Validation |
|---------|---------------------|
| `.yml` workflow files | Trigger workflow, check `gh run list` status |
| `**/api/**`, `**/routes/**` | Hit endpoints with curl/httpx |
| `**/models/**`, migrations | Verify migration runs, check schema |
| `**/*_test.py`, `**/*.test.ts` | Run test suite |
| `**/cli/**`, `**/commands/**` | Verify CLI help and basic operations |
| UI components | Use browser automation if available |

### Test Coverage Check

```bash
# Check if unit tests exist for changed files
gh pr view <number> --json files -q '.files[].path' | while read f; do
  base=$(basename "$f" .py)
  test_file="tests/unit/test_${base}.py"
  [ -f "$test_file" ] && echo "Tests exist: $test_file" || echo "No tests: $test_file"
done
```

---

## Step 3: Discover Available Validation Tools

**Always Available:**
- `gh` CLI - GitHub operations, workflow status, PR checks
- `curl` - HTTP endpoint testing
- `pytest` / `jest` / `make test` - Run unit tests
- `git` - Verify commits, file changes

**Check for browser automation** if UI validation needed.

### E2E Validation Requirement (CRITICAL)

**For PRs involving user-facing workflow changes, multi-system integration, or web UI changes:**
- E2E validation is REQUIRED when browser tools are available
- Unit tests do NOT substitute for E2E
- Run both when tools are available

---

## Step 4: Design Validation Plan

Based on the analysis, create a validation plan:

```markdown
## Validation Plan for PR #<number>

### Validation Tools Available
- [ ] Browser automation: [Yes/No]
- [ ] gh CLI: [Yes]
- [ ] pytest: [Yes]

### Validation Steps

1. **[Step Name]**
   - Tool: [specific tool]
   - Action: [what to do]
   - Expected: [success criteria]

2. **[Step Name]**
   - Tool: [specific tool]
   - Action: [what to do]
   - Expected: [success criteria]
```

---

## Step 5: Execute Validation

Execute each validation step:

1. Run each step from the validation plan
2. Record results (pass/fail with details)
3. On failure: Document what failed and why
4. On success: Continue to next step

### Handle Failures

If validation steps fail:
- Document the failure clearly
- Determine if it's a test issue or code issue
- For code issues: Consider using `vd-bug` skill to fix
- For test issues: Update test expectations if appropriate

---

## Step 6: Post Results to PR

After validation completes, post results as a comment:

```bash
gh pr comment <number> --body "$(cat <<'EOF'
## Validation Results

**Status**: [PASSED/FAILED]

### Checks Performed

| Check | Status | Details |
|-------|--------|---------|
| Unit Tests | [result] | [details] |
| [Other Check] | [result] | [details] |

### Issues Found
- [List any issues discovered]

### Recommendation
- [Merge / Needs fixes / etc.]

---
*Validated using vd-validate-issue skill*
EOF
)"
```

---

## Step 7: Summary and Recommendation

After all validation completes:

**If all checks passed:**
> PR #<number> is ready to merge. All validation checks passed.

**If some checks failed:**
> PR #<number> has issues that need attention:
> - [List specific failures]
> - [Recommended fixes]

---

## Quick Reference: Common Validation Scenarios

### Scenario: Code Changes with Unit Tests
1. Run `pytest tests/` or `npm test` to verify tests pass
2. If tests exist for changed files, verify they cover the changes
3. Check code coverage if available

### Scenario: GitHub Actions Workflow Changes
1. Trigger the workflow manually: `gh workflow run <name>`
2. Watch for completion: `gh run watch`
3. Verify expected outcomes

### Scenario: API Endpoint Changes
1. Run unit tests for the endpoint
2. Use curl to hit the endpoint directly
3. Verify response format and status codes

### Scenario: Database Migration Changes
1. Verify migration runs without errors
2. Check schema matches expected state
3. Run any affected integration tests
