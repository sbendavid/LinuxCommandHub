# Mastering Linux Commands

## A Complete Beginner's Handbook for Learning Linux Command-Line

**Author:** Ben-David (BDofTech)

---

> "Every great developer starts at the command line."

---

## About the Author

Ben-David (BDofTech) is a passionate technology educator dedicated to making complex computing concepts accessible to beginners. With years of experience in Linux system administration and software development, Ben-David believes that mastering the command line is the foundation for anyone looking to excel in the tech industry. This guide represents his commitment to helping newcomers build confidence and competence in Linux environments.

---

## Table of Contents

**Part 1: Getting Started with Linux**

- Understanding the Linux Terminal
- pwd - Print Working Directory
- cd - Change Directory
- ls - List Directories
- Basic Navigation Tips

**Part 2: File Operations**

- touch - Creating Files
- file - Identifying File Types
- cat - Reading Files
- less - Viewing Files Page by Page
- history - Command History
- cp - Copying Files
- mv - Moving and Renaming Files
- mkdir - Making Directories
- rm - Removing Files and Directories
- find - Searching for Files

**Part 3: Getting Help and Shortcuts**

- help Command
- man Pages
- whatis Command
- alias - Creating Shortcuts
- exit and logout

**Part 4: Input/Output and Text Processing**

- Standard Streams (stdout, stdin, stderr)
- Pipes and tee
- Environment Variables
- Text Manipulation Tools (cut, paste, head, tail)
- Text Transformation (expand, join, split, sort, tr, uniq)
- Word Count and Line Numbers (wc, nl)
- grep - Pattern Searching
- Regular Expressions Basics

**Part 5: Text Editors**

- Vim Basics
- Vim Navigation
- Vim Editing Commands
- Emacs Basics
- Emacs Buffer Management
- Emacs Editing

**Part 6: Users, Permissions, and Processes**

- User Management
- File Permissions
- Ownership and Special Permissions
- Process Management (ps, top)
- Signals and Job Control
- Process States

**Part 7: System Administration**

- Package Management
- Device Management
- Filesystem Hierarchy
- Disk Management
- Boot Process
- Kernel Basics
- System Services (System V, Upstart, Systemd)
- System Monitoring
- Cron Jobs
- System Logging

**Part 8: Networking**

- Network Basics
- Network Configuration
- Routing
- DNS
- File Sharing (scp, rsync, NFS, Samba)
- Network Troubleshooting Tools

---

# Part 1: Getting Started with Linux

## Understanding the Linux Terminal

Welcome to the world of Linux! The terminal (also called the command line or shell) is your gateway to powerful system control. While it might seem intimidating at first, the command line gives you precise control over your computer and is essential for system administration, development, and automation.

The terminal uses a command-line interface (CLI) where you type commands and press Enter to execute them. Each command follows a pattern:

```
command [options] [arguments]
```

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

Think of your filesystem as a giant tree. You're always standing at one location (directory) in this tree. The `pwd` command tells you exactly where you are. This is especially useful when you're navigating through many directories and need to confirm your location.

**Practical tip:** Run `pwd` whenever you feel lost. It's your compass in the Linux filesystem.

---

## cd - Change Directory

**What it does:** Moves you to a different directory (folder).

**Syntax:**

```bash
cd [directory]
```

**Examples:**

Move to a specific path:

```bash
cd /home/pete/Desktop
```

Move to a folder in your current location:

```bash
cd Hawaii
```

**Special directory shortcuts:**

- `.` (current directory): Refers to where you are right now

  ```bash
  cd .
  ```

- `..` (parent directory): Takes you one level up

  ```bash
  cd ..
  ```

- `~` (home directory): Takes you to your home folder

  ```bash
  cd ~
  ```

- `-` (previous directory): Returns you to your last location
  ```bash
  cd -
  ```

**Real-world example:**

```bash
$ pwd
/home/pete/Documents
$ cd ../Pictures
$ pwd
/home/pete/Pictures
```

---

## ls - List Directories

**What it does:** Shows you the contents of a directory.

**Basic syntax:**

```bash
ls
```

**View contents of a specific directory:**

```bash
ls /home/pete
```

**Common options:**

- `-a` (all): Shows hidden files (files starting with a dot)

  ```bash
  ls -a
  ```

- `-l` (long format): Displays detailed information

  ```bash
  ls -l
  ```

- `-la` (combined): Shows all files with details
  ```bash
  ls -la
  ```

**Understanding ls -l output:**

```
drwxr-xr-x 2 pete users 4096 Oct 24 10:30 Documents
```

- `d`: It's a directory (would be `-` for a file)
- `rwxr-xr-x`: Permissions
- `2`: Number of links
- `pete`: Owner
- `users`: Group
- `4096`: Size in bytes
- `Oct 24 10:30`: Last modified
- `Documents`: Name

---

## Basic Navigation Tips

**1. Tab completion:** Press Tab while typing to auto-complete commands and file names. This saves time and reduces typos.

**2. Up arrow:** Press the up arrow to cycle through previous commands.

**3. Clear screen:** Type `clear` or press `Ctrl+L` to clear the terminal.

**4. Practice path navigation:**

```bash
# Go to home directory
cd ~

# List contents
ls

# Go to a subdirectory
cd Documents

# Go back one level
cd ..

# Check where you are
pwd
```

---

# Part 2: File Operations

## touch - Creating Files

**What it does:** Creates an empty file or updates the timestamp of an existing file.

**Syntax:**

```bash
touch filename
```

**Example:**

```bash
touch mysuperduperfile
```

**Why it's useful:** While `touch` is often used to create empty files for testing, its primary purpose is to update file timestamps. In real-world scenarios, you might use it to trigger systems that watch for file changes.

---

## file - Identifying File Types

**What it does:** Tells you what type of file you're looking at.

**Syntax:**

```bash
file filename
```

**Example:**

```bash
$ file banana.jpg
banana.jpg: JPEG image data, JFIF standard 1.01
```

**Why it matters:** Unlike Windows, Linux doesn't rely on file extensions to determine file types. The `file` command examines the actual content to tell you what type of file it is. This is crucial because a file named `document.txt` could actually be an image!

---

## cat - Reading Files

**What it does:** Displays the entire content of one or more files.

**Syntax:**

```bash
cat filename
```

**Read multiple files:**

```bash
cat dogfile birdfile
```

**When to use cat:**

- For small files you want to read quickly
- When combining multiple files
- When you need to see everything at once

**Limitations:** Not ideal for large files since it dumps everything to your screen at once. For large files, use `less` instead.

---

## less - Viewing Files Page by Page

**What it does:** Opens files in a paginated viewer, perfect for large files.

**Syntax:**

```bash
less /home/pete/Documents/text1
```

**Navigation commands within less:**

- `q`: Quit and return to terminal
- `Page Up/Page Down`: Navigate by pages
- `Up/Down arrows`: Navigate line by line
- `g`: Jump to the beginning
- `G`: Jump to the end
- `/search`: Search for text (type `/` followed by your search term)
- `h`: Show help

**Pro tip:** Think of `less` as a read-only text viewer. It's perfect for log files, documentation, and any large text file you need to browse through carefully.

---

## history - Command History

**What it does:** Shows a list of commands you've previously executed.

**Syntax:**

```bash
history
```

**Useful tricks:**

- `!123`: Re-run command number 123 from your history
- `!!`: Re-run the last command
- `!$`: Use the last argument from the previous command

**Clear your terminal:**

```bash
clear
```

---

## cp - Copying Files

**What it does:** Creates a duplicate of a file or directory.

**Basic syntax:**

```bash
cp source destination
```

**Examples:**

Copy a file to a directory:

```bash
cp mycoolfile /home/pete/Documents/cooldocs
```

Copy multiple files using wildcards:

```bash
cp *.jpg /home/pete/Pictures
```

**Wildcard characters:**

- `*`: Matches any number of characters
  - `*.jpg` matches all files ending in .jpg
- `?`: Matches exactly one character
  - `file?.txt` matches file1.txt, fileA.txt, etc.
- `[]`: Matches any character within brackets
  - `file[123].txt` matches file1.txt, file2.txt, file3.txt

**Copy directories:**

```bash
cp -r Pumpkin/ /home/pete/Documents
```

The `-r` (recursive) flag is required to copy directories.

**Interactive copy (ask before overwriting):**

```bash
cp -i mycoolfile /home/pete/Pictures
```

---

## mv - Moving and Renaming Files

**What it does:** Moves files/directories to a new location OR renames them.

**Rename a file:**

```bash
mv oldfile newfile
```

**Move a file to a directory:**

```bash
mv file2 /home/pete/Documents
```

**Move multiple files:**

```bash
mv file_1 file_2 /somedirectory
```

**Rename a directory:**

```bash
mv directory1 directory2
```

**Safety options:**

