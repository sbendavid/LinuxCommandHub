# Contributing to Linux Command Mastery

First off, thank you for considering contributing to Linux Command Mastery! It's people like you that make this guide a great learning resource for the Linux community.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Getting Started](#getting-started)
- [Contribution Guidelines](#contribution-guidelines)
- [Style Guide](#style-guide)
- [Commit Message Guidelines](#commit-message-guidelines)
- [Pull Request Process](#pull-request-process)

---

## Code of Conduct

This project and everyone participating in it is governed by our commitment to providing a welcoming and inspiring community for all. By participating, you are expected to uphold this code:

- **Be respectful** - Different viewpoints and experiences are valuable
- **Be patient** - Not everyone has the same level of expertise
- **Be constructive** - Provide helpful feedback
- **Be collaborative** - Work together to improve the project
- **Focus on what's best** - For the community and learners

---

## How Can I Contribute?

### 🐛 Reporting Bugs

Found an error in a command or explanation? Report it!

- Use the bug report template
- Check if the issue already exists
- Include detailed information:
  - Which part/file has the issue
  - What's wrong or unclear
  - Expected vs actual behavior
  - Your Linux distribution and version

### 💡 Suggesting Enhancements

Have ideas to improve the guide?

- Use the feature request template
- Explain the enhancement clearly
- Describe why it would be useful
- Provide examples if possible

### 📝 Improving Documentation

Documentation can always be better!

- Fix typos and grammatical errors
- Clarify confusing explanations
- Add more examples
- Improve formatting
- Add diagrams or visualizations

### 🆕 Adding New Content

Want to add new commands, exercises, or sections?

- Ensure it fits the guide's scope
- Follow the existing structure
- Include practical examples
- Add exercises where appropriate
- Update the table of contents

### 🌍 Translations

Help make this guide accessible to non-English speakers!

- Check if translation already exists
- Follow the translation guide
- Maintain consistency with original
- Keep technical terms accurate

---

## Getting Started

### Prerequisites

- GitHub account
- Git installed locally
- Text editor (VS Code, Sublime, etc.)
- Basic understanding of Markdown
- Basic git knowledge

### Fork and Clone

1. **Fork the repository**

   - Click the "Fork" button in the top right

2. **Clone your fork**

   ```bash
   git clone https://github.com/YOUR-USERNAME/LinuxCommandHub.git
   cd linux-command-mastery
   ```

3. **Add upstream remote**

   ```bash
   git remote add upstream https://github.com/sbendavid/LinuxCommandHub.git
   ```

4. **Create a branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

---

## Contribution Guidelines

### What We're Looking For

✅ **High-quality contributions:**

- Accurate and tested commands
- Clear, beginner-friendly explanations
- Practical, real-world examples
- Well-formatted Markdown
- Proper grammar and spelling

✅ **Content additions:**

- New commands with complete explanations
- Additional practice exercises
- Troubleshooting scenarios
- Real-world use cases
- Tips and best practices

✅ **Improvements:**

- Better examples
- Clearer explanations
- Additional context
- More comprehensive coverage
- Better organization

### What We're NOT Looking For

❌ **Please avoid:**

- Promotional content or links
- Overly complex explanations for beginners
- Incomplete or untested commands
- Duplicate content
- Off-topic content
- Large structural changes without discussion

---

## Style Guide

### Markdown Formatting

**Headers:**

```markdown
# Part Title (H1)

## Section Title (H2)

### Subsection (H3)
```

**Code blocks:**

````markdown
```bash
command example
```
````

````

**Inline code:**
```markdown
Use `command` for inline code
````

**Commands with output:**

````markdown
```bash
$ command
Expected output here
```
````

````

**Emphasis:**
```markdown
**Bold for important terms**
*Italics for emphasis*
````

---

### Content Structure

**Command documentation template:**

````markdown
### command_name - Brief Description

**What it does:** Clear explanation in one sentence.

**Syntax:**

```bash
command [options] [arguments]
```
````

**Basic Usage:**

```bash
command example
```

**Example:**

```bash
$ command argument
output
```

**Options:**

- `-a` - Description
- `-l` - Description

**Practical Examples:**

**Example 1: Use case**

```bash
command with real scenario
```

**Why it's useful:** Explanation of practical value

````

---

### Writing Style

**Voice and Tone:**
- Use friendly, conversational tone
- Write in second person ("you")
- Be encouraging and supportive
- Avoid jargon when possible
- Explain technical terms when used

**Examples:**

✅ **Good:**
```markdown
The `ls` command shows you the files in a directory. Think of it as
looking inside a folder to see what's there.
````

❌ **Avoid:**

```markdown
ls displays directory contents utilizing filesystem metadata queries.
```

**Formatting preferences:**

- Use consistent terminology
- Write in present tense
- Use active voice
- Keep sentences concise
- Break up long paragraphs

---

### Command Examples

**Always include:**

1. The actual command
2. Expected output
3. Explanation of what happened
4. Real-world context when helpful

**Example format:**

````markdown
**List files in long format:**

```bash
$ ls -l /home/user
total 48
drwxr-xr-x 2 user user 4096 Oct 24 10:00 Documents
-rw-r--r-- 1 user user 1234 Oct 24 10:00 file.txt
```
````

This shows detailed information including permissions, owner, size, and modification time.

```

---

## Commit Message Guidelines

### Format

```

type(scope): brief description

Detailed explanation of what and why (optional)

Fixes #issue_number (if applicable)

```

### Types

- **feat:** New feature or command
- **fix:** Bug fix or correction
- **docs:** Documentation improvements
- **style:** Formatting, typos (no code change)
- **refactor:** Restructuring content
- **test:** Adding exercises or examples
- **chore:** Maintenance tasks

### Examples

```

feat(part-02): add rsync command examples

Added comprehensive rsync examples including:

- Basic sync operations
- Remote synchronization
- Common options and flags

Closes #45

```

```

fix(part-06): correct chmod permission calculation

Fixed incorrect octal permission calculation in the
permissions section. Changed 664 to 644 for standard
file permissions.

Fixes #78

```

```

docs(README): update table of contents

Added links to new troubleshooting section

````

---

## Pull Request Process

### Before Submitting

- [ ] Test all commands if you added/modified any
- [ ] Check spelling and grammar
- [ ] Ensure Markdown renders correctly
- [ ] Update table of contents if needed
- [ ] Follow the style guide
- [ ] Write clear commit messages

### Submitting a PR

1. **Push your changes**
   ```bash
   git push origin feature/your-feature-name
````

2. **Create Pull Request**

   - Go to your fork on GitHub
   - Click "New Pull Request"
   - Fill out the PR template completely
   - Link related issues

3. **PR Description should include:**
   - What changes were made
   - Why the changes are needed
   - How to test (if applicable)
   - Screenshots (if visual changes)
   - Related issues

### Review Process

- Maintainers will review within 7 days
- Address feedback and comments
- Make requested changes
- Keep discussion professional and friendly
- Be patient - reviews take time

### After Approval

- Maintainer will merge your PR
- Delete your feature branch
- Update your fork:
  ```bash
  git checkout main
  git pull upstream main
  git push origin main
  ```

---

## Testing Your Contributions

### For Command Examples

```bash
# Test in a safe environment (VM or container)
docker run -it ubuntu:latest bash

# Test the command
your-command-here

# Verify output matches documentation
```

### For Code Blocks

- Ensure syntax highlighting works
- Verify commands are copy-pasteable
- Check for typos in commands

### For Links

- Verify all internal links work
- Check external links are valid
- Ensure relative paths are correct

---

## Recognition

Contributors will be:

- Listed in the Contributors section
- Credited in release notes
- Acknowledged in the README

Thank you for contributing! 🎉

---

## Questions?

- 💬 Open a discussion on GitHub
- 📧 Email: [maintainer-email]
- 🐛 Create an issue for bugs
- 💡 Create an issue for feature requests

---

## Resources

- [GitHub Flow Guide](https://guides.github.com/introduction/flow/)
- [Markdown Guide](https://www.markdownguide.org/)
- [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/)
- [Open Source Guide](https://opensource.guide/how-to-contribute/)

---

**Happy Contributing! 🐧✨**
