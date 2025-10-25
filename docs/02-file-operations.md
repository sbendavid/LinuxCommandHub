# Part 2: File Operations

[← Back to Part 1](./01-getting-started.md) | [Main Guide](../README.md) | [Next: Help & Shortcuts →](./03-help-shortcuts.md)

---

## Table of Contents

- [touch - Creating Files](#touch---creating-files)
- [file - Identifying File Types](#file---identifying-file-types)
- [cat - Reading Files](#cat---reading-files)
- [less - Viewing Files Page by Page](#less---viewing-files-page-by-page)
- [history - Command History](#history---command-history)
- [cp - Copying Files](#cp---copying-files)
- [mv - Moving and Renaming](#mv---moving-and-renaming)
- [mkdir - Making Directories](#mkdir---making-directories)
- [rm - Removing Files](#rm---removing-files)
- [find - Searching for Files](#find---searching-for-files)

---

## touch - Creating Files

**What it does:** Creates an empty file or updates the timestamp of an existing file.

**Syntax:**

```bash
touch filename
```

### Basic Usage

**Create a single file:**

```bash
touch mysuperduperfile
```

**Create multiple files at once:**

```bash
touch file1.txt file2.txt file3.txt
```

**Verify creation:**

```bash
$ touch myfile.txt
$ ls -l myfile.txt
-rw-r--r-- 1 pete users 0 Oct 24 10:45 myfile.txt
```

Notice the size is 0—it's completely empty!

---

### ⚠️ WARNING: No Recycle Bin!

**There is NO undo for `rm` in Linux!** Once you delete something, it's gone forever. Be extremely careful with this command.

---

### Basic Usage

**Delete a single file:**

```bash
rm file1.txt
```

**Delete multiple files:**

```bash
rm file1.txt file2.txt file3.txt
```

**Delete with wildcard:**

```bash
rm *.txt  # Deletes ALL .txt files in current directory
```

---

### Options

**Force deletion (-f):**

```bash
rm -f file.txt
```

Doesn't ask for confirmation, even for protected files.

**Interactive mode (-i):**

```bash
$ rm -i file.txt
rm: remove regular file 'file.txt'?
```

Asks before deleting each file. **Much safer!**

**Verbose mode (-v):**

```bash
$ rm -v file.txt
removed 'file.txt'
```

---

### Removing Directories

**Remove an empty directory:**

```bash
rmdir EmptyFolder
```

**Remove a directory and all its contents:**

```bash
rm -r MyFolder
```

The `-r` (recursive) flag deletes the directory and everything inside it.

**Remove without prompting:**

```bash
rm -rf MyFolder
```

**⚠️ DANGER:** The `-rf` combination is extremely powerful and dangerous! Double-check before using it.

---

### Safety Practices

**1. Use interactive mode:**

```bash
rm -i *.txt
```

**2. List files first:**

```bash
ls *.txt    # See what will be deleted
rm *.txt    # Then delete
```

**3. Use echo to preview:**

```bash
echo rm *.txt  # Shows the command without executing
```

**4. Make an alias:**
Add to your `~/.bashrc`:

```bash
alias rm='rm -i'
```

Now `rm` always asks for confirmation!

---

### Horror Stories to Learn From

**Don't do this:**

```bash
rm -rf /  # Tries to delete EVERYTHING on your system!
```

Modern systems protect against this, but still—never run it!

**Be careful with spaces:**

```bash
rm -rf MyFolder /  # WRONG! Space before / is catastrophic
rm -rf MyFolder/   # Correct
```

**Watch your wildcards:**

```bash
rm * .txt  # WRONG! Deletes everything, then tries to delete .txt
rm *.txt   # Correct
```

---

### Safer Alternatives

**Use trash-cli instead:**

```bash
sudo apt install trash-cli
trash myfile.txt  # Moves to trash instead of deleting
```

**Or move to a trash directory:**

```bash
mkdir -p ~/.trash
mv unwanted_file.txt ~/.trash/
```

---

## find - Searching for Files

**What it does:** Searches for files and directories based on various criteria.

**Syntax:**

```bash
find [path] [options] [expression]
```

---

### Basic Usage

**Find by name:**

```bash
find /home -name "puppies.jpg"
```

Searches in `/home` for a file named exactly "puppies.jpg".

**Case-insensitive name search:**

```bash
find /home -iname "puppies.jpg"
```

Matches puppies.jpg, Puppies.JPG, PUPPIES.jpg, etc.

---

### Finding by Type

**Find only directories:**

```bash
find /home -type d -name "MyFolder"
```

**Find only files:**

```bash
find /home -type f -name "*.txt"
```

**Type options:**

- `f` - regular file
- `d` - directory
- `l` - symbolic link
- `b` - block device
- `c` - character device

---

### Finding by Size

**Find files larger than 100MB:**

```bash
find /home -size +100M
```

**Find files smaller than 10KB:**

```bash
find /home -size -10k
```

**Find files exactly 50MB:**

```bash
find /home -size 50M
```

**Size units:**

- `b` - 512-byte blocks (default)
- `c` - bytes
- `k` - kilobytes
- `M` - megabytes
- `G` - gigabytes

---

### Finding by Time

**Find files modified in the last 7 days:**

```bash
find /home -mtime -7
```

**Find files modified more than 30 days ago:**

```bash
find /home -mtime +30
```

**Find files accessed today:**

```bash
find /home -atime 0
```

**Time types:**

- `mtime` - modification time
- `atime` - access time
- `ctime` - change time (metadata)

---

### Finding by Permissions

**Find files with 777 permissions:**

```bash
find /home -perm 777
```

**Find files with setuid bit:**

```bash
find /home -perm -4000
```

---

### Combining Criteria

**Find large log files:**

```bash
find /var/log -type f -name "*.log" -size +10M
```

**Find old temporary files:**

```bash
find /tmp -type f -mtime +7
```

**Find files by owner:**

```bash
find /home -user pete -name "*.txt"
```

---

### Executing Commands on Results

**Delete all found files:**

```bash
find /tmp -name "*.tmp" -delete
```

**Or use -exec:**

```bash
find /tmp -name "*.tmp" -exec rm {} \;
```

**Copy all found files:**

```bash
find . -name "*.jpg" -exec cp {} /backup/ \;
```

**The {} is replaced with each found file!**

---

### Practical Examples

**Example 1: Find and list large files**

```bash
find /home -type f -size +100M -exec ls -lh {} \;
```

**Example 2: Find empty files**

```bash
find /home -type f -empty
```

**Example 3: Find by multiple extensions**

```bash
find . -type f \( -name "*.jpg" -o -name "*.png" \)
```

The `-o` means "or".

**Example 4: Find and count**

```bash
find /home -name "*.txt" | wc -l
```

Counts how many .txt files exist.

---

### Common Use Cases

**Use Case 1: Clean up old logs**

```bash
find /var/log -name "*.log" -mtime +90 -delete
```

**Use Case 2: Find duplicate file names**

```bash
find . -name "filename.txt"
```

**Use Case 3: Find files by multiple users**

```bash
find /home \( -user pete -o -user john \) -name "*.doc"
```

**Use Case 4: Find recently modified config files**

```bash
find /etc -name "*.conf" -mtime -1
```

---

### Performance Tips

**1. Limit search depth:**

```bash
find /home -maxdepth 3 -name "file.txt"
```

**2. Start from specific directory:**

```bash
find ~/Documents -name "*.pdf"  # Faster than searching from /
```

**3. Exclude directories:**

```bash
find /home -path "*/node_modules" -prune -o -name "*.js" -print
```

---

## Summary

In this part, you learned:

✅ **touch** - Create files and update timestamps  
✅ **file** - Identify file types  
✅ **cat** - Display file contents  
✅ **less** - View files page by page  
✅ **history** - View command history  
✅ **cp** - Copy files and directories  
✅ **mv** - Move and rename files  
✅ **mkdir** - Create directories  
✅ **rm** - Delete files (carefully!)  
✅ **find** - Search for files with powerful criteria

---

## Quick Reference

```bash
# Creating
touch file.txt              # Create empty file
mkdir folder                # Create directory
mkdir -p path/to/folder     # Create nested directories

# Viewing
cat file.txt                # Display file
less file.txt               # View file page by page
file myfile                 # Check file type

# Copying
cp file.txt backup.txt      # Copy file
cp -r folder/ backup/       # Copy directory
cp *.txt /backup/           # Copy multiple files

# Moving/Renaming
mv old.txt new.txt          # Rename file
mv file.txt /backup/        # Move file
mv *.txt /backup/           # Move multiple files

# Deleting
rm file.txt                 # Delete file
rm -i file.txt              # Delete with confirmation
rm -r folder/               # Delete directory
rmdir empty_folder/         # Delete empty directory

# Searching
find . -name "*.txt"        # Find by name
find . -type f -size +10M   # Find large files
find . -mtime -7            # Find recent files
```

---

## Practice Exercises

**Exercise 1: Basic File Operations**

```bash
# 1. Create a directory called "practice"
# 2. Create three text files inside it
# 3. Copy one file and rename the copy
# 4. Move one file to your home directory
# 5. Delete one file
# 6. List all files with details
```

**Exercise 2: Wildcards**

```bash
# 1. Create files: test1.txt, test2.txt, test3.txt, data1.csv, data2.csv
# 2. Copy all .txt files to a backup folder
# 3. Move all .csv files to a data folder
# 4. List all files starting with "test"
```

**Exercise 3: Finding Files**

```bash
# 1. Find all .txt files in your home directory
# 2. Find files larger than 1MB
# 3. Find files modified in the last 24 hours
# 4. Find empty files
# 5. Count how many .jpg files you have
```

**Exercise 4: Directory Management**

```bash
# 1. Create this structure: project/src/main/java
# 2. Create several files in different levels
# 3. Copy the entire project directory
# 4. Find all files in the project
# 5. Delete the backup copy
```

---

## Common Mistakes to Avoid

**Mistake 1: Forgetting -r for directories**

```bash
cp MyFolder /backup/        # Wrong! Error message
cp -r MyFolder /backup/     # Correct
```

**Mistake 2: Overwriting without checking**

```bash
mv important.txt backup.txt  # Overwrites backup.txt without warning!
mv -i important.txt backup.txt  # Asks first
```

**Mistake 3: Using rm -rf carelessly**

```bash
rm -rf /important/data/     # No confirmation, no recovery!
rm -ri /important/data/     # Much safer
```

**Mistake 4: Wildcards without testing**

```bash
# Test first:
ls *.txt
# Then delete:
rm *.txt
```

---

## Troubleshooting

**Problem: "Permission denied"**

```bash
$ rm file.txt
rm: cannot remove 'file.txt': Permission denied
```

**Solution:** You don't own the file, or it's write-protected. Use `sudo` if appropriate, or check ownership with `ls -l`.

**Problem: "Directory not empty"**

```bash
$ rmdir MyFolder
rmdir: failed to remove 'MyFolder': Directory not empty
```

**Solution:** Use `rm -r MyFolder` instead of `rmdir`.

**Problem: "No such file or directory"**

```bash
$ cp file.txt /backup/
cp: cannot create regular file '/backup/': No such file or directory
```

**Solution:** The `/backup/` directory doesn't exist. Create it first with `mkdir /backup/`.

---

## Next Steps

Excellent work! You now know how to manage files and directories effectively.

**Ready to continue?** Head to [Part 3: Getting Help and Shortcuts](./03-help-shortcuts.md) to learn how to get help and create your own command shortcuts.

**Need more practice?** Try the exercises above before moving on!

---

[← Back to Part 1](./01-getting-started.md) | [Main Guide](../README.md) | [Next: Help & Shortcuts →](./03-help-shortcuts.md) Understanding Timestamps

Every file has three timestamps:

- **Access time** - Last time file was read
- **Modify time** - Last time content was changed
- **Change time** - Last time metadata was changed

**View timestamps:**

```bash
stat myfile.txt
```

**What `touch` actually does:**

```bash
$ ls -l myfile.txt
-rw-r--r-- 1 pete users 0 Oct 24 10:45 myfile.txt

$ touch myfile.txt

$ ls -l myfile.txt
-rw-r--r-- 1 pete users 0 Oct 24 10:47 myfile.txt
```

The timestamp updated to the current time!

---

### Real-World Uses

**1. Creating placeholder files:**

```bash
touch README.md LICENSE .gitignore
```

**2. Updating timestamps for build systems:**
Some build tools check file modification times to decide what needs rebuilding.

**3. Creating test files:**

```bash
touch test1.txt test2.txt test3.txt
```

---

## file - Identifying File Types

**What it does:** Determines the type of a file by examining its content.

**Syntax:**

```bash
file filename
```

### Why It Matters

Unlike Windows, Linux doesn't rely solely on file extensions. A file named `document.txt` could actually be an image! The `file` command examines the actual content to tell you the truth.

---

### Examples

**Image file:**

```bash
$ file banana.jpg
banana.jpg: JPEG image data, JFIF standard 1.01
```

**Text file:**

```bash
$ file script.sh
script.sh: Bourne-Again shell script, ASCII text executable
```

**Binary executable:**

```bash
$ file /bin/ls
/bin/ls: ELF 64-bit LSB executable, x86-64
```

**Empty file:**

```bash
$ touch empty.txt
$ file empty.txt
empty.txt: empty
```

**Directory:**

```bash
$ file Documents/
Documents/: directory
```

---

### Practical Scenarios

**Scenario 1: Suspicious download**

```bash
$ file suspicious_document.pdf
suspicious_document.pdf: PE32 executable (GUI) Intel 80386, for MS Windows
```

That's not a PDF—it's a Windows executable! Don't open it!

**Scenario 2: Unknown file without extension**

```bash
$ file mystery_file
mystery_file: PNG image data, 1920 x 1080, 8-bit/color RGB
```

Now you know it's a PNG image. You can rename it:

```bash
mv mystery_file image.png
```

---

## cat - Reading Files

**What it does:** Displays the entire content of one or more files.

**Syntax:**

```bash
cat filename
```

The name "cat" stands for "concatenate"—it can combine multiple files.

---

### Basic Usage

**Read a single file:**

```bash
$ cat myfile.txt
Hello, this is the content of my file!
```

**Read multiple files:**

```bash
$ cat file1.txt file2.txt
Content of file1
Content of file2
```

**Create a file with cat:**

```bash
$ cat > newfile.txt
Type your content here
Press Ctrl+D when done
```

---

### Practical Examples

**Example 1: Quick config file check**

```bash
cat /etc/hostname
```

**Example 2: Combining files**

```bash
cat header.txt body.txt footer.txt > complete.txt
```

**Example 3: Displaying with line numbers**

```bash
cat -n myfile.txt
```

Output:

```
     1  First line
     2  Second line
     3  Third line
```

---

### When NOT to Use cat

**Don't use cat for:**

- Large files (use `less` instead)
- Binary files (will display garbage)
- Files you need to search or navigate

**Warning example:**

```bash
$ cat /var/log/syslog
# Thousands of lines flood your screen!
```

Use `less` for large files instead!

---

## less - Viewing Files Page by Page

**What it does:** Opens files in a paginated, scrollable viewer.

**Syntax:**

```bash
less filename
```

Think of `less` as a read-only text viewer—perfect for large files like logs and documentation.

---

### Basic Usage

```bash
less /var/log/syslog
```

The file opens in a viewer. Now you can navigate!

---

### Navigation Commands

**Basic scrolling:**

- `Space` or `Page Down` - Next page
- `b` or `Page Up` - Previous page
- `Down Arrow` - One line down
- `Up Arrow` - One line up

**Jumping:**

- `g` - Go to beginning of file
- `G` - Go to end of file
- `50g` - Go to line 50

**Searching:**

- `/pattern` - Search forward for "pattern"
- `?pattern` - Search backward for "pattern"
- `n` - Next search result
- `N` - Previous search result

**Other:**

- `h` - Help (shows all commands)
- `q` - Quit and return to terminal

---

### Practical Examples

**Example 1: Reading system logs**

```bash
less /var/log/syslog
```

Then press `/error` to search for errors.

**Example 2: Viewing documentation**

```bash
less README.md
```

**Example 3: Following a log file**

```bash
less +F /var/log/syslog
```

The `+F` flag makes `less` behave like `tail -f`, showing new lines as they're added.

---

### Pro Tips

**Tip 1:** `less` is "more" powerful than the older `more` command. If you see tutorials using `more`, use `less` instead!

**Tip 2:** Search is case-sensitive by default. Use `/pattern/i` for case-insensitive search.

**Tip 3:** Press `v` while in `less` to open the file in your default editor!

---

## history - Command History

**What it does:** Shows a list of commands you've previously executed.

**Syntax:**

```bash
history
```

---

### Basic Usage

```bash
$ history
    1  ls
    2  cd Documents
    3  pwd
    4  cat myfile.txt
    5  history
```

---

### Useful Tricks

**Rerun a command by number:**

```bash
!3  # Reruns command #3 (pwd in the example above)
```

**Rerun the last command:**

```bash
!!
```

**Rerun the last command that started with 'cd':**

```bash
!cd
```

**Use last argument from previous command:**

```bash
$ cat /var/log/syslog
$ less !$  # !$ expands to /var/log/syslog
```

**Search history:**
Press `Ctrl+R` and start typing:

```bash
(reverse-i-search)`pwd': pwd
```

---

### Clear the Screen

When your terminal gets cluttered:

```bash
clear
```

Or use the keyboard shortcut: `Ctrl+L`

---

## cp - Copying Files

**What it does:** Creates a duplicate of a file or directory.

**Syntax:**

```bash
cp source destination
```

---

### Basic Usage

**Copy a file:**

```bash
cp myfile.txt myfile_backup.txt
```

**Copy to a directory:**

```bash
cp myfile.txt /home/pete/Documents/
```

**Copy with a new name:**

```bash
cp myfile.txt /home/pete/Documents/newname.txt
```

---

### Wildcard Characters

Wildcards let you work with multiple files at once!

**Asterisk (\*) - matches anything:**

```bash
# Copy all .jpg files
cp *.jpg /home/pete/Pictures/

# Copy all files starting with "report"
cp report* /home/pete/Documents/
```

**Question mark (?) - matches one character:**

```bash
# Copy file1.txt, file2.txt, fileA.txt, etc.
cp file?.txt /backup/
```

**Brackets ([]) - matches any character in brackets:**

```bash
# Copy file1.txt, file2.txt, file3.txt only
cp file[123].txt /backup/
```

---

### Copying Directories

**To copy a directory, use the `-r` (recursive) flag:**

```bash
cp -r MyFolder/ /home/pete/Documents/
```

Without `-r`, you'll get an error:

```bash
$ cp MyFolder/ /backup/
cp: -r not specified; omitting directory 'MyFolder/'
```

---

### Safety Options

**Interactive mode (-i):**
Asks before overwriting:

```bash
$ cp -i myfile.txt /backup/myfile.txt
cp: overwrite '/backup/myfile.txt'?
```

Type `y` for yes, `n` for no.

**Verbose mode (-v):**
Shows what's being copied:

```bash
$ cp -v myfile.txt /backup/
'myfile.txt' -> '/backup/myfile.txt'
```

**Combine options:**

```bash
cp -riv MyFolder/ /backup/
```

Copies recursively, asks before overwriting, and shows progress!

---

### Real-World Examples

**Example 1: Backup before editing**

```bash
cp config.conf config.conf.backup
```

**Example 2: Copy all images**

```bash
cp *.{jpg,jpeg,png,gif} /media/usb/pictures/
```

**Example 3: Preserve file attributes**

```bash
cp -p myfile.txt /backup/
```

The `-p` flag preserves permissions, timestamps, and ownership.

---

## mv - Moving and Renaming

**What it does:** Moves files/directories to a new location OR renames them.

**Syntax:**

```bash
mv source destination
```

**Important:** Unlike `cp`, `mv` doesn't create a copy—it actually moves the file!

---

### Renaming Files

**Rename a file:**

```bash
mv oldname.txt newname.txt
```

**Rename a directory:**

```bash
mv OldFolder NewFolder
```

---

### Moving Files

**Move a file to a directory:**

```bash
mv myfile.txt /home/pete/Documents/
```

**Move multiple files:**

```bash
mv file1.txt file2.txt file3.txt /home/pete/Documents/
```

**Move and rename simultaneously:**

```bash
mv myfile.txt /home/pete/Documents/renamed.txt
```

---

### Safety Options

**Interactive mode (-i):**

```bash
mv -i file.txt /backup/
```

Asks before overwriting existing files.

**Backup mode (-b):**

```bash
mv -b file.txt /backup/
```

Creates a backup of the destination file if it exists.

**Verbose mode (-v):**

```bash
$ mv -v file.txt /backup/
'file.txt' -> '/backup/file.txt'
```

---

### Common Use Cases

**Use Case 1: Organizing files**

```bash
mv *.pdf ~/Documents/PDFs/
mv *.jpg ~/Pictures/
```

**Use Case 2: Fixing typos**

```bash
mv Documets/ Documents/
```

**Use Case 3: Cleaning up**

```bash
mv *.log /var/log/archive/
```

---

### mv vs cp

| Feature       | mv                            | cp                      |
| ------------- | ----------------------------- | ----------------------- |
| Original file | Deleted                       | Preserved               |
| Speed         | Fast (just updates directory) | Slower (copies data)    |
| Disk space    | No extra space needed         | Requires double space   |
| Use when      | Reorganizing, renaming        | Backing up, duplicating |

---

## mkdir - Making Directories

**What it does:** Creates new directories (folders).

**Syntax:**

```bash
mkdir directory_name
```

---

### Basic Usage

**Create a single directory:**

```bash
mkdir MyFolder
```

**Create multiple directories:**

```bash
mkdir Folder1 Folder2 Folder3
```

**Verify creation:**

```bash
$ mkdir TestFolder
$ ls -ld TestFolder
drwxr-xr-x 2 pete users 4096 Oct 24 11:30 TestFolder
```

---

### Creating Nested Directories

**Problem:** This fails!

```bash
$ mkdir projects/web/frontend
mkdir: cannot create directory 'projects/web/frontend': No such file or directory
```

Why? Because `projects` and `projects/web` don't exist yet.

**Solution:** Use the `-p` (parents) flag!

```bash
mkdir -p projects/web/frontend
```

This creates all necessary parent directories automatically.

---

### Practical Examples

**Example 1: Project structure**

```bash
mkdir -p myproject/{src,docs,tests}
```

This creates:

```
myproject/
├── src/
├── docs/
└── tests/
```

**Example 2: Date-based folders**

```bash
mkdir -p backups/2024/{01,02,03,04,05,06,07,08,09,10,11,12}
```

**Example 3: Verbose creation**

```bash
$ mkdir -pv projects/web/frontend
mkdir: created directory 'projects'
mkdir: created directory 'projects/web'
mkdir: created directory 'projects/web/frontend'
```

---

### Setting Permissions During Creation

```bash
mkdir -m 755 PublicFolder
mkdir -m 700 PrivateFolder
```

The `-m` flag sets permissions immediately.

---

## rm - Removing Files

**What it does:** Permanently deletes files and directories.

**Syntax:**

```bash
rm filename
```

---

###
