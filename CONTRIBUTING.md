# Contributing to BLS Protocol

Thank you for your interest in contributing to the BLS Protocol! This document provides guidelines and instructions for contributing.

## Table of Contents

1. [Code of Conduct](#code-of-conduct)
2. [Getting Started](#getting-started)
3. [How to Contribute](#how-to-contribute)
4. [Development Setup](#development-setup)
5. [Submission Guidelines](#submission-guidelines)
6. [Style Guides](#style-guides)
7. [Community](#community)

---

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inclusive environment for everyone, regardless of:
- Age
- Body size
- Disability
- Ethnicity
- Gender identity and expression
- Level of experience
- Nationality
- Personal appearance
- Race
- Religion
- Sexual identity and orientation

### Our Standards

**Positive behavior includes:**
- Using welcoming and inclusive language
- Being respectful of differing viewpoints
- Gracefully accepting constructive criticism
- Focusing on what is best for the community
- Showing empathy towards other community members

**Unacceptable behavior includes:**
- Trolling, insulting/derogatory comments, and personal attacks
- Public or private harassment
- Publishing others' private information without permission
- Other conduct which could reasonably be considered inappropriate

### Enforcement

Violations of the Code of Conduct may be reported to conduct@bls-protocol.org. All complaints will be reviewed and investigated.

---

## Getting Started

### Prerequisites

- Git
- Text editor (VS Code recommended)
- Basic understanding of YAML/JSON
- Familiarity with business logic concepts

### First Time Setup

1. **Fork the repository**
   ```bash
   # Click "Fork" on GitHub
   ```

2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR_USERNAME/bls-protocol.git
   cd bls-protocol
   ```

3. **Add upstream remote**
   ```bash
   git remote add upstream https://github.com/bls-protocol/bls-protocol.git
   ```

4. **Create a branch**
   ```bash
   git checkout -b feature/my-contribution
   ```

---

## How to Contribute

### Ways to Contribute

We welcome contributions in many forms:

#### 📝 Documentation
- Fix typos or clarify existing docs
- Add examples
- Write tutorials
- Translate documentation

#### 🐛 Bug Reports
- Report issues with specifications
- Identify inconsistencies
- Suggest improvements

#### ✨ New Features
- Propose new logic types
- Suggest domain extensions
- Design new tools

#### 💡 Examples
- Share real-world BLS files
- Create industry-specific examples
- Document use cases

#### 🔧 Tools
- Build validators
- Create visualizers
- Develop IDE extensions

---

## Development Setup

### For Documentation Changes

1. **Edit markdown files**
   ```bash
   vim docs/guides/getting-started.md
   ```

2. **Preview changes**
   ```bash
   # Use any markdown previewer
   ```

3. **Commit changes**
   ```bash
   git add docs/
   git commit -m "docs: improve getting started guide"
   ```

### For Specification Changes

1. **Update specification**
   ```bash
   vim docs/specifications/BLS_Protocol_v1.0.1.md
   ```

2. **Update schemas if needed**
   ```bash
   vim schemas/v1.0.1/core.json
   ```

3. **Update CHANGELOG**
   ```bash
   vim CHANGELOG.md
   ```

4. **Update version if breaking**
   ```bash
   # If MAJOR or MINOR version bump needed
   ```

### For New Examples

1. **Create example directory**
   ```bash
   mkdir -p examples/my-use-case
   ```

2. **Add BLS files**
   ```bash
   # Create BLS files following specification
   ```

3. **Add README**
   ```bash
   # Explain the example
   vim examples/my-use-case/README.md
   ```

4. **Validate examples**
   ```bash
   # Ensure BLS files are valid
   bls validate examples/my-use-case/**/*.bls.yaml
   ```

---

## Submission Guidelines

### Before Submitting

- [ ] Read the relevant documentation
- [ ] Check existing issues/PRs
- [ ] Test your changes
- [ ] Update documentation
- [ ] Follow style guides
- [ ] Write clear commit messages

### Pull Request Process

1. **Update your fork**
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

2. **Push to your fork**
   ```bash
   git push origin feature/my-contribution
   ```

3. **Create Pull Request**
   - Go to GitHub
   - Click "New Pull Request"
   - Fill out the PR template
   - Link related issues

4. **PR Review Process**
   - Maintainers will review
   - Address feedback
   - Update as needed
   - Once approved, we'll merge

### PR Title Format

```
<type>(<scope>): <description>

Examples:
docs: add workflow tutorial
feat(schema): add healthcare extension
fix(spec): correct decision policy example
chore: update dependencies
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation
- `style`: Formatting
- `refactor`: Code restructuring
- `test`: Tests
- `chore`: Maintenance

---

## Style Guides

### BLS File Style

```yaml
# Use 2-space indentation
bls_version: "1.0.1"

specification:
  # ID: lowercase, kebab-case
  id: "my-logic-name"
  
  # Name: Title Case
  name: "My Logic Name"
  
  # Description: Clear, concise
  description: "Does X by doing Y"
  
  # Logic: Well-commented
  logic:
    # Explain complex rules
    rules:
      - id: "rule-name"  # kebab-case
        # ...
```

### Documentation Style

```markdown
# Use ATX headers (# ## ###)

## Keep lines under 100 characters

Use **bold** for emphasis, not *italic* or _underline_.

Use `code` for:
- File names
- Commands
- Code snippets

Use fenced code blocks:
\`\`\`yaml
code: here
\`\`\`

Use tables for comparisons.
```

### Commit Message Style

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Example:**
```
feat(schema): add validation rules logic type

Add support for validation rules as a new logic type.
This enables data validation specifications in BLS.

Closes #123
```

### YAML Formatting

- 2-space indentation
- No tabs
- Trailing newline
- No trailing whitespace
- Quotes for strings with special characters
- Consistent key ordering

### JSON Schema Formatting

- 2-space indentation
- Required fields listed
- Descriptions for all fields
- Examples where helpful

---

## Review Process

### What We Look For

**Quality:**
- ✅ Clear and concise
- ✅ Well-documented
- ✅ Follows specification
- ✅ No breaking changes (unless justified)

**Completeness:**
- ✅ Tests (if applicable)
- ✅ Documentation updated
- ✅ Examples provided
- ✅ CHANGELOG updated

**Compatibility:**
- ✅ Backward compatible (for minor versions)
- ✅ Follows semantic versioning
- ✅ Works across use cases

### Review Timeline

- **Initial review:** Within 3 days
- **Follow-up:** Within 2 days
- **Merge:** After approval + CI pass

---

## Community

### Communication Channels

- **GitHub Discussions:** General questions, ideas
- **GitHub Issues:** Bug reports, feature requests
- **Discord:** Real-time chat (coming soon)
- **Twitter:** [@BLSProtocol](https://twitter.com/BLSProtocol) (coming soon)

### Getting Help

**Questions about:**
- **Using BLS:** GitHub Discussions
- **Contributing:** This document or GitHub Discussions
- **Bugs:** GitHub Issues
- **Features:** GitHub Issues

### Recognition

Contributors are recognized in:
- README.md (top contributors)
- CHANGELOG.md (per version)
- Release notes

---

## License

By contributing, you agree that your contributions will be licensed under the Apache License 2.0.

---

## Questions?

- Open a [Discussion](https://github.com/bls-protocol/bls-protocol/discussions)
- Email: contribute@bls-protocol.org
- Twitter: [@BLSProtocol](https://twitter.com/BLSProtocol)

---

**Thank you for contributing to BLS Protocol!** 🎉

Your contributions help make business logic more accessible, maintainable, and AI-friendly.
