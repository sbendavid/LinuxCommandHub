# Security Policy

## Our Commitment

The security and safety of LinuxCommandHub users is our top priority. While this project primarily contains educational documentation, we take security seriously.

---

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 1.0.x   | :white_check_mark: |
| < 1.0   | :x:                |

We support the latest major version with security updates. Once we release v2.0, v1.x will receive security updates for 6 months.

---

## What Constitutes a Security Issue

### In Scope

**Documentation Security Issues:**

- Commands that could cause data loss without adequate warnings
- Dangerous command examples without safety notices
- Scripts with security vulnerabilities
- Misleading security advice
- Instructions that could compromise system security

**Project Security Issues:**

- Malicious code in repository
- Compromised dependencies (if any are added)
- XSS vulnerabilities in generated websites
- Authentication/authorization issues (if interactive features added)

**Examples:**

- A script example with command injection vulnerability
- Missing warning on `rm -rf` usage
- Incorrect permissions advice that weakens security
- Malicious links or redirects

### Out of Scope

- General Linux security questions (use Discussions)
- Theoretical security discussions
- Feature requests for security content
- Typos or grammatical errors (use regular Issues)

---

## Reporting a Vulnerability

### How to Report

**For security vulnerabilities, please do NOT create a public GitHub issue.**

Instead:

1. **Email:** Send details to [INSERT SECURITY EMAIL]

   - Subject: "SECURITY: Brief description"
   - Include detailed description
   - Steps to reproduce (if applicable)
   - Potential impact
   - Suggested fix (if you have one)

2. **GitHub Security Advisory:**
   - Go to Security tab → Advisories → New draft security advisory
   - Fill out the form privately

### What to Include

Please provide:

- **Description** - What's the issue?
- **Location** - Which file/section?
- **Impact** - What could happen?
- **Reproduction** - How to reproduce it?
- **Suggestion** - How to fix it? (optional)
- **Your Details** - Name/handle for credit (optional)

### Example Report

```
Subject: SECURITY: Dangerous rm command without warning

Description:
Part 2, section "Removing Files", shows 'rm -rf /' as an
example without adequate warning about its danger.

Location:
File: docs/02-file-operations.md
Line: 234

Impact:
Beginners might accidentally execute this command, causing
complete data loss.

Suggestion:
Add prominent warning box and remove the example or show it
with safeguards.
```

---

## Response Timeline

| Stage              | Timeline    | Description                |
| ------------------ | ----------- | -------------------------- |
| Acknowledgment     | 24-48 hours | We confirm receipt         |
| Initial Assessment | 3-5 days    | We evaluate severity       |
| Investigation      | 5-10 days   | We verify and analyze      |
| Fix Development    | Varies      | Based on complexity        |
| Disclosure         | Coordinated | We work with you on timing |

### Severity Levels

**Critical** (24-48 hours)

- Commands that could destroy data
- Malicious code in repository
- Active exploitation possible

**High** (3-5 days)

- Security misadvice affecting many users
- Dangerous examples without warnings
- Significant security risks

**Medium** (1-2 weeks)

- Minor security misadvice
- Missing safety warnings
- Needs clarification

**Low** (As able)

- Theoretical concerns
- Suggestions for improvement
- Minor issues

---

## Security Best Practices in This Project

### For Content

**1. Dangerous Commands**
All potentially destructive commands must have:

- ⚠️ Clear warning boxes
- Explanation of what could go wrong
- Safe alternatives when available
- Testing recommendations

Example:

```markdown
⚠️ **WARNING:** This command permanently deletes files!
```

**2. Root/Sudo Usage**

- Always explain why `sudo` is needed
- Warn about running untested commands as root
- Encourage understanding before executing

**3. Security-Related Content**

- Keep security advice current
- Link to authoritative sources
- Note distribution-specific differences
- Avoid security-by-obscurity

**4. Example Scripts**

- Include input validation
- Handle errors properly
- Follow security best practices
- Comment security-relevant code

### For Contributors

**When contributing:**

- Review security implications of commands
- Add warnings for dangerous operations
- Test commands in safe environments
- Consider beginner misuse scenarios

**Don't include:**

- Actual exploits or malware
- Commands to harm others' systems
- Real credentials or keys
- Phishing or social engineering examples

---

## User Safety Guidelines

### For Learners Using This Guide

**Before running ANY command:**

1. **Understand it first**

   - Read the explanation
   - Check the man page
   - Understand each flag/option

2. **Test safely**

   - Use a VM or container
   - Create backups first
   - Start with non-destructive commands
   - Use `-n` or `--dry-run` when available

3. **Be especially careful with:**

   - Commands containing `rm -rf`
   - Anything running as root/sudo
   - Commands modifying system files
   - Scripts from unknown sources
   - Commands with wildcards (\*)

4. **Never blindly copy-paste:**
   - Understand each part
   - Verify the source
   - Check for malicious additions
   - Test in safe environment first

---

## Dangerous Commands to Always Be Careful With

These commands require extra caution (not exhaustive):

### File System

```bash
rm -rf /           # Deletes everything
chmod 777 /        # Makes everything world-writable
chown -R user /    # Changes ownership of everything
dd if=/dev/zero    # Can overwrite drives
```

### System

```bash
:(){ :|:& };:      # Fork bomb (crashes system)
mkfs.ext4 /dev/sda # Formats a drive
sudo anything      # Runs with full privileges
```

### Network

```bash
wget url | bash    # Runs remote script
curl url | sudo sh # Runs remote script as root
```

**Always:**

- Understand before executing
- Have backups
- Test in VMs
- Be cautious with sudo

---

## Reporting False Positives

If you believe we've marked something as dangerous unnecessarily:

1. Open a regular GitHub issue
2. Explain why it's safe
3. Provide references/documentation
4. Suggest alternative wording

---

## Security Updates

When we fix a security issue:

1. **Patch is developed** privately
2. **Fix is tested** thoroughly
3. **Security advisory** is published
4. **Fix is released** in new version
5. **Announcement** is made
6. **Credit given** to reporter (if desired)

---

## Recognition

We appreciate security researchers and responsible disclosure:

- Credit in security advisories
- Thank you in release notes
- Listed in SECURITY.md (if desired)
- Contributor recognition

---

## Questions?

- 💬 General security questions: GitHub Discussions
- 🐛 Non-security issues: GitHub Issues
- 🔒 Security concerns: [SECURITY EMAIL]

---

## Additional Resources

**Linux Security:**

- [Linux Security Documentation](https://www.kernel.org/doc/html/latest/security/)
- [OWASP Cheat Sheets](https://cheatsheetseries.owasp.org/)
- [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks/)

**Secure Coding:**

- [OWASP Secure Coding Practices](https://owasp.org/www-project-secure-coding-practices-quick-reference-guide/)
- [Bash Security Pitfalls](https://mywiki.wooledge.org/BashPitfalls)

---

## Our Promise

We commit to:

- Take all security reports seriously
- Respond promptly to vulnerabilities
- Fix issues transparently
- Keep users informed
- Learn and improve continuously

**Thank you for helping keep LinuxCommandHub safe!** 🔒

---

**Last Updated:** October 26, 2025
**Next Review:** January 2026