- `-i` (interactive): Ask before overwriting

  ```bash
  mv -i directory1 directory2
  ```

- `-b` (backup): Create a backup of the destination if it exists
  ```bash
  mv -b directory1 directory2
  ```

**Important note:** Unlike `cp`, `mv` doesn't create a copy—it actually moves the file. Be careful with your destinations!

---

## mkdir - Making Directories

**What it does:** Creates new directories (folders).

**Create multiple directories:**

```bash
mkdir books paintings
```

**Create nested directories:**

```bash
mkdir -p books/hemmingway/favorites
```

The `-p` (parents) flag creates all parent directories that don't exist. Without it, `mkdir` would fail if `books` or `books/hemmingway` didn't already exist.

---

## rm - Removing Files and Directories

**What it does:** Permanently deletes files and directories.

**⚠️ WARNING:** There's no recycle bin in Linux. Once you delete something with `rm`, it's gone forever!

**Delete a file:**

```bash
rm file1
```

**Force deletion (no questions asked):**

```bash
rm -f file1
```

**Interactive deletion (asks for confirmation):**

```bash
rm -i file
```

**Delete a directory and its contents:**

```bash
rm -r directory
```

**Remove an empty directory:**

```bash
rmdir directory
```

**Safety tip:** Always double-check before running `rm`, especially with `-r` (recursive) and `-f` (force). The combination `rm -rf` is particularly dangerous and should be used with extreme caution.

---

## find - Searching for Files

**What it does:** Searches for files and directories based on various criteria.

**Find by name:**

```bash
find /home -name puppies.jpg
```

**Find directories by name:**

```bash
find /home -type d -name MyFolder
```

**Common find options:**

- `-name`: Search by name
- `-type d`: Only directories
- `-type f`: Only files
- `-size`: Search by size
- `-mtime`: Search by modification time

**Example: Find all .txt files in your home directory:**

```bash
find ~ -name "*.txt"
```

---

# Part 3: Getting Help and Shortcuts

## help Command

**What it does:** Provides quick help for built-in shell commands.

**Syntax:**

```bash
help command_name
```

**Example:**

```bash
help echo
```

**Alternative format:**

```bash
echo --help
```

Many commands support the `--help` flag, which displays usage information.

---

## man - Manual Pages

**What it does:** Opens the comprehensive manual for a command.

**Syntax:**

```bash
man command_name
```

**Example:**

```bash
man ls
```

**How to navigate man pages:**

- Use arrow keys or Page Up/Down to scroll
- Press `q` to quit
- Press `/` to search within the manual
- Press `n` to go to the next search result

**Pro tip:** Man pages are your best friend. They contain detailed information about every aspect of a command, including all options, examples, and related commands.

---

## whatis Command

**What it does:** Gives you a one-line description of a command.

**Syntax:**

```bash
whatis command_name
```

**Example:**

```bash
$ whatis cat
cat (1) - concatenate files and print on standard output
```

**When to use it:** Perfect for when you need a quick reminder of what a command does without opening the full manual.

---

## alias - Creating Shortcuts

**What it does:** Creates custom shortcuts for commands you use frequently.

**Create an alias:**

```bash
alias foobar='ls -la'
```

Now typing `foobar` will execute `ls -la`.

**Remove an alias:**

```bash
unalias foobar
```

**View all aliases:**

```bash
alias
```

**Making aliases permanent:** Add them to your `~/.bashrc` or `~/.bash_profile` file so they persist across sessions.

**Common useful aliases:**

```bash
alias ll='ls -la'
alias ..='cd ..'
alias update='sudo apt update && sudo apt upgrade'
```

---

## exit and logout

**What they do:** Close your terminal session.

**Exit:**

```bash
exit
```

**Logout:**

```bash
logout
```

Both commands end your current shell session. Use them when you're done working in the terminal or when switching users.

---

# Part 4: Input/Output and Text Processing

## Standard Streams (stdout, stdin, stderr)

Linux uses three standard data streams for input and output:

### stdout (Standard Output)

**What it is:** The normal output from commands, displayed in your terminal.

**Redirect output to a file (overwrite):**

```bash
echo Hello World > peanuts.txt
```

**Redirect output to a file (append):**

```bash
echo Hello World >> peanuts.txt
```

**Difference:**

- `>` creates or overwrites a file
- `>>` appends to an existing file

---

### stdin (Standard Input)

**What it is:** The input stream where commands receive data.

**Redirect input from a file:**

```bash
cat < peanuts.txt
```

**Combine input and output redirection:**

```bash
cat < peanuts.txt > banana.txt
```

This reads from peanuts.txt and writes to banana.txt.

---

### stderr (Standard Error)

**What it is:** The error message stream, separate from normal output.

**Example of stderr:**

```bash
ls /fake/directory
```

This produces an error message sent to stderr.

**Redirect stderr to a file:**

```bash
ls /fake/directory 2> peanuts.txt
```

**Redirect both stdout and stderr:**

```bash
ls /fake/directory > peanuts.txt 2>&1
```

Or the shorthand:

```bash
ls /fake/directory &> peanuts.txt
```

**Discard error messages:**

```bash
ls /fake/directory 2> /dev/null
```

`/dev/null` is a special file that discards all data written to it.

---

## pipe and tee

**Pipe (|):** Sends the output of one command as input to another.

**Example:**

```bash
ls | grep "file"
```

This lists all files and filters for those containing "file".

**tee command:** Writes output to both a file AND the screen.

**Syntax:**

```bash
ls | tee peanuts.txt
```

This displays the output of `ls` while also saving it to peanuts.txt.

---

## env - Environment Variables

**What it does:** Displays all environment variables.

**Syntax:**

```bash
env
```

**Environment variables** are named values that affect how processes run. Common examples include `PATH` (where the system looks for commands) and `HOME` (your home directory).

---

## cut - Extracting Text

**What it does:** Cuts out specific portions of text from each line.

**Extract specific character:**

```bash
cut -c 5 sample.txt
```

Gets the 5th character from each line.

**Extract specific field:**

```bash
cut -f 2 sample.txt
```

Gets the 2nd field (columns are separated by tabs by default).

**Use custom delimiter:**

```bash
cut -f 1 -d ";" sample.txt
```

Gets the 1st field, using semicolon as the separator.

---

## paste - Merging Lines

**What it does:** Merges lines from files.

**Merge lines sequentially:**

```bash
paste -s sample2.txt
```

**Use custom delimiter:**

```bash
paste -d ' ' -s sample2.txt
```

Merges lines separated by spaces.

---

## head - View File Beginning

**What it does:** Shows the first lines of a file.

**Default (first 10 lines):**

```bash
head /var/log/syslog
```

**Specify number of lines:**

```bash
head -n 15 /var/log/syslog
```

---

## tail - View File End

**What it does:** Shows the last lines of a file.

**Default (last 10 lines):**

```bash
tail /var/log/syslog
```

**Specify number of lines:**

```bash
tail -n 10 /var/log/syslog
```

**Follow a file in real-time:**

```bash
tail -f /var/log/syslog
```

The `-f` flag is incredibly useful for watching log files as they update.

---

## expand and unexpand

**expand:** Converts tabs to spaces.

```bash
expand sample.txt
expand sample.txt > result.txt
```

**unexpand:** Converts spaces to tabs.

```bash
unexpand -a result.txt
```

---

## join and split

**join:** Combines lines from two files based on a common field.

**Basic join:**

```bash
join file1.txt file2.txt
```

**Join on specific fields:**

```bash
join -1 2 -2 1 file1.txt file2.txt
```

Joins using field 2 from file1 and field 1 from file2.

**split:** Breaks a file into smaller pieces.

```bash
split somefile
```

Creates files named xaa, xab, xac, etc.

---

## sort - Sorting Lines

**What it does:** Sorts lines in a file.

**Basic sort:**

```bash
sort file1.txt
```

**Reverse sort:**

```bash
sort -r file1.txt
```

**Numerical sort:**

```bash
sort -n file1.txt
```

Sorts by numerical value instead of alphabetically.

---

## tr - Translate Characters

**What it does:** Translates or deletes characters.

**Convert lowercase to uppercase:**

```bash
tr a-z A-Z
```

**Example with pipes:**

```bash
echo "hello world" | tr a-z A-Z
```

Output: HELLO WORLD

---

## uniq - Remove Duplicates

**What it does:** Filters out repeated adjacent lines.

**Basic usage:**

```bash
uniq reading.txt
```

**Count occurrences:**

```bash
uniq -c reading.txt
```

**Show only unique lines:**

```bash
uniq -u reading.txt
```

**Show only duplicate lines:**

```bash
uniq -d reading.txt
```

**Important:** `uniq` only removes adjacent duplicates. Always sort first:

```bash
sort reading.txt | uniq
```

---

## wc and nl

