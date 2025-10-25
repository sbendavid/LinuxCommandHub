# Part 3: Getting Help and Shortcuts

[← Back to Part 2](./02-file-operations.md) | [Main Guide](../README.md) | [Next: I/O & Text Processing →](./04-io-text-processing.md)

---

## Table of Contents

- [help Command](#help-command)
- [man - Manual Pages](#man---manual-pages)
- [whatis Command](#whatis-command)
- [alias - Creating Shortcuts](#alias---creating-shortcuts)
- [exit and logout](#exit-and-logout)

---

## help Command

**What it does:** Provides quick help for built-in shell commands.

**Syntax:**

```bash
help command_name
```

### Examples

```bash
$ help echo
echo: echo [-neE] [arg ...]
    Write arguments to the standard output.
```

**Alternative format:**

```bash
$ echo --help
Usage: echo [SHORT-OPTION]... [STRING]...
  or:  echo LONG-OPTION
Echo the STRING(s) to standard output.
```

### When to Use

- Quick syntax reminder
- Check available options
- Built-in shell commands (cd, echo, etc.)

---

## man - Manual Pages

**What it does:** Opens comprehensive documentation for commands.

**Syntax:**

```bash
man command_name
```

### Basic Usage

```bash
man ls
```

This opens the complete manual for the `ls` command.

### Navigation

Inside a man page:

- **Arrow keys** or **j/k** - Scroll up/down
- **Space** or **Page Down** - Next page
- **b** or **Page Up** - Previous page
- **g** - Go to beginning
- **G** - Go to end
- **/pattern** - Search for text
- **n** - Next search result
- **N** - Previous search result
- **q** - Quit

### Man Page Sections

Man pages are organized into sections:

1. **User commands** - Regular commands
2. **System calls** - Kernel functions
3. **Library functions** - C library functions
4. **Special files** - Devices
5. **File formats** - Configuration files
6. **Games** - Games and demos
7. **Miscellaneous** - Conventions, standards
8. **System administration** - Root commands

**View specific section:**

```bash
man 5 passwd   # File format of /etc/passwd
man 1 passwd   # The passwd command
```

### Anatomy of a Man Page

```
NAME
    Command name and brief description

SYNOPSIS
    Command syntax and options

DESCRIPTION
    Detailed explanation

OPTIONS
    All available options

EXAMPLES
    Usage examples

SEE ALSO
    Related commands

AUTHOR
    Who wrote it

COPYRIGHT
    Licensing information
```

### Pro Tips

**Search all man pages:**

```bash
man -k keyword
```

Example:

```bash
$ man -k "copy files"
cp (1)          - copy files and directories
install (1)     - copy files and set attributes
```

**Read man pages in browser:**

```bash
man -H ls
```

**Print man page:**

```bash
man ls | col -b > ls_manual.txt
```

---

## whatis Command

**What it does:** Provides a one-line description of a command.

**Syntax:**

```bash
whatis command_name
```

### Examples

```bash
$ whatis cat
cat (1) - concatenate files and print on standard output

$ whatis ls
ls (1) - list directory contents

$ whatis grep
grep (1) - print lines that match patterns
```

### Multiple Commands

```bash
$ whatis cat ls grep
cat (1)  - concatenate files and print on standard output
ls (1)   - list directory contents
grep (1) - print lines that match patterns
```

### When to Use

- Quick reminder of what a command does
- Checking if a command exists
- When you don't need full documentation

### Update whatis Database

If `whatis` doesn't work:

```bash
sudo mandb
```

This rebuilds the manual page database.

---

## alias - Creating Shortcuts

**What it does:** Creates custom shortcuts for commands you use frequently.

**Syntax:**

```bash
alias name='command'
```

### Basic Usage

**Create an alias:**

```bash
alias ll='ls -la'
```

Now typing `ll` executes `ls -la`!

**View all aliases:**

```bash
alias
```

**Remove an alias:**

```bash
unalias ll
```

---

### Useful Aliases

**Navigation shortcuts:**

```bash
alias ..='cd ..'
alias ...='cd ../..'
alias home='cd ~'
```

**Safety aliases:**

```bash
alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'
```

**Productivity aliases:**

```bash
alias ll='ls -lah'
alias la='ls -A'
alias l='ls -CF'
```

**System management:**

```bash
alias update='sudo apt update && sudo apt upgrade'
alias install='sudo apt install'
alias ports='netstat -tulanp'
```

**Git shortcuts (if you use Git):**

```bash
alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gp='git push'
```

**Directory shortcuts:**

```bash
alias proj='cd ~/projects'
alias docs='cd ~/Documents'
alias dl='cd ~/Downloads'
```

---

### Making Aliases Permanent

Aliases created in the terminal only last for that session. To make them permanent:

**1. Edit your bash configuration:**

```bash
nano ~/.bashrc
```

**2. Add your aliases:**

```bash
# My custom aliases
alias ll='ls -lah'
alias update='sudo apt update && sudo apt upgrade'
alias ..='cd ..'
```

**3. Save and reload:**

```bash
source ~/.bashrc
```

Or close and reopen your terminal.

---

### Advanced Alias Examples

**Alias with parameters (use functions):**

```bash
# Add to ~/.bashrc
mkcd() {
    mkdir -p "$1" && cd "$1"
}
```

Usage: `mkcd new_folder` creates and enters the folder.

**Alias with pipes:**

```bash
alias psg='ps aux | grep -v grep | grep -i -e VSZ -e'
```

**Colorize output:**

```bash
alias ls='ls --color=auto'
alias grep='grep --color=auto'
```

---

### Viewing and Managing Aliases

**List all aliases:**

```bash
alias
```

**Check specific alias:**

```bash
alias ll
```

**Temporarily bypass an alias:**

```bash
\ls   # Runs ls without any alias
```

**Remove all aliases (current session):**

```bash
unalias -a
```

---

### Real-World Alias Examples

**Web developer:**

```bash
alias serve='python -m http.server 8000'
alias start-nginx='sudo systemctl start nginx'
alias stop-nginx='sudo systemctl stop nginx'
```

**System administrator:**

```bash
alias logs='tail -f /var/log/syslog'
alias ports='netstat -tulanp'
alias meminfo='free -m -l -t'
alias df='df -h'
```

**Daily tasks:**

```bash
alias c='clear'
alias h='history'
alias now='date +"%T"'
alias today='date +"%d-%m-%Y"'
```

---

## exit and logout

**What they do:** End your terminal session.

### exit

```bash
exit
```

- Closes the current shell
- Works in any shell session
- Can specify exit code: `exit 1`

### logout

```bash
logout
```

- Closes a login shell
- Doesn't work in subshells
- Used when you've logged in remotely

### Exit Codes

```bash
$ ls /nonexistent
ls: cannot access '/nonexistent': No such file or directory
$ echo $?
2
```

- `0` - Success
- `1-255` - Various errors
- `$?` stores the last exit code

### Keyboard Shortcuts

**Ctrl+D** - Send EOF (End of File), also exits shell

**Ctrl+C** - Cancel current command

**Ctrl+Z** - Suspend current command

---

## Summary

In this part, you learned:

✅ **help** - Quick help for shell built-ins  
✅ **man** - Comprehensive manual pages  
✅ **whatis** - One-line command descriptions  
✅ **alias** - Create custom shortcuts  
✅ **exit/logout** - End your session

### Quick Reference

```bash
# Getting help
help cd                    # Help for built-in commands
man ls                     # Full manual for ls
whatis cat                 # One-line description
man -k "search term"       # Search man pages

# Creating aliases
alias ll='ls -la'          # Create alias
alias                      # View all aliases
unalias ll                 # Remove alias

# Making aliases permanent
nano ~/.bashrc             # Edit config file
source ~/.bashrc           # Reload config

# Exiting
exit                       # Close shell
logout                     # Logout of login shell
Ctrl+D                     # EOF (also exits)
```

---

## Practice Exercises

**Exercise 1: Exploring Help**

```bash
# 1. Get help for the 'cd' command
# 2. Read the man page for 'grep'
# 3. Use whatis to check what 'chmod' does
# 4. Search man pages for commands related to "network"
```

**Exercise 2: Creating Aliases**

```bash
# 1. Create an alias 'll' for 'ls -la'
# 2. Create an alias to go up two directories
# 3. Create a safe rm alias
# 4. Test your aliases
# 5. Remove one alias
```

**Exercise 3: Permanent Aliases**

```bash
# 1. Open ~/.bashrc in a text editor
# 2. Add 3 useful aliases
# 3. Save and reload the configuration
# 4. Test your new aliases
```

---

## Tips for Learning

**1. Read man pages regularly:** Pick a command and read its man page fully.

**2. Start with simple aliases:** Don't overcomplicate. Start with basic shortcuts.

**3. Document your aliases:** Add comments in ~/.bashrc

```bash
# Shortcut for detailed listing
alias ll='ls -lah'
```

**4. Share aliases:** Learn from others' dotfiles on GitHub.

**5. Use whatis for discovery:** Run `whatis *` to see descriptions of commands in your PATH.

---

## Next Steps

You now know how to get help and create shortcuts!

**Ready to continue?** Head to [Part 4: I/O & Text Processing](./04-io-text-processing.md) to learn about redirecting output and manipulating text.

---

[← Back to Part 2](./02-file-operations.md) | [Main Guide](../README.md) | [Next: I/O & Text Processing →](./04-io-text-processing.md)
