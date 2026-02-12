# BLS Protocol — Git Workflow Guide

**Reference for contributors and maintainers.**

---

## Repository History

```
a035eab  chore: init project scaffold                      (root)
95e391d  feat(spec): add BLS Protocol v1.0.0 core spec
ce6c433  feat(spec): add Autonomous Coding System arch     ← tag: v1.0.0
4efb027  docs(guides): add getting started guide
c5b59f0  feat(templates): add decision policy BLS template
3e1e29c  chore: add community and project management files ← tag: v1.0.1
42c8e86  feat(spec): add BLS Protocol v1.0.2 formal layer  ← tag: v1.0.2 (HEAD)
```

---

## First-Time Push to GitHub

```bash
# 1. Create repo on GitHub (no README, no .gitignore)
#    github.com/new → name: bls-protocol → Create

# 2. Add remote
git remote add origin https://github.com/YOUR_USERNAME/bls-protocol.git

# 3. Push all commits
git push -u origin main

# 4. Push all tags
git push origin --tags

# Verify
git remote -v
git log --oneline --decorate
```

---

## Commit Message Convention

Format: `<type>(<scope>): <subject>`

```
<type>(<scope>): <short subject (≤72 chars)>

<body — what and why, not how>
<blank line>
<footer — refs, breaking changes, sign-off>
```

### Types

| Type | When to use |
|------|-------------|
| `feat` | New feature, new BLS section, new logic type |
| `fix` | Corrects a bug or error in spec |
| `docs` | Documentation only (guides, tutorials, examples) |
| `chore` | Project setup, CI, tooling, dependencies |
| `refactor` | Restructure without behavioral change |
| `test` | Adding or fixing tests / test cases |
| `perf` | Performance improvements |
| `style` | Formatting, whitespace (no spec change) |
| `revert` | Revert a previous commit |

### Scopes

| Scope | What it covers |
|-------|---------------|
| `spec` | BLS Protocol specification files |
| `schema` | JSON Schema files |
| `templates` | BLS template files |
| `examples` | Example BLS files |
| `guides` | Documentation guides |
| `tools` | Tooling (validator, CLI, etc.) |
| `agents` | Discovery / BLS agent prompts |
| `ci` | GitHub Actions, CI/CD |
| `deps` | Dependency changes |

### Examples

```bash
# New feature in spec
git commit -m "feat(spec): add workflow compensation model

Introduces saga-pattern compensation for transactional workflows.
Compensation steps execute in reverse order of original execution
when a step fails with halt strategy.

Closes #45"

# Fix incorrect example
git commit -m "fix(spec): correct state machine transition example

The kenya-routing example had wrong country code (KEN vs KE).
ISO 3166-1 alpha-2 requires exactly 2 characters.

Refs #52"

# Documentation
git commit -m "docs(guides): add workflow tutorial

Step-by-step guide for creating workflow BLS files.
Includes outbound-call example with retry logic,
compensation steps, and parallel execution.

Part of #docs-series"

# Chore
git commit -m "chore(ci): add BLS validation GitHub Action

Validates all .bls.yaml files on push and PR.
Runs: yamllint, schema check, completeness check.

Refs #tooling-milestone"

# Breaking change
git commit -m "feat(spec)!: require determinism_profile in v1.1.0

BREAKING CHANGE: determinism_profile is now required.
Previous BLS files without this field will fail validation.

Migration: add determinism_profile with level: 'strict' to
all existing BLS files.

See: MIGRATION_v1.1.md"
```

### Rules

```
✅ Subject line ≤ 72 characters
✅ Subject in imperative mood ("add" not "added", "fix" not "fixed")
✅ No period at end of subject
✅ Body separated by blank line
✅ Body explains WHY, not what (git diff shows what)
✅ Reference issues in footer: "Closes #n" or "Refs #n"
✅ Sign-off on all commits: "Signed-off-by: Name <email>"
❌ No "WIP" commits on main
❌ No merge commits on main (use rebase)
❌ No commit messages like "fix", "update", "changes"
```

---

## Branch Strategy

```
main                    ← stable, always deployable, tagged releases
├── feat/spec-v1.1      ← new specification version
├── feat/validator-tool ← new tooling
├── fix/state-machine   ← bug fix
├── docs/workflow-guide ← documentation
└── chore/ci-setup      ← project tooling
```

### Branch Naming

```bash
feat/<feature-name>     # New features
fix/<issue-or-name>     # Bug fixes
docs/<topic>            # Documentation
chore/<task>            # Project tooling
refactor/<area>         # Restructuring
release/<version>       # Release preparation
```

### Branch Workflow

```bash
# 1. Create branch from main
git checkout main
git pull origin main
git checkout -b feat/add-workflow-template

# 2. Work and commit
git add templates/workflow.bls.yaml
git commit -m "feat(templates): add workflow BLS template"

# 3. Keep branch updated
git fetch origin
git rebase origin/main

# 4. Push branch
git push origin feat/add-workflow-template

# 5. Open Pull Request on GitHub
#    Title: "feat(templates): add workflow BLS template"
#    Description: What, why, how to test

# 6. After approval — squash merge to main
git checkout main
git merge --squash feat/add-workflow-template
git commit -m "feat(templates): add workflow BLS template"

# 7. Delete branch
git branch -d feat/add-workflow-template
git push origin --delete feat/add-workflow-template
```