**wc (word count):** Counts lines, words, and characters.

**Full count:**

```bash
wc /etc/passwd
```

**Count only lines:**

```bash
wc -l /etc/passwd
```

**nl (number lines):** Adds line numbers to a file.

```bash
nl file1.txt
```

---

## grep - Pattern Searching

**What it does:** Searches for patterns in text.

**Basic search:**

```bash
grep fox sample.txt
```

**Case-insensitive search:**

```bash
grep -i somepattern somefile
```

**Use with pipes:**

```bash
env | grep -i User
```

**Search for files with specific extensions:**

```bash
ls /somedir | grep '\.txt$'
```

**grep is powerful:** It's one of the most frequently used commands for finding text in files, logs, and command output.

---

## regex - Regular Expressions Basics

Regular expressions (regex) are patterns used to match text. Here are fundamental concepts:

### Test string for examples:

```
sally sells seashells
by the seashore
```

### 1. Beginning of a line with ^

```
^by
```

Matches lines starting with "by"

- Matches: "by the seashore"

### 2. End of a line with $

```
seashore$
```

Matches lines ending with "seashore"

- Matches: "by the seashore"

### 3. Matching any single character with .

```
b.
```

The dot matches any single character

- Matches: "by"

### 4. Bracket notation with []

**Match any character in brackets:**

```
d[iou]g
```

- Matches: dig, dog, dug

**Exclude characters (^ inside brackets):**

```
d[^i]g
```

- Matches: dog, dug
- Doesn't match: dig

**Character ranges:**

```
d[a-c]g
```

- Matches: dag, dbg, dcg

**Case sensitivity:**

```
d[A-C]g
```

- Matches: dAg, dBg, dCg
- Doesn't match: dag, dbg, dcg

**Pro tip:** Regular expressions are a vast topic. These basics will help you get started, but there's much more to learn for advanced pattern matching.

---

# Part 5: Text Editors

## Vim (Vi Improved)

**What it is:** A powerful, modal text editor ubiquitous in Unix-like systems.

**Start Vim:**

```bash
vim filename
```

**Vim modes:**

- **Normal mode:** For navigation and commands (default)
- **Insert mode:** For typing text
- **Command mode:** For saving, quitting, etc.

---

## Vim Search Patterns

### Test file content:

```
My pretty file is very pretty.
```

**Search forward:**

```
/pretty
```

Press `n` to go to the next match.

**Search backward:**

```
?pretty
```

---

## Vim Navigation

**In Normal mode:**

- `h` or Left arrow: Move left one character
- `j` or Down arrow: Move down one line
- `k` or Up arrow: Move up one line
- `l` or Right arrow: Move right one character

**Why hjkl?** On old keyboards, these keys had arrow labels. They're faster because your fingers don't leave the home row.

---

## Vim Inserting and Appending Text

**Press Esc to return to Normal mode at any time.**

**Enter Insert mode:**

- `i`: Insert before cursor
- `a`: Append after cursor
- `I`: Insert at beginning of line
- `A`: Append at end of line
- `o`: Open new line below and start inserting
- `O`: Open new line above and start inserting

**Example workflow:**

1. Press `i` to enter Insert mode
2. Type your text
3. Press Esc to return to Normal mode

---

## Vim Editing

**Press Esc to ensure you're in Normal mode before using these commands.**

### Deleting (operator d):

- `x`: Delete character under cursor
- `dw`: Delete from cursor to start of next word
- `d$`: Delete from cursor to end of line
- `dd`: Delete the current line
- `3dd`: Delete three lines
- `2dw`: Delete two words

### Changing (operator c):

Changes delete text AND put you in Insert mode:

- `cw`: Change word from cursor
- `c$`: Change to end of line
- `cc`: Change the whole line

### Yank and Put (copy/paste):

- `yw`: Yank (copy) word
- `yy`: Yank current line
- `p`: Put (paste) after cursor/below line
- `P`: Put before cursor/above line

### Other useful edits:

- `r{char}`: Replace character under cursor with {char}
- `R`: Enter Replace mode (overwrite text)
- `J`: Join current line with next line
- `.`: Repeat the last change (super useful!)

---

## Vim Saving and Exiting

**Enter Command mode by typing `:` in Normal mode.**

- `:w`: Write (save) the file
- `:q`: Quit Vim
- `:wq`: Write and quit
- `:q!`: Quit without saving (force quit)
- `ZZ`: Write and quit (faster alternative)
- `u`: Undo last action
- `Ctrl-r`: Redo last action

**Learning Vim:** It has a steep learning curve, but once you master it, you'll edit text faster than ever. Practice with `vimtutor` by typing it in your terminal.

---

## Emacs

**What it is:** Another powerful, highly customizable text editor.

**Start Emacs:**

```bash
emacs filename
```

**Key notation:**

- `C-x`: Hold Ctrl and press x
- `M-x`: Hold Alt (Meta) and press x

---

## Emacs Manipulate Files

### Saving files:

- `C-x C-s`: Save file
- `C-x C-w`: Save file as (new name)
- `C-x s`: Save all buffers

### Opening a file:

- `C-x C-f`: Find (open) file

---

## Emacs Buffer Navigation

**Buffers** in Emacs are like tabs—they hold file contents.

### Switch buffers:

- `C-x b`: Switch to buffer
- `C-x Right arrow`: Cycle right through buffers
- `C-x Left arrow`: Cycle left through buffers

### Close buffer:

- `C-x k`: Kill (close) buffer

### Split screen:

- `C-x 2`: Split horizontally
- `C-x o`: Switch between split buffers
- `C-x 1`: Make current buffer full screen

---

## Emacs Editing

### Text Navigation:

- `C-Up arrow`: Move up one paragraph
- `C-Down arrow`: Move down one paragraph
- `C-Left arrow`: Move one word left
- `C-Right arrow`: Move one word right
- `M->`: Move to end of buffer

### Cutting and Pasting:

- `C-w`: Cut (kill)
- `C-y`: Yank (paste)

---

## Emacs Exiting and Help

### Close Emacs:

- `C-x C-c`: Exit Emacs
  - Will prompt to save unsaved buffers

### Get help:

- `C-h C-h`: Open help menu

### Undo:

- `C-x u`: Undo

**Emacs vs Vim:** This is one of tech's classic debates. Both are powerful; try both and see which fits your workflow. Many developers know both!

---

# Part 6: Users, Permissions, and Processes

## root User

**What it is:** The superuser account with unrestricted access to everything.

**Switch to root:**

```bash
su
```

**Why be careful:** With great power comes great responsibility. As root, you can accidentally destroy your entire system. Use root access sparingly and carefully.

---

## /etc/passwd - User Information

**What it is:** A file containing basic information about all users.

**View it:**

```bash
cat /etc/passwd
```

**Example line:**

```
pete:x:1000:1000:Pete User:/home/pete:/bin/bash
```

**Fields (separated by colons):**

1. Username: pete
2. Password: x (actual password is in /etc/shadow)
3. User ID: 1000
4. Group ID: 1000
5. Description: Pete User
6. Home directory: /home/pete
7. Default shell: /bin/bash

---

## /etc/shadow - Password Storage

**What it is:** Encrypted password storage (only root can read it).

**View it:**

```bash
sudo cat /etc/shadow
```

**Example:**

```
root:MyEPTEa$6Nonsense:15000:0:99999:7:::
```

This file contains encrypted passwords and password aging information.

---

## /etc/group - Group Information

**What it is:** Information about user groups.

**View it:**

```bash
cat /etc/group
```

**Example:**

```
root:*:0:pete
```

**Format:** groupname:password:GID:users

---

## User Management Tools

### Adding Users:

**useradd (basic):**

```bash
sudo useradd bob
```

**adduser (interactive, easier):**

```bash
sudo adduser bob
```

### Removing Users:

**userdel:**

```bash
sudo userdel bob
```

**deluser:**

```bash
sudo deluser bob
```

### Changing Passwords:

```bash
passwd bob
```

---

## File Permissions

**Permission format:**

```
d | rwx | r-x | r-x
```

**Breaking it down:**

- `d`: Type (d=directory, -=file, l=link)
- `rwx`: Owner permissions
- `r-x`: Group permissions
- `r-x`: Other (everyone else) permissions

**Permission meanings:**

- `r`: readable
- `w`: writable
- `x`: executable
- `-`: permission not granted

**Example:**

```
-rw-r--r-- 1 pete users 1234 Oct 24 10:00 myfile.txt
```

- Owner (pete): read and write
- Group (users): read only
- Others: read only

---

## Modifying Permissions

### Symbolic method:

**Add permission:**

```bash
chmod u+x myfile
```

Adds execute permission for the owner (user).

**Remove permission:**

```bash
chmod u-x myfile
```

**Add multiple permissions:**

```bash
chmod ug+w myfile
```

