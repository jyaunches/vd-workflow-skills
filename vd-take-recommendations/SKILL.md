---
name: vd-take-recommendations
description: Apply specific recommendations from spec-review by section number. Use when selectively applying review recommendations.
---

# Take Recommendations

Apply specific recommendations from a spec-review session by section number, updating both spec and test spec files incrementally.

## Arguments

- sections: Comma-separated list of section numbers (e.g., "1, 2, 4" or "1,3,5")

## Usage

```
sections: 1, 2, 4
sections: 1,3,5
sections: 2
```

This is designed to work during an active spec review session where numbered sections have been presented.

## Description

Apply recommendations from specific sections in an iterative fashion:

1. **Parse Section Numbers**: Extract section numbers from input
2. **Process Each Section**: For each specified section:
   - Apply the recommendation to the spec file
   - Apply corresponding changes to the test spec file
   - Commit the changes with a descriptive message
   - Move to the next section
3. **Report Progress**: Show which sections have been processed

## Process Flow

For each section number:

1. **Apply to Spec File**: Update main specification
2. **Apply to Test Spec File**: Update test specification
3. **Commit Changes**:
   ```bash
   git add "<spec_dir>/spec.md" "<spec_dir>/tests.md"
   git commit -m "Apply spec review recommendation from section X"
   ```
4. **Continue**: Move to next section

## Output Format

```
Processing Section X...
✓ Updated spec file: [brief description]
✓ Updated test spec file: [brief description]  
✓ Committed changes: Apply spec review recommendation from section X

Processing Section Y...
[continues for each section]

Summary:
- Processed sections: X, Y, Z
- Total commits: 3
- Files updated: spec.md, tests.md
```

## Section Reference

**Phase 1 Sections (Design Review):**
- Section 1 - Simplification Opportunities
- Section 2 - Pythonic Improvements  
- Section 3 - Reuse Opportunities
- Section 4 - Recommended Libraries

**Phase 2 Sections (Implementation Review):**
- Section 5+ - Implementation Questions (numbered sequentially)

## Error Handling

- **Invalid Section Numbers**: Skip and report
- **Missing Files**: Report error and stop
- **Git Issues**: Report and stop to avoid inconsistent state
