# VD Workflow Skills

A collection of [pi coding agent](https://github.com/badlogic/pi-mono) skills for spec-driven, test-driven development workflows.

## Installation

Clone this repo and symlink or copy the skill directories to your pi skills folder:

```bash
git clone https://github.com/jyaunches/vd-workflow-skills.git
cd vd-workflow-skills

# Option 1: Symlink all skills
for skill in vd-*/; do
  ln -sf "$(pwd)/$skill" ~/.pi/agent/skills/
done

# Option 2: Copy all skills
cp -r vd-*/ ~/.pi/agent/skills/
```

## Skills Overview

### Specification & Review
| Skill | Description |
|-------|-------------|
| `vd-spec-review-architecture` | Review specs for architectural concerns and design gaps |
| `vd-spec-review-design` | Review specs for Pythonic patterns, simplification, reuse |
| `vd-spec-review-implementation` | Review specs for implementation clarity and test coverage |
| `vd-spec-simplify` | Remove unnecessary complexity and YAGNI violations |
| `vd-spec-tests` | Generate test guides aligned with spec phases for TDD |
| `vd-take-recommendations` | Apply specific recommendations from spec-review by section |

### Implementation
| Skill | Description |
|-------|-------------|
| `vd-implement-phase` | Execute implementation phases using TDD workflow |
| `vd-bug` | Fix bugs using Test-Driven Development methodology |
| `vd-fix-tests` | Run tests, identify failures, fix with commit context |
| `vd-check-work` | Validate implementation against acceptance criteria |

### Validation
| Skill | Description |
|-------|-------------|
| `vd-validation-plan` | Generate BDD validation plans with Given/When/Then scenarios |
| `vd-validation-review` | Review and approve validation plans before execution |
| `vd-validate-issue` | Validate GitHub issues/PRs with executable validation steps |

### Utilities
| Skill | Description |
|-------|-------------|
| `vd-git-session-cleanup` | Clean up temporary files created during dev sessions |

## Usage

Skills are automatically loaded by pi when the task matches their description. You can also invoke them explicitly:

```
# In pi, just describe what you want:
"implement phase 2 from the spec"
"review this spec for architectural issues"
"fix the failing tests"
```

## License

MIT