Adds write permission for owner (u) and group (g).

**Permission shortcuts:**

- `u`: user (owner)
- `g`: group
- `o`: others
- `a`: all

---

### Numerical method:

**Permission values:**

- 4: read
- 2: write
- 1: execute

**Add values for desired permissions:**

- 7 (4+2+1): read, write, execute
- 6 (4+2): read, write
- 5 (4+1): read, execute
- 4: read only

**Example:**

```bash
chmod 755 myfile
```

- Owner: 7 (rwx)
- Group: 5 (r-x)
- Others: 5 (r-x)

**Common permission sets:**

- `644`: Standard file (owner rw, others r)
- `755`: Standard directory or executable
- `600`: Private file (owner only)
- `700`: Private directory (owner only)

---

## Ownership Permissions

### Change file owner:

```bash
sudo chown patty myfile
```

### Change group:

```bash
sudo chgrp whales myfile
```

### Change both at once:

```bash
sudo chown patty:whales myfile
```

---

## umask - Default Permissions

**What it is:** Sets default permissions for newly created files and directories.

**View current umask:**

```bash
umask
```

**Set umask:**

```bash
umask 021
```

**How it works:** umask values are subtracted from the default permissions (666 for files, 777 for directories).

---

## Setuid (SUID)

**What it is:** Allows a file to run with the permissions of its owner, not the user executing it.

**Why it matters:** Some programs need root privileges to run, but you don't want to give users root access. SUID solves this.

### Modifying SUID:

**Symbolic way:**

```bash
sudo chmod u+s myfile
```

**Numerical way:**

```bash
sudo chmod 4755 myfile
```

The leading 4 sets SUID.

**Identifying SUID files:**

```bash
ls -l
-rwsr-xr-x
```

Notice the `s` instead of `x` in the owner's execute position.

---

## Setgid (SGID)

**What it is:** Similar to SUID, but for groups. Files created in an SGID directory inherit the directory's group.

### Modifying SGID:

**Symbolic way:**

```bash
sudo chmod g+s myfile
```

**Numerical way:**

```bash
sudo chmod 2555 myfile
```

---

## The Sticky Bit

**What it is:** On directories, prevents users from deleting files they don't own (even if they have write permission to the directory).

**Common use:** The `/tmp` directory uses the sticky bit so users can't delete each other's temporary files.

### Modify sticky bit:

**Symbolic way:**

```bash
sudo chmod +t mydir
```

**Numerical way:**

```bash
sudo chmod 1755 mydir
```

**Identifying sticky bit:**

```bash
drwxrwxrwt
```

Notice the `t` in the others' execute position.

---

## ps - Viewing Processes

**What it does:** Shows information about running processes.

**Basic usage:**

```bash
ps
```

**Example output:**

```
PID   TTY      STAT   TIME   CMD
41230 pts/4    Ss     00:00:00 bash
51224 pts/4    R+     00:00:00 ps
```

**Column meanings:**

- **PID:** Process ID (unique identifier)
- **TTY:** Controlling terminal
- **STAT:** Process status (R=running, S=sleeping, Z=zombie)
- **TIME:** Total CPU usage time
- **CMD:** Command name

**View all processes:**

```bash
ps aux
```

This shows every process running on the system, with detailed information.

---

## top - Interactive Process Viewer

**What it does:** Displays processes in real-time, updated continuously.

**Start top:**

```bash
top
```

**Useful keys in top:**

- `q`: Quit
- `k`: Kill a process
- `M`: Sort by memory usage
- `P`: Sort by CPU usage
- `h`: Help

**Top is like Task Manager** on Windows—it shows you what's using your system resources.

---

## Process Details

**View detailed process info:**

```bash
ps l
```

This shows additional fields like priority and nice values.

---

## Signals - Controlling Processes

**What they are:** Messages sent to processes to control their behavior.

### Common signals:

- **SIGHUP (1):** Hangup (reload configuration)
- **SIGINT (2):** Interrupt (Ctrl+C)
- **SIGKILL (9):** Kill immediately (cannot be caught)
- **SIGTERM (15):** Terminate gracefully (default)
- **SIGSTOP:** Stop (pause) process

---

## kill - Terminate Processes

