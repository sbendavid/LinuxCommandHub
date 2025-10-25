# Part 4: Input/Output and Text Processing

[← Back to Part 3](./03-help-shortcuts.md) | [Main Guide](../README.md) | [Next: Text Editors →](./05-text-editors.md)

---

## Table of Contents

- [Standard Streams](#standard-streams)
- [Pipes and tee](#pipes-and-tee)
- [Environment Variables](#environment-variables)
- [Text Processing Tools](#text-processing-tools)
- [grep - Pattern Searching](#grep---pattern-searching)
- [Regular Expressions](#regular-expressions)

---

## Standard Streams

Linux uses three standard data streams:

### 1. stdout (Standard Output)

**File descriptor: 1**

Normal output from commands, displayed in your terminal.

**Redirect to file (overwrite):**

```bash
echo "Hello World" > output.txt
```

**Redirect to file (append):**

```bash
echo "More text" >> output.txt
```

**Examples:**

```bash
# Save directory listing to file
ls -la > files.txt

# Append date to log
date >> log.txt

# Save command output
ps aux > processes.txt
```

---

### 2. stdin (Standard Input)

**File descriptor: 0**

Input to commands, usually from keyboard.

**Redirect input from file:**

```bash
cat < input.txt
```

**Combine input and output:**

```bash
cat < input.txt > output.txt
```

**Here document:**

```bash
cat > file.txt << EOF
Line 1
Line 2
Line 3
EOF
```

---

### 3. stderr (Standard Error)

**File descriptor: 2**

Error messages, separate from normal output.

**Examples:**

```bash
# This produces an error
ls /nonexistent
ls: cannot access '/nonexistent': No such file or directory
```

**Redirect stderr to file:**

```bash
ls /nonexistent 2> errors.txt
```

**Redirect both stdout and stderr:**

```bash
ls /nonexistent > output.txt 2>&1
```

Or use shorthand:

```bash
ls /nonexistent &> output.txt
```

**Discard errors (send to /dev/null):**

```bash
ls /nonexistent 2> /dev/null
```

**Separate stdout and stderr:**

```bash
command > output.txt 2> errors.txt
```

---

### Practical Examples

**Example 1: Save successful output, see errors:**

```bash
find / -name "*.conf" > found.txt 2> /dev/null
```

**Example 2: Log everything:**

```bash
./script.sh &> complete_log.txt
```

**Example 3: Append to log with timestamp:**

```bash
echo "$(date): Backup started" >> backup.log
```

---

## Pipes and tee

### Pipe (|)

**Sends output of one command as input to another.**

**Syntax:**

```bash
command1 | command2
```

**Examples:**

**Count files in directory:**

```bash
ls | wc -l
```

**Find specific processes:**

```bash
ps aux | grep firefox
```

**Sort and view:**

```bash
cat names.txt | sort | less
```

**Chain multiple pipes:**

```bash
cat /var/log/syslog | grep error | wc -l
```

---

### tee Command

**Writes output to both file AND screen.**

**Syntax:**

```bash
command | tee file.txt
```

**Examples:**

**View and save:**

```bash
ls -la | tee listing.txt
```

**Append mode:**

```bash
echo "New line" | tee -a file.txt
```

**Multiple files:**

```bash
echo "Data" | tee file1.txt file2.txt file3.txt
```

**Save intermediate results:**

```bash
cat data.txt | sort | tee sorted.txt | uniq > unique.txt
```

---

## Environment Variables

**What they are:** Named values affecting how processes run.

**View all variables:**

```bash
env
```

**View specific variable:**

```bash
echo $HOME
echo $PATH
echo $USER
```

**Set a variable:**

```bash
export MY_VAR="Hello"
echo $MY_VAR
```

### Important Variables

**PATH:** Where the system looks for commands

```bash
echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
```

**HOME:** Your home directory

```bash
echo $HOME
/home/pete
```

**USER:** Your username

```bash
echo $USER
pete
```

**SHELL:** Your current shell

```bash
echo $SHELL
/bin/bash
```

**Make permanent (add to ~/.bashrc):**

```bash
export MY_VAR="value"
```

---

## Text Processing Tools

### cut - Extract Sections

**Extract specific characters:**

```bash
$ echo "Hello World" | cut -c 1-5
Hello
```

**Extract fields (default: tab-separated):**

```bash
$ cat file.txt
name	age	city
$ cut -f 2 file.txt
age
```

**Custom delimiter:**

```bash
$ echo "one;two;three" | cut -d ";" -f 2
two
```

---

### paste - Merge Lines

**Combine files side-by-side:**

```bash
$ cat file1.txt
A
B
C
$ cat file2.txt
1
2
3
$ paste file1.txt file2.txt
A	1
B	2
C	3
```

**Serial merge:**

```bash
$ paste -s file.txt
A	B	C
```

---

### head - First Lines

**First 10 lines (default):**

```bash
head file.txt
```

**Specific number:**

```bash
head -n 5 file.txt
```

**First N bytes:**

```bash
head -c 100 file.txt
```

---

### tail - Last Lines

**Last 10 lines (default):**

```bash
tail file.txt
```

**Specific number:**

```bash
tail -n 20 file.txt
```

**Follow file (watch for updates):**

```bash
tail -f /var/log/syslog
```

**Start from line N:**

```bash
tail -n +50 file.txt
```

---

### sort - Sort Lines

**Alphabetically:**

```bash
sort names.txt
```

**Reverse order:**

```bash
sort -r names.txt
```

**Numerical sort:**

```bash
sort -n numbers.txt
```

**Sort by field:**

```bash
sort -t "," -k 2 file.csv
```

---

### uniq - Remove Duplicates

**Remove adjacent duplicates:**

```bash
uniq file.txt
```

**Count occurrences:**

```bash
uniq -c file.txt
```

**Show only unique:**

```bash
uniq -u file.txt
```

**Show only duplicates:**

```bash
uniq -d file.txt
```

**Important:** Always sort first!

```bash
sort file.txt | uniq
```

---

### tr - Translate Characters

**Lowercase to uppercase:**

```bash
echo "hello" | tr a-z A-Z
# HELLO
```

**Delete characters:**

```bash
echo "hello123" | tr -d 0-9
# hello
```

**Squeeze repeats:**

```bash
echo "hello    world" | tr -s ' '
# hello world
```

---

### wc - Word Count

**Count everything:**

```bash
$ wc file.txt
  10  50  300 file.txt
# lines words bytes
```

**Count lines only:**

```bash
wc -l file.txt
```

**Count words:**

```bash
wc -w file.txt
```

**Count characters:**

```bash
wc -c file.txt
```

---

### nl - Number Lines

```bash
$ nl file.txt
     1	First line
     2	Second line
     3	Third line
```

---

## grep - Pattern Searching

**What it does:** Searches for patterns in text.

**Basic syntax:**

```bash
grep pattern file
```

### Basic Usage

**Search in file:**

```bash
grep "error" logfile.txt
```

**Case-insensitive:**

```bash
grep -i "error" logfile.txt
```

**Show line numbers:**

```bash
grep -n "error" logfile.txt
```

**Invert match (show non-matching):**

```bash
grep -v "error" logfile.txt
```

**Count matches:**

```bash
grep -c "error" logfile.txt
```

---

### Multiple Files

**Search multiple files:**

```bash
grep "TODO" *.txt
```

**Recursive search:**

```bash
grep -r "error" /var/log/
```

**Show only filenames:**

```bash
grep -l "error" *.log
```

---

### Advanced Options

**Whole word only:**

```bash
grep -w "cat" file.txt
# Matches "cat" but not "catch"
```

**Extended regex:**

```bash
grep -E "error|warning" file.txt
```

**Show context:**

```bash
grep -A 2 "error" file.txt  # 2 lines after
grep -B 2 "error" file.txt  # 2 lines before
grep -C 2 "error" file.txt  # 2 lines before and after
```

---

### Practical Examples

**Find errors in logs:**

```bash
grep -i "error" /var/log/syslog | tail -20
```

**Search for IP addresses:**

```bash
grep -E "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" access.log
```

**Find files containing text:**

```bash
grep -rl "database" /etc/
```

**Exclude binary files:**

```bash
grep -I "pattern" *
```

---

## Regular Expressions

**What they are:** Patterns for matching text.

### Test String

```
sally sells seashells
by the seashore
```

### Basic Patterns

**1. Start of line (^):**

```bash
grep "^by" file.txt
# Matches: "by the seashore"
```

**2. End of line ($):**

```bash
grep "seashore$" file.txt
# Matches: "by the seashore"
```

**3. Any character (.):**

```bash
grep "s.a" file.txt
# Matches: sea, s+a, s a, etc.
```

**4. Character sets ([]):**

```bash
grep "s[aei]" file.txt
# Matches: sa, se, si
```

**5. Negated set ([^]):**

```bash
grep "s[^e]" file.txt
# Matches: sa, si, so, but not se
```

**6. Ranges:**

```bash
grep "[a-z]" file.txt  # Lowercase letters
grep "[A-Z]" file.txt  # Uppercase letters
grep "[0-9]" file.txt  # Digits
```

---

### Quantifiers

**Zero or more (\*):**

```bash
grep "go*d" file.txt
# Matches: gd, god, good, goood
```

**One or more (+):**

```bash
grep -E "go+d" file.txt
# Matches: god, good, goood (but not gd)
```

**Zero or one (?):**

```bash
grep -E "colou?r" file.txt
# Matches: color, colour
```

**Exact count ({n}):**

```bash
grep -E "[0-9]{3}" file.txt
# Matches: exactly 3 digits
```

**Range ({n,m}):**

```bash
grep -E "[0-9]{2,4}" file.txt
# Matches: 2 to 4 digits
```

---

### Practical Regex Examples

**Email addresses:**

```bash
grep -E "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" file.txt
```

**Phone numbers:**

```bash
grep -E "[0-9]{3}-[0-9]{3}-[0-9]{4}" file.txt
```

**IP addresses:**

```bash
grep -E "([0-9]{1,3}\.){3}[0-9]{1,3}" file.txt
```

**URLs:**

```bash
grep -E "https?://[a-zA-Z0-9./?=_-]+" file.txt
```

---

## Combining Tools

**Example 1: Find top 10 most common words:**

```bash
cat file.txt | tr -s ' ' '\n' | sort | uniq -c | sort -rn | head -10
```

**Example 2: Count unique IPs in log:**

```bash
grep -oE "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" access.log | sort | uniq | wc -l
```

**Example 3: Extract emails and save:**

```bash
grep -oE "[a-zA-Z0-9._+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" file.txt | sort | uniq > emails.txt
```

---

## Summary

✅ **Standard streams** - stdin, stdout, stderr  
✅ **Redirection** - >, >>, <, 2>  
✅ **Pipes** - | for chaining commands  
✅ **tee** - Output to file and screen  
✅ **Text tools** - cut, paste, sort, uniq, tr, wc  
✅ **grep** - Pattern searching  
✅ **Regex** - Powerful pattern matching

### Quick Reference

```bash
# Redirection
command > file              # Overwrite
command >> file             # Append
command 2> file             # Errors only
command &> file             # Both stdout and stderr

# Pipes
cmd1 | cmd2                 # Chain commands
cmd | tee file              # Save and display

# Text processing
cut -d "," -f 2 file        # Extract field
sort file                   # Sort lines
uniq file                   # Remove duplicates
grep "pattern" file         # Search
wc -l file                  # Count lines

# Combining
sort file | uniq | wc -l    # Count unique lines
```

---

## Practice Exercises

Try these to master text processing!

**Exercise 1: Log Analysis**

```bash
# Using /var/log/syslog:
# 1. Count total lines
# 2. Find all error messages
# 3. Count unique errors
# 4. Find errors from last hour
```

**Exercise 2: Text Manipulation**

```bash
# 1. Create a file with duplicate lines
# 2. Sort and remove duplicates
# 3. Convert to uppercase
# 4. Count words per line
```

**Exercise 3: Data Extraction**

```bash
# 1. Extract all email addresses from a file
# 2. Sort them alphabetically
# 3. Count how many unique domains
# 4. Save results to a file
```

---

## Next Steps

Excellent! You now know how to process and manipulate text like a pro.

**Continue to:** [Part 5: Text Editors](./05-text-editors.md) to learn Vim and Emacs.

---

[← Back to Part 3](./03-help-shortcuts.md) | [Main Guide](../README.md) | [Next: Text Editors →](./05-text-editors.md)
