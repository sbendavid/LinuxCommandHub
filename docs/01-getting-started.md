# Part 1: Getting Started with Linux

[← Back to Main Guide](../README.md) | [Next: File Operations →](./02-file-operations.md)

---

## Table of Contents

- [Understanding the Linux Terminal](#understanding-the-linux-terminal)
- [pwd - Print Working Directory](#pwd---print-working-directory)
- [cd - Change Directory](#cd---change-directory)
- [ls - List Directories](#ls---list-directories)
- [Basic Navigation Tips](#basic-navigation-tips)

---

## Understanding the Linux Terminal

Welcome to the world of Linux! The terminal (also called the command line or shell) is your gateway to powerful system control. While it might seem intimidating at first, the command line gives you precise control over your computer and is essential for system administration, development, and automation.

The terminal uses a command-line interface (CLI) where you type commands and press Enter to execute them. Each command follows a pattern:

```bash
command [options] [arguments]
```

**Example:**

```bash
ls -la /home
```

Breaking this down:

- `ls` is the command (list directory contents)
- `-la` are options (list all files in long format)
- `/home` is the argument (the directory to list)

Don't worry if this seems abstract now—everything will become clear with practice!

---

## pwd - Print Working Directory

**What it does:** Shows you where you currently are in the filesystem.

**Syntax:**

```bash
pwd
```

**Example:**

```bash
$ pwd
/home/pete
```

### Understanding the Output

The output `/home/pete` is called a **path**. Think of your filesystem as a giant tree:

- The root (`/`) is at the very top
- Everything branches from there
- You're always standing at one location in this tree

### Why It's Useful

Think of `pwd` as your GPS coordinates in the filesystem. When you're navigating through many directories, it's easy to lose track of where you are. Running `pwd` tells you exactly where you're standing.

### Real-World Scenario

Imagine you're working on a project with hundreds of directories:

```bash
$ cd /var/www/myproject/src/components/user/profile
$ pwd
/var/www/myproject/src/components/user/profile
```

Without `pwd`, you might forget you're deep in the `profile` directory!

### Practical Tip

Run `pwd` whenever you feel lost. It's your compass in the Linux filesystem—simple, reliable, and always accurate.

---

## cd - Change Directory

**What it does:** Moves you to a different directory (folder).

**Basic Syntax:**

```bash
cd [directory]
```

### Basic Usage

**Move to a specific path:**

```bash
cd /home/pete/Desktop
```

**Move to a folder in your current location:**

```bash
cd Hawaii
```

This moves you into a folder called "Hawaii" that's in your current directory.

---

### Special Directory Shortcuts

Linux provides several shortcuts that make navigation much faster:

#### 1. Current Directory (.)

The single dot `.` represents your current directory.

```bash
cd .
```

This doesn't change your location—it keeps you where you are. While this seems useless now, it becomes important for other commands later.

#### 2. Parent Directory (..)

The double dot `..` takes you one level up in the directory tree.

```bash
$ pwd
/home/pete/Documents/projects
$ cd ..
$ pwd
/home/pete/Documents
```

You can chain these together:

```bash
$ cd ../..
$ pwd
/home/pete
```

This moves you up two levels.

#### 3. Home Directory (~)

The tilde `~` is a shortcut to your home directory (usually `/home/username`).

```bash
cd ~
```

This instantly takes you home from anywhere in the filesystem.

```bash
$ pwd
/var/log/apache2/sites/configs
$ cd ~
$ pwd
/home/pete
```

#### 4. Previous Directory (-)

The dash `-` takes you to the previous directory you were in.

```bash
$ pwd
/home/pete/Documents
$ cd /var/log
$ pwd
/var/log
$ cd -
$ pwd
/home/pete/Documents
```

This is incredibly useful for jumping back and forth between two directories!

---

### Real-World Examples

**Example 1: Navigate to Desktop**

```bash
cd ~/Desktop
```

**Example 2: Go up and then down**

```bash
$ pwd
/home/pete/Documents/work/project1
$ cd ../../personal
$ pwd
/home/pete/Documents/personal
```

**Example 3: Quick return**

```bash
$ cd /etc/apache2
# Do some work...
$ cd -
# Back to where you were!
```

---

### Common Mistakes

**Mistake 1: Forgetting the space**

```bash
cd/home/pete  # Wrong - missing space
cd /home/pete  # Correct
```

**Mistake 2: Using backslashes**

```bash
cd C:\Users\Pete  # Wrong - Windows style
cd /home/pete     # Correct - Linux style
```

**Mistake 3: Not handling spaces in names**

```bash
cd My Documents    # Wrong - shell sees two arguments
cd "My Documents"  # Correct - quoted
cd My\ Documents   # Also correct - escaped space
```

---

### Pro Tips

1. **Tab completion:** Start typing a directory name and press Tab. The shell will auto-complete it!

   ```bash
   cd Doc[Tab]  # Auto-completes to "Documents"
   ```

2. **Case sensitivity:** Linux is case-sensitive. `Documents` and `documents` are different!

   ```bash
   cd documents  # Might fail
   cd Documents  # Might work
   ```

3. **Use relative paths:** When you're already close to your destination, use relative paths:
   ```bash
   cd ../sister-directory
   ```
   Instead of:
   ```bash
   cd /home/pete/parent/sister-directory
   ```

---

## ls - List Directories

**What it does:** Shows you the contents of a directory.

**Basic Syntax:**

```bash
ls [options] [directory]
```

### Basic Usage

**List current directory:**

```bash
ls
```

**Example output:**

```
Desktop  Documents  Downloads  Music  Pictures  Videos
```

**List a specific directory:**

```bash
ls /home/pete
```

---

### Common Options

#### Option: -a (all files)

Shows ALL files, including hidden files (files starting with a dot).

```bash
ls -a
```

**Example output:**

```
.  ..  .bashrc  .profile  Desktop  Documents  Downloads
```

**What are those dots?**

- `.` represents the current directory
- `..` represents the parent directory
- `.bashrc` and `.profile` are hidden configuration files

**Why hide files?** Configuration files are often hidden to reduce clutter. They're important, but you don't need to see them every time you list files.

---

#### Option: -l (long format)

Displays detailed information about each file.

```bash
ls -l
```

**Example output:**

```
drwxr-xr-x 2 pete users 4096 Oct 24 10:30 Documents
-rw-r--r-- 1 pete users 1234 Oct 23 14:20 file.txt
```

**Understanding the columns:**

```
drwxr-xr-x  2  pete  users  4096  Oct 24 10:30  Documents
    │       │   │      │      │        │          │
    │       │   │      │      │        │          └─ Name
    │       │   │      │      │        └─ Last modified
    │       │   │      │      └─ Size (bytes)
    │       │   │      └─ Group
    │       │   └─ Owner
    │       └─ Number of links
    └─ Permissions
```

**First character meanings:**

- `d` = directory
- `-` = regular file
- `l` = symbolic link
- `b` = block device
- `c` = character device

**Permissions breakdown:**

```
drwxr-xr-x
 │││ │││ │││
 │││ │││ │││
 │││ │││ └┴┴─ Others: read, execute
 │││ └┴┴───── Group: read, execute
 │└┴───────── Owner: read, write, execute
 └──────────── Type: directory
```

We'll learn more about permissions in Part 6!

---

#### Option: -la (combined)

You can combine options! This shows all files in long format.

```bash
ls -la
```

**Example output:**

```
total 48
drwxr-xr-x 6 pete users 4096 Oct 24 10:35 .
drwxr-xr-x 3 root root  4096 Oct 20 08:15 ..
-rw------- 1 pete users 2847 Oct 24 09:30 .bash_history
-rw-r--r-- 1 pete users  220 Oct 20 08:15 .bash_logout
drwxr-xr-x 2 pete users 4096 Oct 24 10:30 Documents
-rw-r--r-- 1 pete users 1234 Oct 23 14:20 file.txt
```

---

### More Useful Options

**-h (human-readable sizes):**

```bash
ls -lh
```

Shows file sizes in KB, MB, GB instead of bytes:

```
-rw-r--r-- 1 pete users 1.2K Oct 23 14:20 file.txt
-rw-r--r-- 1 pete users 5.3M Oct 22 11:15 image.jpg
```

**-t (sort by time):**

```bash
ls -lt
```

Shows newest files first.

**-r (reverse order):**

```bash
ls -lr
```

Reverses the sorting order.

**-R (recursive):**

```bash
ls -R
```

Lists all subdirectories and their contents too.

**Combining multiple options:**

```bash
ls -laht
```

Shows: all files, long format, human-readable sizes, sorted by time!

---

### Practical Examples

**Example 1: Find large files**

```bash
ls -lhS
```

The `-S` option sorts by size (largest first).

**Example 2: See what changed recently**

```bash
ls -lt | head
```

Shows the 10 most recently modified files.

**Example 3: Check if a hidden config file exists**

```bash
ls -la | grep .bashrc
```

---

### Real-World Scenarios

**Scenario 1: Web developer checking files**

```bash
$ cd /var/www/mysite
$ ls -lh
-rw-r--r-- 1 www-data www-data  15K Oct 24 10:30 index.html
-rw-r--r-- 1 www-data www-data 3.2K Oct 24 10:28 style.css
drwxr-xr-x 2 www-data www-data 4.0K Oct 24 09:15 images
```

**Scenario 2: Finding configuration files**

```bash
$ cd ~
$ ls -a | grep "^\."
.bash_history
.bash_logout
.bashrc
.profile
```

---

## Basic Navigation Tips

### 1. Tab Completion

**The most important productivity tip!**

Press the `Tab` key while typing to auto-complete:

```bash
cd Doc[Tab]
# Becomes: cd Documents/

ls /usr/lo[Tab]
# Becomes: ls /usr/local/
```

If multiple options exist, press `Tab` twice to see all possibilities:

```bash
cd D[Tab][Tab]
Desktop/   Documents/   Downloads/
```

---

### 2. Command History

**Up/Down arrows:** Cycle through previous commands

```bash
[Up Arrow]    # Shows previous command
[Down Arrow]  # Shows next command
```

**History command:** See all previous commands

```bash
history
```

**Rerun a command by number:**

```bash
!123  # Runs command #123 from history
```

**Rerun the last command:**

```bash
!!
```

**Search history:**
Press `Ctrl+R` and start typing to search through your command history.

---

### 3. Clear the Screen

**Method 1:** Type the command

```bash
clear
```

**Method 2:** Keyboard shortcut

```
Ctrl+L
```

Both clear the terminal screen, giving you a fresh view.

---

### 4. Practice Path Navigation

Here's a practice routine to build muscle memory:

```bash
# Start from home
cd ~
pwd

# List what's here
ls

# Go into Documents
cd Documents
pwd

# Go back to parent
cd ..
pwd

# Go to Desktop
cd Desktop
ls -la

# Return to where you were before
cd -
pwd

# Go home
cd ~
```

**Practice this daily!** Navigation becomes second nature with repetition.

---

### 5. Understanding Absolute vs Relative Paths

**Absolute path:** Starts from the root (`/`)

```bash
cd /home/pete/Documents
```

Works from anywhere in the filesystem.

**Relative path:** Starts from your current location

```bash
cd Documents
```

Only works if `Documents` is in your current directory.

**When to use each:**

- **Absolute:** When you know the exact location
- **Relative:** When you're already nearby

---

### 6. Quick Navigation Patterns

**Jump to home and back:**

```bash
cd /var/log
# Do something...
cd ~
# Do something at home...
cd -
# Back to /var/log
```

**Navigate sibling directories:**

```bash
$ pwd
/home/pete/projects/project1
$ cd ../project2
$ pwd
/home/pete/projects/project2
```

**Go up multiple levels:**

```bash
$ pwd
/home/pete/Documents/work/2024/reports
$ cd ../../..
$ pwd
/home/pete/Documents
```

---

## Summary

In this part, you learned:

✅ **pwd** - Find your current location  
✅ **cd** - Navigate to different directories  
✅ **ls** - List directory contents  
✅ **Special shortcuts** - `.`, `..`, `~`, `-`  
✅ **Tab completion** - Save time typing  
✅ **Command history** - Reuse previous commands

### Quick Reference

```bash
pwd                    # Where am I?
cd /path/to/directory  # Go to specific path
cd ~                   # Go home
cd ..                  # Go up one level
cd -                   # Go to previous location
ls                     # List files
ls -la                 # List all files with details
ls -lh                 # List with human-readable sizes
```

---

## Practice Exercises

Try these exercises to reinforce what you've learned:

**Exercise 1: Basic Navigation**

```bash
# 1. Go to your home directory
# 2. Use pwd to confirm where you are
# 3. List all files (including hidden)
# 4. Go to your Desktop
# 5. Go back to home using a different method
```

**Exercise 2: Using Shortcuts**

```bash
# 1. Navigate to /var/log
# 2. Check your location
# 3. Jump back home using ~
# 4. Return to /var/log using -
```

**Exercise 3: Exploring**

```bash
# 1. From your home directory, go to Documents
# 2. List files in long format
# 3. Go up one level
# 4. List files showing hidden files
# 5. Navigate to a sibling directory
```

---

## Common Issues and Solutions

**Issue: "No such file or directory"**

```bash
$ cd Documets
bash: cd: Documets: No such file or directory
```

**Solution:** Check spelling (use tab completion!) and verify the directory exists with `ls`.

**Issue: "Permission denied"**

```bash
$ cd /root
bash: cd: /root: Permission denied
```

**Solution:** You don't have permission to enter that directory. Use `sudo` if necessary and you have authorization.

**Issue: Lost in the filesystem**
**Solution:** Run `pwd` to see where you are, then `cd ~` to go home.

---

## Next Steps

Great job! You now know how to navigate the Linux filesystem.

**Ready to continue?** Head to [Part 2: File Operations](./02-file-operations.md) to learn how to create, copy, move, and delete files.

**Need to review?** Go back to the [Main Guide](../README.md) to see all available sections.

---

[← Back to Main Guide](../README.md) | [Next: File Operations →](./02-file-operations.md)