**What it does:** Sends signals to processes (despite the name, it doesn't always kill them).

**Terminate gracefully:**

```bash
kill 12445
```

**Force kill:**

```bash
kill -9 12445
```

**Using signal names:**

```bash
kill -SIGTERM 12445
```

**Kill by name:**

```bash
killall firefox
```

---

## niceness - Process Priority

**What it is:** Priority level for processes (-20 to 19, lower = higher priority).

**Start process with nice value:**

```bash
nice -n 5 apt upgrade
```

**Change priority of running process:**

```bash
renice 10 -p 3245
```

**Who can do what:**

- Regular users: Can only increase niceness (lower priority)
- Root: Can set any niceness value

---

## /proc Filesystem

**What it is:** A virtual filesystem containing process information.

**Browse processes:**

```bash
ls /proc
```

You'll see numbered directories—each represents a running process (the number is the PID).

**View process status:**

```bash
cat /proc/12345/status
```

---

## Job Control

**Managing background and foreground processes.**

### Send job to background:

**At start:**

```bash
sleep 1000 &
```

**Already running:**

1. Press `Ctrl+Z` to suspend
2. Type `bg` to continue in background

### View background jobs:

```bash
jobs
```

**Example output:**

```
[1]   Running    sleep 1000 &
[2]-  Running    sleep 1001 &
[3]+  Running    sleep 1002 &
```

### Move job to foreground:

```bash
fg %1
```

### Kill background job:

```bash
kill %1
```

---

# Part 7: System Administration

## Package Management Overview

**What it is:** Tools for installing, updating, and removing software.

Linux distributions use different package managers:

- **Debian/Ubuntu:** dpkg, apt
- **Red Hat/Fedora:** rpm, yum, dnf

---

## tar and gzip - Archiving Files

### Compressing with gzip:

**Compress:**

```bash
gzip mycoolfile
```

Creates: mycoolfile.gz

**Decompress:**

```bash
gunzip mycoolfile.gz
```

---

### Creating archives with tar:

**Create archive:**

```bash
tar cvf mytarfile.tar mycoolfile1 mycoolfile2
```

**Flags:**

- `c`: create
- `v`: verbose (show progress)
- `f`: filename

**Extract archive:**

```bash
tar xvf mytarfile.tar
```

**Flags:**

- `x`: extract

---

### Compressed archives:

**Create compressed tar:**

```bash
tar czf myfile.tar.gz directory/
```

**Extract compressed tar:**

```bash
tar xzf myfile.tar.gz
```

**The `z` flag:** Handles gzip compression automatically.

---

## dpkg and rpm - Package Installers

### dpkg (Debian):

**Install:**

```bash
dpkg -i some_deb_package.deb
```

**Remove:**

```bash
dpkg -r some_deb_package.deb
```

**List installed:**

```bash
dpkg -l
```

---

### rpm (Red Hat):

**Install:**

```bash
rpm -i some_rpm_package.rpm
```

**Remove:**

```bash
rpm -e some_rpm_package.rpm
```

**List installed:**

```bash
rpm -qa
```

---

## apt and yum - Repository Management

### apt (Debian/Ubuntu):

**Install package:**

```bash
apt install package_name
```

**Remove package:**

```bash
apt remove package_name
```

**Update package lists:**

```bash
apt update
```

**Upgrade packages:**

```bash
apt upgrade
```

**Get package info:**

```bash
apt show package_name
```

**Full system update:**

```bash
apt update && apt upgrade
```

---

### yum (Red Hat/CentOS):

**Install:**

```bash
yum install package_name
```

**Remove:**

```bash
yum erase package_name
```

**Update:**

```bash
yum update
```

**Get info:**

```bash
yum info package_name
```

---

## Compiling Source Code

**When you need it:** Sometimes software isn't available as a package and must be compiled from source.

**Basic steps:**

1. **Install build tools:**

```bash
sudo apt install build-essential
```

2. **Extract source:**

```bash
tar -xzvf package.tar.gz
cd package/
```

3. **Configure:**

```bash
./configure
```

4. **Compile:**

```bash
make
```

5. **Install:**

```bash
sudo make install
```

6. **Uninstall:**

```bash
sudo make uninstall
```

**Better alternative:**

```bash
sudo checkinstall
```

Creates a package that can be easily removed later.

---

## Device Management

### /dev Directory

**What it is:** Contains device files that represent hardware.

**View devices:**

```bash
ls /dev
```

### Device Types:

- **Character devices:** Transfer data character by character (keyboards, mice)
- **Block devices:** Transfer data in blocks (hard drives)
- **Pipe devices:** Allow process communication
- **Socket devices:** Network communication

---

### Common Device Names:

**SCSI/SATA devices:**

- `/dev/sda`: First hard disk
- `/dev/sdb`: Second hard disk
- `/dev/sda1`: First partition on first disk
- `/dev/sda2`: Second partition on first disk

**Pseudo devices:**

- `/dev/null`: Discards all data (black hole)
- `/dev/zero`: Produces null bytes
- `/dev/random`: Produces random numbers

**Old PATA devices:**

- `/dev/hda`: First IDE hard disk
- `/dev/hdb`: Second IDE hard disk

---

## sysfs - Device Information

**What it is:** Modern interface for device information.

**View device info:**

```bash
ls /sys/block/sda
```

---

## udev - Device Manager

**What it does:** Dynamically manages device files in /dev.

**View device information:**

```bash
udevadm info --query=all --name=/dev/sda
```

---

## Device Listing Commands

**List USB devices:**

```bash
lsusb
```

**List PCI devices:**

```bash
lspci
```

**List SCSI devices:**

```bash
lsscsi
```

---

## dd - Data Duplicator

**What it does:** Low-level copying of data (powerful and dangerous).

**Basic syntax:**

```bash
dd if=input_file of=output_file bs=block_size count=number
```

**Example - Create disk image:**

```bash
dd if=/dev/sda of=/home/pete/backup.img bs=1M
```

**Restore from image:**

```bash
dd if=/home/pete/backup.img of=/dev/sdb bs=1M
```

**Flags:**

- `if`: input file
- `of`: output file
- `bs`: block size
- `count`: number of blocks

**⚠️ WARNING:** dd is nicknamed "disk destroyer" for a reason. Double-check your commands before running!

---

## Filesystem Hierarchy

**Understanding where everything lives:**

- `/`: Root directory (top of everything)
- `/bin`: Essential user commands
- `/boot`: Bootloader and kernel files
- `/dev`: Device files
- `/etc`: System configuration files
- `/home`: User home directories
- `/lib`: Shared libraries
- `/media`: Removable media mount points
- `/mnt`: Temporary mount points
- `/opt`: Optional software packages
- `/proc`: Process information (virtual)
- `/root`: Root user's home directory
- `/run`: Runtime data since last boot
- `/sbin`: System administration binaries
- `/srv`: Service data
- `/tmp`: Temporary files
- `/usr`: User programs and data
- `/var`: Variable data (logs, caches)

---

## Filesystem Types

### Common filesystems:

- **ext4:** Standard Linux filesystem (most common)
- **Btrfs:** Modern filesystem with snapshots
- **XFS:** High-performance for large files
- **NTFS:** Windows filesystem
- **FAT32:** Universal compatibility
- **HFS+:** macOS filesystem

**View filesystem types:**

```bash
df -T
```

---

## Disk Anatomy

### Key concepts:

- **Partition table:** Map of how disk is divided
- **Partitions:** Sections of the disk
- **Filesystem:** Data structure on a partition

### Partition table types:

- **MBR (Master Boot Record):** Older standard, 4 primary partitions max
- **GPT (GUID Partition Table):** Modern standard, unlimited partitions

**View partition table:**

```bash
sudo parted -l
```

---

## Disk Partitioning Tools

**Available tools:**

- `fdisk`: Basic, MBR only
- `parted`: Command-line, supports MBR and GPT
- `gparted`: GUI version of parted
- `gdisk`: GPT only

### Using parted:

**Start parted:**

```bash
sudo parted
```

**Select device:**

```
select /dev/sdb
```

**View partitions:**

```
print
```

**Create partition:**

```
mkpart primary 123 4567
```

**Resize partition:**

```
resize 2 1245 3456
```

---

## Creating Filesystems

**Format a partition:**

```bash
sudo mkfs -t ext4 /dev/sdb2
```

**This creates:** An ext4 filesystem on partition /dev/sdb2.

---

## mount and umount

**What mounting means:** Making a filesystem accessible at a specific location.

**Mount a filesystem:**

```bash
sudo mount -t ext4 /dev/sdb2 /mydrive
```

**Unmount:**

```bash
sudo umount /mydrive
```

or

```bash
sudo umount /dev/sdb2
```

**View mounted filesystems:**

```bash
mount
```

**View UUIDs:**

```bash
sudo blkid
```

---

## /etc/fstab - Automatic Mounting

**What it is:** Configuration file for automatic mounting at boot.

**View fstab:**

```bash
cat /etc/fstab
```

**Example entry:**

```
UUID=xxxx-xxxx /mydrive ext4 defaults 0 2
```

**Fields:**

1. Device (UUID or path)
2. Mount point
3. Filesystem type
4. Mount options
5. Dump backup setting
6. Filesystem check order

---

## swap - Virtual Memory

**What it is:** Disk space used as extra RAM when memory is full.

### Using a partition for swap:

**Initialize swap:**

```bash
sudo mkswap /dev/sdb2
```

**Enable swap:**

```bash
sudo swapon /dev/sdb2
```

**Disable swap:**

```bash
sudo swapoff /dev/sdb2
```

**Add to fstab for persistence:**

```
/dev/sdb2 none swap sw 0 0
```

---

## Disk Usage

**View disk space:**

```bash
df -h
```

The `-h` flag shows human-readable sizes (GB, MB, etc.).

**View directory sizes:**

```bash
du -h /path/to/directory
```

---

## Filesystem Repair

**Check and repair filesystem:**

```bash
sudo fsck /dev/sda
```

**⚠️ IMPORTANT:** Never run fsck on a mounted filesystem! Unmount it first.

---

## Inodes

**What they are:** Data structures storing file metadata (permissions, timestamps, location).

**View inode numbers:**

```bash
ls -li
```

**View inode information:**

```bash
stat ~/Desktop/
```

**Each file has:** One inode containing all its metadata except the filename.

---

## Symbolic Links (symlinks)

**What they are:** Shortcuts to files or directories.

**Create symlink:**

```bash
ln -s myfile mylink
```

**Create hardlink:**

```bash
ln somefile somelink
```

**Difference:**

- **Symlink:** Points to filename (breaks if original deleted)
- **Hardlink:** Points to inode (survives original deletion)

---

## Boot Process Overview

### Four main stages:

1. **BIOS/UEFI:** Hardware initialization
2. **Bootloader:** Loads the kernel
3. **Kernel:** Initializes the system
4. **Init:** Starts system services

---

## BIOS vs UEFI

**BIOS (Basic Input/Output System):**

- Older firmware standard
- Uses MBR partition table
- Limited to 2TB disks

**UEFI (Unified Extensible Firmware Interface):**

- Modern replacement for BIOS
- Uses GPT partition table
- Supports larger disks
- Faster boot times
- Secure boot capability

---

## Bootloader

**Common bootloaders:**

- **GRUB (GRand Unified Bootloader):** Most common
- **LILO:** Older Linux bootloader
- **systemd-boot:** Newer, simpler bootloader

**What it does:** Loads the kernel and initial RAM disk into memory.

---

## Kernel Initialization

**The kernel:**

1. Probes hardware
2. Mounts root filesystem
3. Loads necessary drivers
4. Starts init process

**initrd/initramfs:** Temporary root filesystem used during boot to load necessary modules before mounting the real root filesystem.

---

## Init Systems

**Three major init systems:**

### 1. System V init (SysV):

- Traditional Unix init system
- Uses runlevels (0-6)
- Sequential startup

### 2. Upstart:

- Event-based
- Parallel startup
- Used by older Ubuntu versions

### 3. Systemd:

- Modern standard
- Parallel startup
- Service dependencies
- Used by most current distributions

---

## Kernel Information

**View kernel version:**

```bash
uname -r
```

**Kernel location:**

- `vmlinuz`: The actual kernel
- `initrd`: Initial RAM disk
- `System.map`: Symbol lookup table
- `config`: Kernel configuration

---

## Kernel Modules

**What they are:** Loadable kernel code (drivers, features).

**List loaded modules:**

```bash
lsmod
```

**Load a module:**

```bash
sudo modprobe bluetooth
```

**Remove a module:**

```bash
sudo modprobe -r bluetooth
```

**Load on bootup:**
Edit `/etc/modprobe.d/module.conf`:

```
options module_name parameter=value
```

**Blacklist (prevent loading):**

```
blacklist module_name
```

---

## System V Service Management

**List services:**

```bash
service --status-all
```

**Start service:**

```bash
sudo service networking start
```

**Stop service:**

```bash
sudo service networking stop
```

**Restart service:**

```bash
sudo service networking restart
```

---

## Upstart Job Management

**List jobs:**

```bash
initctl list
```

**View job status:**

```bash
initctl status networking
```

**Control jobs:**

```bash
sudo initctl start networking
sudo initctl stop networking
sudo initctl restart networking
```

**Emit event:**

```bash
sudo initctl emit some_event
```

---

## Systemd Service Management

**List units:**

```bash
systemctl list-units
```

**View service status:**

```bash
systemctl status networking.service
```

**Control services:**

```bash
sudo systemctl start networking.service
sudo systemctl stop networking.service
sudo systemctl restart networking.service
```

**Enable/disable at boot:**

```bash
sudo systemctl enable networking.service
sudo systemctl disable networking.service
```

---

## Power States

**Shutdown:**

```bash
sudo shutdown -h now
```

**Reboot:**

```bash
sudo shutdown -r now
```

or simply:

```bash
sudo reboot
```

---

## System Monitoring with top

**Understanding top output:**

### Line 1: Uptime information

- Current time
- System uptime
- Number of users
- Load average (1, 5, 15 minutes)

### Line 2: Task summary

- Running, sleeping, stopped, zombie processes

### Line 3: CPU usage

- `us`: User processes
- `sy`: System (kernel)
- `ni`: Nice (low priority)
- `id`: Idle
- `wa`: I/O wait
- `hi`: Hardware interrupts
- `si`: Software interrupts
- `st`: Steal time (virtual machines)

### Lines 4-5: Memory

- Physical RAM usage
- Swap usage

### Process list columns:

- PID: Process ID
- USER: Owner
- PR: Priority
- NI: Nice value
- VIRT: Virtual memory
- RES: Physical memory
- SHR: Shared memory
- S: Status (S=sleep, R=run, Z=zombie)
- %CPU: CPU usage
- %MEM: Memory usage
- TIME+: Total CPU time
- COMMAND: Process name

**Monitor specific process:**

```bash
top -p 1234
```

---

## lsof and fuser - Open Files

**lsof (list open files):**

```bash
lsof .
```

Shows all processes using files in current directory.

**fuser:**

```bash
fuser -v .
```

Shows which processes are using a file or directory.

---

## Process Threads

**View threads:**

```bash
ps m
```

**Threads:** Lightweight processes that share memory space with their parent process.

---

## CPU Monitoring

**View load average:**

```bash
uptime
```

Shows: current time, uptime, users, and load averages.

**Load average meaning:**

- Under 1.0: System not busy
- Equal to CPU count: Fully utilized
- Above CPU count: Processes waiting

---

## I/O Monitoring

**iostat:**

```bash
iostat
```

**CPU section:**

- %user: User-level CPU usage
- %nice: Nice processes
- %system: Kernel CPU usage
- %iowait: Waiting for I/O
- %steal: Virtual machine steal time
- %idle: Idle time

**Disk section:**

- tps: Transfers per second
- kB_read/s: KB read per second
- kB_wrtn/s: KB written per second

---

## Memory Monitoring

**vmstat:**

```bash
vmstat
```

**Sections:**

- **procs:** r=run queue, b=blocked
- **memory:** swpd, free, buff, cache
- **swap:** si=in, so=out
- **io:** bi=blocks in, bo=blocks out
- **system:** in=interrupts, cs=context switches
- **cpu:** us, sy, id, wa

---

## Continuous Monitoring with sar

**Install sar:**

```bash
sudo apt install sysstat
```

**View CPU queue:**

```bash
sudo sar -q
```

**View memory:**

```bash
sudo sar -r
```

**View specific CPU:**

```bash
sudo sar -P
```

**View historical data:**

```bash
sar -q /var/log/sysstat/sa02
```

---

## Cron Jobs - Task Scheduling

**Cron format:**

```
minute hour day month day_of_week command
```

**Example:**

```
30 08 * * * /home/pete/scripts/change_wallpaper
```

Runs at 8:30 AM every day.

**Field ranges:**

- Minute: 0-59
- Hour: 0-23
- Day of month: 1-31
- Month: 1-12
- Day of week: 0-7 (0 and 7 = Sunday)

**Special characters:**

- `*`: Any value
- `,`: List (1,3,5)
- `-`: Range (1-5)
- `/`: Step (\*/5 = every 5)

**Edit crontab:**

```bash
crontab -e
```

---

## System Logging

### syslog

**Send log message:**

```bash
logger -s Hello
```

---

### Important log files:

**/var/log/messages:**
General system messages (Red Hat based)

**/var/log/syslog:**
General system log (Debian based)

**/var/log/dmesg:**
Kernel messages and boot log

**/var/log/kern.log:**
Kernel log

**/var/log/auth.log:**
Authentication attempts (login, sudo)

---

# Part 8: Networking

## File Sharing with scp

**Secure Copy Protocol (scp)** - Transfers files over SSH.

**Copy to remote:**

```bash
scp myfile.txt username@remotehost.com:/remote/directory
```

**Copy from remote:**

```bash
scp username@remotehost.com:/remote/file.txt /local/directory
```

**Copy directory:**

```bash
scp -r mydir username@remotehost.com:/remote/directory
```

---

## rsync - Efficient File Sync

**What it does:** Synchronizes files, only transferring differences.

**Common options:**

- `-v`: Verbose
- `-r`: Recursive
- `-h`: Human-readable
- `-z`: Compressed transfer

**Local sync:**

```bash
rsync -zvr /source/ /destination/
```

**To remote:**

```bash
rsync -zvr /local/ username@remote:/path/
```

**From remote:**

```bash
rsync -zvr username@remote:/path/ /local/
```

---

## Simple HTTP Server

**Start quick web server:**

**Python 3:**

```bash
python -m http.server
```

**Python 2:**

```bash
python -m SimpleHTTPServer
```

Serves current directory on port 8000.

---

## NFS - Network File System

**Client setup:**

**Start NFS client:**

```bash
sudo service nfsclient start
```

**Mount NFS share:**

```bash
sudo mount server:/directory /mount_directory
```

---

## Samba - Windows File Sharing

### Setup Samba server:

**Install:**

```bash
sudo apt update
sudo apt install samba
```

**Configure:**

```bash
sudo vi /etc/samba/smb.conf
```

**Set password:**

```bash
sudo smbpasswd -a username
```

**Create share directory:**

```bash
mkdir /my/share
```

**Restart Samba:**

```bash
sudo service smbd restart
```

### Access Samba share:

**From Linux:**

```bash
smbclient //HOST/directory -U user
```

**Mount permanently:**

```bash
sudo mount -t cifs //server/share /mountpoint -o user=username,pass=password
```

---

## Network Basics

### Network components:

- **ISP:** Internet Service Provider
- **Router:** Connects your network to internet
- **WAN:** Wide Area Network (internet)
- **WLAN:** Wireless Local Network
- **LAN:** Wired Local Network
- **Hosts:** Devices on network

---

## Network Models

### TCP/IP Layers:

**Application Layer:**

- HTTP: Web browsing
- SMTP: Email
- FTP: File transfer
- DNS: Name resolution

**Transport Layer:**

- TCP: Reliable delivery
- UDP: Fast, unreliable delivery

**Network Layer:**

- IP: Routing between networks
- ICMP: Error messages

**Link Layer:**

- Ethernet
- WiFi
- ARP: Address resolution

---

## Network Addressing

### MAC Address:

Physical hardware address (48-bit)
Example: 00:1A:2B:3C:4D:5E

### IP Address:

Logical network address

- IPv4: 192.168.1.100
- IPv6: 2001:0db8:85a3::8a2e:0370:7334

### Hostname:

Human-friendly name
Example: mycomputer.local

---

## Ports

**What they are:** Endpoints for network connections (0-65535)

**Common ports:**

- 20/21: FTP
- 22: SSH
- 23: Telnet
- 25: SMTP
- 53: DNS
- 80: HTTP
- 443: HTTPS
- 3306: MySQL
- 5432: PostgreSQL

---

## DHCP - Dynamic Host Configuration

**How DHCP works:**

1. **DISCOVER:** Client broadcasts request
2. **OFFER:** Server offers configuration
3. **REQUEST:** Client accepts offer
4. **ACK:** Server acknowledges

---

## IPv4 Addressing

**Format:** Four octets (0-255)
Example: 192.168.1.100

**Classes:**

- Class A: 1.0.0.0 to 126.0.0.0
- Class B: 128.0.0.0 to 191.255.0.0
- Class C: 192.0.0.0 to 223.255.255.0

**Private ranges:**

- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16

---

## Subnets and Subnet Masks

**Subnet mask:** Defines network and host portions

**Common masks:**

- 255.255.255.0 (/24): 254 hosts
- 255.255.0.0 (/16): 65,534 hosts
- 255.255.255.128 (/25): 126 hosts

**CIDR notation:**
192.168.1.0/24 = 192.168.1.0 with mask 255.255.255.0

---

## Binary Conversion Cheat Sheet

**Powers of 2:**

- 2^1 = 2
- 2^2 = 4
- 2^3 = 8
- 2^4 = 16
- 2^5 = 32
- 2^6 = 64
- 2^7 = 128
- 2^8 = 256

**Decimal to binary:**
128 | 64 | 32 | 16 | 8 | 4 | 2 | 1

Example: 192 = 128 + 64 = 11000000

---

## NAT - Network Address Translation

**What it does:** Allows multiple devices to share one public IP address.

**Benefits:**

- Conserves IP addresses
- Adds security layer
- Hides internal network structure

---

## IPv6

**Format:** Eight groups of four hexadecimal digits
Example: 2001:0db8:85a3:0000:0000:8a2e:0370:7334

**Advantages:**

- Virtually unlimited addresses
- Built-in security (IPSec)
- No need for NAT
- Simplified header format

---

## Routing

**What it is:** Determining the path packets take through networks.

**View routing table:**

```bash
route -n
```

or

```bash
ip route show
```

**Routing table columns:**

- **Destination:** Target network
- **Gateway:** Next hop router
- **Genmask:** Netmask
- **Flags:** U=up, G=gateway
- **Iface:** Network interface

---

## Network Interfaces

### ifconfig (traditional):

**View interfaces:**

```bash
ifconfig
```

**Configure interface:**

```bash
ifconfig eth0 192.168.2.1 netmask 255.255.255.0 up
```

**Bring interface up/down:**

```bash
ifup eth0
ifdown eth0
```

---

### ip (modern):

**Show interfaces:**

```bash
ip link show
```

**Show statistics:**

```bash
ip -s link show eth0
```

**Show IP addresses:**

```bash
ip address show
```

**Configure interface:**

```bash
ip link set eth0 up
ip link set eth0 down
ip address add 192.168.1.1/24 dev eth0
```

---

## route - Manage Routes

**Add route:**

```bash
sudo route add -net 192.168.2.0/24 gw 10.11.12.3
```

**Delete route:**

```bash
sudo route del -net 192.168.2.0/24
```

**With ip command:**

```bash
ip route add 192.168.2.0/24 via 10.11.12.3
ip route delete 192.168.2.0/24
```

---

## dhclient - DHCP Client

**Request new IP:**

```bash
sudo dhclient
```

**Release IP:**

```bash
sudo dhclient -r
```

---

## arp - Address Resolution Protocol

**View ARP cache:**

```bash
arp
```

**Modern alternative:**

```bash
ip neighbour show
```

**What ARP does:** Maps IP addresses to MAC addresses on local networks.

---

## ICMP - Internet Control Message Protocol

**What it is:** Protocol for error messages and diagnostics.

### Common ICMP Types:

- **Type 0:** Echo Reply (ping response)
- **Type 3:** Destination Unreachable
  - Code 0: Network Unreachable
  - Code 1: Host Unreachable
- **Type 8:** Echo Request (ping)
- **Type 11:** Time Exceeded (traceroute)

---

## ping - Test Connectivity

**What it does:** Sends ICMP echo requests to test if a host is reachable.

**Basic usage:**

```bash
ping www.google.com
```

**Limited pings:**

```bash
ping -c 3 www.google.com
```

**Understanding output:**

```
64 bytes from 172.217.14.196: icmp_seq=1 ttl=117 time=11.4 ms
```

- **icmp_seq:** Packet sequence number
- **ttl:** Time To Live (hops remaining)
- **time:** Round-trip time

**What it tells you:**

- Host is reachable: You get replies
- Network issues: Packet loss percentage
- Latency: Time in milliseconds

---

## traceroute - Trace Network Path

**What it does:** Shows the route packets take to reach a destination.

**Usage:**

```bash
traceroute google.com
```

**Output shows:**

- Each hop (router) along the path
- Round-trip times for each hop
- Where delays or failures occur

**Why it's useful:** Helps diagnose where network problems occur.

---

## netstat - Network Statistics

**What it does:** Displays network connections, routing tables, and interface statistics.

**Show all TCP connections:**

```bash
netstat -at
```

**Show all UDP connections:**

```bash
netstat -au
```

**Show listening ports:**

```bash
netstat -l
```

**Show with process info:**

```bash
netstat -tulpn
```

### Understanding output:

```
Proto Recv-Q Send-Q Local Address    Foreign Address    State
tcp   0      0      0.0.0.0:22       0.0.0.0:*          LISTEN
tcp   0      0      192.168.1.5:443  74.125.224.72:443  ESTABLISHED
```

**Columns:**

- **Proto:** Protocol (TCP or UDP)
- **Recv-Q:** Data queued to receive
- **Send-Q:** Data queued to send
- **Local Address:** Your machine's address and port
- **Foreign Address:** Remote machine's address and port
- **State:** Connection state

**Common states:**

- **LISTEN:** Waiting for incoming connections
- **ESTABLISHED:** Active connection
- **SYN_SENT:** Attempting to establish connection
- **CLOSE_WAIT:** Remote end has closed connection
- **TIME_WAIT:** Waiting after close to handle late packets

---

## Packet Analysis with tcpdump

**What it does:** Captures and analyzes network packets.

**Install:**

```bash
sudo apt install tcpdump
```

**Capture on interface:**

```bash
sudo tcpdump -i wlan0
```

**Capture and save to file:**

```bash
sudo tcpdump -w capture.pcap
```

**Read from file:**

```bash
tcpdump -r capture.pcap
```

**Filter by host:**

```bash
sudo tcpdump host 192.168.1.1
```

**Filter by port:**

```bash
sudo tcpdump port 80
```

**Understanding output:**

```
10:30:45.123456 IP 192.168.1.5.52134 > 172.217.14.196.80: Flags [S]
```

- **Timestamp:** 10:30:45.123456
- **Source:** 192.168.1.5 port 52134
- **Destination:** 172.217.14.196 port 80
- **Flags:** [S] = SYN (connection start)

**Common flags:**

- S: SYN (synchronize)
- A: ACK (acknowledge)
- F: FIN (finish)
- P: PSH (push)
- R: RST (reset)

---

## DNS - Domain Name System

**What it does:** Translates domain names to IP addresses.

### DNS Components:

**Name Server:** Responds to DNS queries

**Zone File:** Contains DNS records for a domain

**Resource Records:** Individual DNS entries

### DNS Process:

1. **Local DNS Server:** First check (usually your ISP or router)
2. **Root Servers:** Top-level DNS servers (. root zone)
3. **TLD Servers:** Top-Level Domain servers (.com, .org, etc.)
4. **Authoritative Server:** Has the actual IP address

**Example query for www.google.com:**

1. Query local DNS server
2. If not cached, query root server
3. Root points to .com TLD server
4. TLD points to google.com authoritative server
5. Authoritative server returns IP address

---

## /etc/hosts - Local Name Resolution

**What it is:** Local file for hostname-to-IP mapping.

**View it:**

```bash
cat /etc/hosts
```

**Example content:**

```
127.0.0.1    localhost
192.168.1.100    myserver.local
```

**Why use it:**

- Override DNS lookups
- Add custom hostnames
- Faster than DNS queries
- Works when DNS is unavailable

---

## /etc/resolv.conf - DNS Configuration

**What it is:** Specifies which DNS servers to use.

**View it:**

```bash
cat /etc/resolv.conf
```

**Example content:**

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

These are Google's public DNS servers.

---

## DNS Setup Options

**Popular DNS servers:**

- **BIND:** Most widely used, full-featured
- **DNSmasq:** Lightweight, easy to configure
- **PowerDNS:** Modern, high-performance

---

## DNS Tools

### nslookup - Query DNS

**Basic lookup:**

```bash
nslookup www.google.com
```

**Specify DNS server:**

```bash
nslookup www.google.com 8.8.8.8
```

**Output shows:**

- Server used for lookup
- IP address(es) returned

---

### dig - Detailed DNS Information

**Basic query:**

```bash
dig www.google.com
```

**Query specific record type:**

```bash
dig www.google.com A
dig www.google.com MX
dig www.google.com NS
```

**Common record types:**

- **A:** IPv4 address
- **AAAA:** IPv6 address
- **MX:** Mail server
- **NS:** Name server
- **CNAME:** Canonical name (alias)
- **TXT:** Text records
- **SOA:** Start of Authority

**dig output sections:**

- **QUESTION:** What was asked
- **ANSWER:** The response
- **AUTHORITY:** Authoritative name servers
- **ADDITIONAL:** Extra helpful information

---

## Network Troubleshooting Workflow

**When network problems occur, follow this systematic approach:**

### 1. Check Local Connectivity

```bash
ip link show
```

Verify interface is up.

### 2. Check IP Configuration

```bash
ip address show
```

Ensure you have an IP address.

### 3. Check Default Gateway

```bash
ip route show
```

Verify routing is configured.

### 4. Test Local Network

```bash
ping 192.168.1.1
```

Test connectivity to gateway.

### 5. Test DNS Resolution

```bash
nslookup google.com
```

Check if DNS is working.

### 6. Test External Connectivity

```bash
ping 8.8.8.8
```

Test internet connectivity by IP.

### 7. Test Domain Resolution

```bash
ping google.com
```

Test complete network path with DNS.

### 8. Trace the Route

```bash
traceroute google.com
```

Identify where packets are failing.

### 9. Check for Listening Services

```bash
netstat -tulpn
```

Verify services are running on expected ports.

### 10. Analyze Packets

```bash
sudo tcpdump -i eth0
```

Look at actual network traffic if needed.

---

## Network Security Best Practices

**1. Use SSH instead of Telnet**

- SSH encrypts all communication
- Never use Telnet (port 23) for remote access

**2. Keep Systems Updated**

```bash
sudo apt update && sudo apt upgrade
```

**3. Use Firewalls**

- Configure iptables or firewalld
- Only open necessary ports

**4. Disable Unused Services**

```bash
sudo systemctl stop service_name
sudo systemctl disable service_name
```

**5. Monitor Logs Regularly**

```bash
tail -f /var/log/auth.log
```

**6. Use Strong Authentication**

- SSH keys instead of passwords
- Disable root login over SSH

**7. Regular Backups**

- Use rsync for automated backups
- Test restore procedures

---

## Common Network Problems and Solutions

### Problem: "Network is unreachable"

**Possible causes:**

- No IP address assigned
- Wrong subnet mask
- No default gateway
- Interface is down

**Solutions:**

```bash
# Check interface status
ip link show

# Bring interface up
sudo ip link set eth0 up

# Get new IP from DHCP
sudo dhclient eth0

# Check routing
ip route show
```

---

### Problem: "DNS resolution fails"

**Symptoms:**

- Can ping by IP but not by hostname
- "Name or service not known" errors

**Solutions:**

```bash
# Check DNS servers
cat /etc/resolv.conf

# Test DNS manually
nslookup google.com

# Add public DNS servers
echo "nameserver 8.8.8.8" | sudo tee -a /etc/resolv.conf
```

---

### Problem: "Connection timeout"

**Possible causes:**

- Firewall blocking
- Service not running
- Wrong port number
- Network congestion

**Solutions:**

```bash
# Check if service is listening
netstat -tulpn | grep :80

# Test connectivity
telnet hostname port

# Check firewall rules
sudo iptables -L
```

---

### Problem: "Slow network performance"

**Diagnostics:**

```bash
# Check interface errors
ip -s link show eth0

# Monitor bandwidth usage
iftop

# Check for packet loss
ping -c 100 gateway

# Look for routing issues
traceroute destination
```

---

## Advanced Networking Commands

### ss - Socket Statistics (modern netstat)

**Show all connections:**

```bash
ss -tuln
```

**Show process info:**

```bash
ss -tulpn
```

**Faster than netstat** and more detailed.

---

### nc (netcat) - Network Swiss Army Knife

**Test port connectivity:**

```bash
nc -zv hostname port
```

**Create simple server:**

```bash
nc -l 1234
```

**Connect as client:**

```bash
nc hostname 1234
```

---

### curl - Transfer Data

**Download file:**

```bash
curl -O https://example.com/file.zip
```

**Test HTTP endpoint:**

```bash
curl https://api.example.com/endpoint
```

**POST data:**

```bash
curl -X POST -d "data" https://example.com/api
```

---

### wget - Download Files

**Download file:**

```bash
wget https://example.com/file.zip
```

**Resume interrupted download:**

```bash
wget -c https://example.com/largefile.iso
```

**Download recursively:**

```bash
wget -r https://example.com/directory/
```

---

## Conclusion

### Your Linux Journey

Congratulations on completing this comprehensive guide! You've learned:

✅ **File system navigation** and management
✅ **Text editors** (Vim and Emacs basics)
✅ **User and permission management**
✅ **Process control** and monitoring
✅ **System administration** essentials
✅ **Package management**
✅ **Networking fundamentals**
✅ **Troubleshooting techniques**

---

### Next Steps

**1. Practice Regularly**

- Set up a virtual machine
- Try commands daily
- Build muscle memory

**2. Create Projects**

- Automate repetitive tasks with scripts
- Set up a web server
- Configure network services

**3. Read Man Pages**

- Dive deeper into commands
- Learn advanced options
- Understand all capabilities

**4. Join Communities**

- Linux forums and subreddits
- Stack Overflow
- Local user groups

**5. Keep Learning**

- Shell scripting (bash)
- Advanced networking
- Security and hardening
- Cloud platforms (AWS, Azure, GCP)

---

### Essential Resources

**Documentation:**

- Man pages: `man command`
- Info pages: `info command`
- /usr/share/doc directory

**Online Resources:**

- Linux Documentation Project
- Arch Wiki (excellent even for non-Arch users)
- DigitalOcean tutorials
- LinuxCommand.org

**Books:**

- "The Linux Command Line" by William Shotts
- "UNIX and Linux System Administration Handbook"
- "How Linux Works" by Brian Ward

---

### Final Thoughts

Linux mastery is a journey, not a destination. Every system administrator, developer, and IT professional continues learning throughout their career. The command line is powerful precisely because it's so flexible and capable.

**Remember:**

- Don't memorize—understand concepts
- Make mistakes in safe environments
- Read error messages carefully
- Use `--help` and `man` pages liberally
- The community is here to help

**Most importantly:** Don't be intimidated. Every expert was once a beginner. With consistent practice, these commands will become second nature.

---

### Quick Reference Summary

**Navigation:**

```bash
pwd              # Show current directory
cd directory     # Change directory
ls -la           # List files with details
```

**Files:**

```bash
touch file       # Create file
cat file         # View file
cp src dest      # Copy
mv src dest      # Move/rename
rm file          # Delete
```

**Search:**

```bash
find /path -name "file"    # Find files
grep "pattern" file        # Search in files
```

**Permissions:**

```bash
chmod 755 file   # Change permissions
chown user file  # Change owner
```

**Processes:**

```bash
ps aux           # List processes
top              # Monitor processes
kill PID         # Terminate process
```

**System:**

```bash
sudo command     # Run as superuser
apt install pkg  # Install package (Debian)
systemctl start service  # Start service
```

**Network:**

```bash
ip addr show     # Show IP addresses
ping host        # Test connectivity
netstat -tulpn   # Show connections
```

---

### Practice Exercises

**Beginner:**

1. Navigate to your home directory
2. Create a directory called "practice"
3. Create 5 text files in it
4. List all files with detailed information
5. Delete one file
6. Rename a file

**Intermediate:**

1. Find all .txt files in your home directory
2. Create a compressed archive of a directory
3. Change permissions on a file to 644
4. Create an alias for a long command
5. Use grep to search for text in multiple files

**Advanced:**

1. Set up a cron job
2. Monitor system resources with top
3. Configure a new user with specific permissions
4. Set up SSH key authentication
5. Analyze network traffic with tcpdump

---

## About This Guide

This guide was created to provide a comprehensive, beginner-friendly introduction to Linux command-line operations. It covers the essential commands and concepts needed to navigate, manage, and troubleshoot Linux systems effectively.

Whether you're studying for a career in IT, developing software, managing servers, or simply exploring Linux out of curiosity, mastering these commands will serve as a solid foundation for all your future work with Linux systems.

---

**Happy Learning!**

_"The only way to learn a new programming language is by writing programs in it."_
_— Dennis Ritchie_

The same applies to Linux—the only way to master the command line is by using it.

---

**Version:** 1.0  
**Last Updated:** October 2025  
**Author:** Ben-David (BDofTech)

**License:** This guide is provided for educational purposes. Feel free to share, print, and use it for learning.

---

## Index

**A**

- alias, apt, arp

**B**

- BIOS, Boot Process

**C**

- cat, cd, chmod, chown, cp, cron, curl

**D**

- dd, df, DHCP, dig, DNS, du

**E**

- Emacs, Environment Variables

**F**

- File Permissions, Filesystems, find, fsck

**G**

- grep, gzip

**H**

- history, hosts file

**I**

- ifconfig, Inodes, iostat, ip, IPv4, IPv6

**J**

- Jobs, join

**K**

- Kernel, kill

**L**

- less, ln, lsof, lsusb

**M**

- man, mkdir, mount, mv

**N**

- NAT, netstat, Network, NFS, nslookup

**P**

- Package Management, passwd, Permissions, ping, Processes, ps

**R**

- Regex, rm, root, route, rsync

**S**

- Samba, scp, Signals, ssh, sudo, Subnets, swap, Systemd

**T**

- tail, tar, tcpdump, top, touch, traceroute

**U**

- umask, Users, UEFI

**V**

- Vim, vmstat

**W**

- wc, wget

**Y**

- yum

---

**END OF GUIDE**

---

_Thank you for reading! May your terminals always return exit code 0._

```
$ echo "Happy Linux Learning!"
Happy Linux Learning!
$ _
```
