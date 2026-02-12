---
name: vd-bug
description: Fix a bug using Test-Driven Development (TDD) methodology with optional automated execution. Use when fixing bugs with TDD approach.
---

# Bug Fix (TDD)

Fix a bug using Test-Driven Development methodology.

## Arguments

- description: Detailed description of the bug
- --auto: Enable automated execution without user approval

## Mode Detection

```bash
if [[ "$*" == *"--auto"* ]]; then
    echo "🚀 AUTO MODE: Will proceed through all phases without user approval"
else
    echo "📋 INTERACTIVE MODE: Will wait for approval at each phase"
fi
```

## Process

### Phase 1: Research & Planning

1. Analyze the bug description
2. **Reproduce the bug:**
   - Run CLI commands that trigger the issue
   - Document exact steps, inputs, outputs
   - Capture error messages, stack traces
3. Search codebase to locate relevant code
4. Identify root cause
5. Determine which files need modification
6. **Present findings:**
   - Bug reproduction steps and results
   - Root cause analysis
   - Files to modify
   - Recommended fix approach
7. **INTERACTIVE**: Wait for user approval
8. **AUTO**: Proceed immediately to Phase 2

### Phase 2: Write Failing Test (Red)

1. Write a test that reproduces the bug
2. **Choose appropriate test file location:**
   - Add to existing test file if tests for module exist
   - Create new logical test file only if needed
   - **Never create bug-specific files** like `test_xyz_bug.py`
3. Run test to confirm it fails
4. Commit failing test:
   ```bash
   git add tests/
   git commit -m "test: Add failing test for [bug description]

   This test demonstrates the bug where [specific issue].
   Expected: [expected behavior]
   Actual: [current broken behavior]"
   ```
5. **INTERACTIVE**: Wait for confirmation
6. **AUTO**: Proceed to Phase 3

### Phase 3: Implement Fix (Green)

1. **INTERACTIVE**: Present implementation approach, wait for approval
2. **AUTO**: Proceed with implementation
3. Implement minimal code change to make test pass
4. Run test to confirm it passes
5. Run related tests to ensure no regressions
6. Commit fix:
   ```bash
   git add .
   git commit -m "fix: [Concise description]

   Resolves issue where [bug description].
   The fix [brief explanation of solution]."
   ```

### Phase 4: Refactor (Optional)

1. Evaluate if refactoring is needed
2. **INTERACTIVE**: Discuss with user, get approval
3. **AUTO**: Only refactor if clearly necessary and safe
4. Ensure all tests still pass
5. Commit refactoring separately

## Test File Organization

- **Add to existing**: If tests for the module exist, add there
- **Create new logical file**: Only if no appropriate test file exists
- **Never create bug-specific files**: Avoid `test_xyz_bug.py` or `test_issue_123.py`
- **Follow existing organization**: Check how similar tests are organized

## Output

### Phase 1 Output
- Summary of bug research findings
- Root cause analysis
- Proposed fix approach
- Files to be modified

### Phase 2 Output
- The failing test code
- Test execution output showing failure
- Explanation of what the test validates

### Phase 3 Output
- The fix implementation
- Test execution showing it passes
- Any related tests that were run

### Phase 4 Output (if applicable)
- Refactoring changes
- Confirmation all tests still pass