---

## Tagging Strategy

### Tag Format

```
v<MAJOR>.<MINOR>.<PATCH>

v1.0.0  — Initial release
v1.0.1  — Backward-compatible additions
v1.0.2  — More additions (no breaking changes)
v1.1.0  — New features, backward compatible
v2.0.0  — Breaking changes
```

### Creating a Tag

```bash
# Annotated tag (always use for releases)
git tag -a v1.1.0 -m "BLS Protocol v1.1.0

Short description of this release.

New features:
  - Feature 1
  - Feature 2

Bug fixes:
  - Fix 1

Breaking changes: None
Migration: Not required"

# Push tag
git push origin v1.1.0

# Push all tags at once
git push origin --tags
```

### Listing Tags

```bash
# List with messages
git tag -l -n1

# Detailed
git tag -l -n3

# Show specific tag
git show v1.0.2
```

### Deleting a Tag (emergency only)

```bash
# Local
git tag -d v1.0.2

# Remote (requires force — coordinate with team first)
git push origin --delete v1.0.2
```

---

## Release Checklist

### Before Tagging

```bash
# 1. All tests pass
bls validate docs/specifications/BLS_Protocol_v1.0.2.md

# 2. CHANGELOG updated
vim CHANGELOG.md

# 3. README version badge updated
sed -i 's/Version-1.0.1/Version-1.0.2/g' README.md

# 4. All commits on main
git log --oneline

# 5. No uncommitted changes
git status
```

### Tagging

```bash
git tag -a v1.0.2 -m "BLS Protocol v1.0.2 — <one-line summary>

<paragraph describing this release>

New:
  - Item 1
  - Item 2

Fixed:
  - Item 1

Breaking changes: None | <describe if any>
Migration: Not required | <link to migration guide>"
```

### After Tagging

```bash
# Push commits and tag
git push origin main
git push origin v1.0.2

# Create GitHub Release
# Go to: github.com/YOUR_ORG/bls-protocol/releases/new
# Tag: v1.0.2
# Title: BLS Protocol v1.0.2 — Formal Protocol Layer
# Body: paste from CHANGELOG.md
# Attach: any release artifacts
# Publish release
```

---

## Useful Git Commands

### Log Formats

```bash
# Clean one-line with tags
git log --oneline --decorate

# With dates
git log --pretty=format:"%h %ad %s" --date=short

# With graph
git log --oneline --graph --all

# Show specific version changes
git log v1.0.1..v1.0.2 --oneline

# Show file changes between versions
git diff v1.0.1..v1.0.2 -- docs/specifications/
```

### Undoing Things

```bash
# Undo last commit (keep changes)
git reset --soft HEAD~1

# Fix last commit message
git commit --amend -m "corrected message"

# Undo staged changes
git restore --staged <file>

# Discard working changes
git restore <file>
```

### Inspection

```bash
# What changed in last commit
git show HEAD

# What's in a specific tag
git show v1.0.2

# Compare two tags
git diff v1.0.1 v1.0.2

# Who changed what
git blame docs/specifications/BLS_Protocol_v1.0.2.md
```

---

## .github/ Directory Structure

Create these files for full GitHub integration:

```
.github/
├── ISSUE_TEMPLATE/
│   ├── bug_report.md
│   ├── feature_request.md
│   └── new_example.md
├── PULL_REQUEST_TEMPLATE.md
└── workflows/
    ├── validate.yml          # Validate BLS files on PR
    └── release.yml           # Auto-create release on tag
```

### PR Template (.github/PULL_REQUEST_TEMPLATE.md)

```markdown
## What does this PR do?

Brief description.

## Type of change

- [ ] Bug fix (non-breaking)
- [ ] New feature (non-breaking)
- [ ] Breaking change
- [ ] Documentation update

## Checklist

- [ ] Follows commit message convention
- [ ] CHANGELOG.md updated
- [ ] BLS files validated
- [ ] Tests added (if applicable)
- [ ] Documentation updated

## Related issues

Closes #
```

### Validate Workflow (.github/workflows/validate.yml)

```yaml
name: Validate BLS Files

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Validate YAML syntax
        run: |
          pip install yamllint
          find . -name "*.bls.yaml" | xargs yamllint

      - name: Check required BLS fields
        run: |
          python scripts/validate_bls.py docs/ templates/ examples/

      - name: Spell check docs
        uses: streetsidesoftware/cspell-action@v2
        with:
          files: "docs/**/*.md"
```

---

## Summary: The Commit Flow

```
Write BLS change
      ↓
git checkout -b feat/my-change
      ↓
git add <files>
      ↓
git commit -m "feat(spec): describe what and why"
      ↓
git push origin feat/my-change
      ↓
Open Pull Request
      ↓
Review + Approve
      ↓
Squash merge to main
      ↓
(If release) git tag -a vX.Y.Z + git push origin vX.Y.Z
      ↓
Create GitHub Release
```

---

**Version:** 1.0  
**Maintained by:** BLS Protocol Core Team
