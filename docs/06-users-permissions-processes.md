# Part 6: Users, Permissions & Processes

[← Back to Part 5](./05-text-editors.md) | [Main Guide](../README.md) | [Next: System Administration →](./07-system-administration.md)

---

## Table of Contents

- [Users and Groups](#users-and-groups)
- [File Permissions](#file-permissions)
- [Processes](#processes)
- [Signals and Job Control](#signals-and-job-control)

---

## Users and Groups

### root User

**The superuser** with unrestricted system access.

**Switch to root:**

```bash
su
```

**Run single command as root:**

```bash
sudo command
```

**Why be careful:** As root, you can destroy your entire system with one command!

---

### /etc/passwd

**Contains basic user information.**

**View it:**

```bash
cat /etc/passwd
```

**Example line:**

```
pete:x:1000:1000:Pete User:/home/pete:/bin/bash
```

**Fields (colon-separated):**

1. Username: `pete`
2. Password: `x` (actual password in /etc/shadow)
3. User ID (UID): `1000`
4. Group ID (GID): `1000`
5. Description: `Pete User`
6. Home directory: `/home/pete`
7. Default shell: `/bin/bash`

---

### /etc/shadow

**Encrypted password storage** (root only).

**View:**

```bash
sudo cat /etc/shadow
```

**Example:**

```
root:$6$MyEncryptedPassword:18500:0:99999:7:::
```

**Fields:**

1. Username
2. Encrypted password
3. Last password change (days since Jan 1, 1970)
4. Minimum password age
5. Maximum password age
6. Warning period
7. Inactivity period
8. Expiration date
9. Reserved field

---

### /etc/group

**Group information.**

**View:**

```bash
cat /etc/group
```

**Example:**

```
users:x:100:pete,john
```

**Fields:**

1. Group name: `users`
2. Password: `x` (rarely used)
3. GID: `100`
4. Members: `pete,john`

---

### User Management

**Add user (basic):**

```bash
sudo useradd bob
```

**Add user (interactive, recommended):**

```bash
sudo adduser bob
```

This prompts for password and user details.

**Remove user:**

```bash
sudo userdel bob
```

**Remove user and home directory:**

```bash
sudo userdel -r bob
```

**Change password:**

```bash
sudo passwd bob
```

**Change your own password:**

```bash
passwd
```

**Modify user:**

```bash
sudo usermod -aG groupname username  # Add to group
sudo usermod -s /bin/zsh username    # Change shell
sudo usermod -d /new/home username   # Change home
```

---

## File Permissions

### Understanding Permission Format

```
-rwxr-xr-x 1 pete users 4096 Oct 24 10:30 myfile
│││││││││
│└┴┴┴┴┴┴┴─ Permissions
└───────── File type
```

**File type (first character):**

- `-` = Regular file
- `d` = Directory
- `l` = Symbolic link
- `b` = Block device
- `c` = Character device

**Permission groups (9 characters):**

```
rwx r-x r-x
│││ │││ │││
│││ │││ └┴┴─ Others (everyone else)
│││ └┴┴───── Group
└┴┴─────── Owner (user)
```

---

### Permission Types

**r (read) = 4:**

- **Files:** View contents
- **Directories:** List contents (ls)

**w (write) = 2:**

- **Files:** Modify contents
- **Directories:** Add/delete files

**x (execute) = 1:**

- **Files:** Run as program
- **Directories:** Enter directory (cd)

**- (no permission) = 0:**

- No access

---

### Modifying Permissions (chmod)

#### Symbolic Method

**Add permission:**

```bash
chmod u+x myfile    # Add execute for user (owner)
chmod g+w myfile    # Add write for group
chmod o+r myfile    # Add read for others
```

**Remove permission:**

```bash
chmod u-x myfile    # Remove execute for user
chmod g-w myfile    # Remove write for group
chmod o-r myfile    # Remove read for others
```

**Set exact permissions:**

```bash
chmod u=rwx myfile  # User: rwx
chmod g=rx myfile   # Group: r-x
chmod o=r myfile    # Others: r--
```

**Multiple changes at once:**

```bash
chmod u+x,g+w,o-r myfile
chmod ug+rw,o-rwx myfile
```

**Who codes:**

- `u` = user (owner)
- `g` = group
- `o` = others
- `a` = all (everyone)

**Examples:**

```bash
chmod a+r myfile    # Everyone can read
chmod u=rwx,go=rx myfile  # Owner: rwx, Group & Others: r-x
```

---

#### Numeric Method

**Permission values:**

- **4** = read (r)
- **2** = write (w)
- **1** = execute (x)
- **0** = no permission (-)

**Calculate by adding:**

- **7** (4+2+1) = rwx
- **6** (4+2) = rw-
- **5** (4+1) = r-x
- **4** = r--
- **3** (2+1) = -wx
- **2** = -w-
- **1** = --x
- **0** = ---

**Format:** `chmod XYZ file`

- X = Owner permissions
- Y = Group permissions
- Z = Others permissions

**Examples:**

```bash
chmod 755 myfile    # rwxr-xr-x (owner: full, others: read+execute)
chmod 644 myfile    # rw-r--r-- (owner: rw, others: read only)
chmod 700 myfile    # rwx------ (owner only)
chmod 666 myfile    # rw-rw-rw- (everyone can read/write)
chmod 600 myfile    # rw------- (private file)
```

**Common permissions:**

- **755** - Standard executable or directory
- **644** - Standard file
- **600** - Private file (owner only)
- **700** - Private directory
- **777** - World-writable (DANGEROUS! Avoid!)

**Apply recursively:**

```bash
chmod -R 755 /path/to/directory
```

---

### Changing Ownership

**Change owner:**

```bash
sudo chown pete myfile
```

**Change group:**

```bash
sudo chgrp users myfile
```

**Change both owner and group:**

```bash
sudo chown pete:users myfile
```

**Recursive (for directories):**

```bash
sudo chown -R pete:users /path/to/directory
```

**Examples:**

```bash
# Change ownership of web directory
sudo chown -R www-data:www-data /var/www/html

# Change ownership to current user
sudo chown $USER:$USER myfile
```

---

### Special Permissions

#### Setuid (SUID) - 4

**What it does:** File runs with permissions of the file owner, not the user executing it.

**Set SUID:**

```bash
chmod u+s myfile
chmod 4755 myfile
```

**Example:**

```bash
$ ls -l /usr/bin/passwd
-rwsr-xr-x 1 root root 59640 passwd
```

Notice the `s` instead of `x` in owner's execute position. This allows regular users to change passwords (which requires root access to modify /etc/shadow).

**When to use:** Rare. Usually for system programs that need elevated privileges.

---

#### Setgid (SGID) - 2

**What it does:**

- **On files:** Runs with group permissions
- **On directories:** New files inherit the directory's group

**Set SGID:**

```bash
chmod g+s mydir
chmod 2755 mydir
```

**Use case for directories:**

```bash
sudo chgrp developers /shared/projects
sudo chmod 2775 /shared/projects
```

Now all files created in `/shared/projects` will belong to the `developers` group.

---

#### Sticky Bit - 1

**What it does:** In a directory, only the file owner can delete their files (even if others have write permission to the directory).

**Set sticky bit:**

```bash
chmod +t mydir
chmod 1755 mydir
```

**Example:**

```bash
$ ls -ld /tmp
drwxrwxrwt 10 root root 4096 /tmp
```

Notice the `t` at the end. This prevents users from deleting each other's temporary files in /tmp.

**Use case:**

```bash
mkdir /shared/uploads
chmod 1777 /shared/uploads
```

Everyone can create files, but only owners can delete their own.

---

### umask - Default Permissions

**What it is:** Determines default permissions for newly created files and directories.

**View current umask:**

```bash
umask
```

**Set umask:**

```bash
umask 022
```

**How it works:**

- **Default file base:** 666 (rw-rw-rw-)
- **Default directory base:** 777 (rwxrwxrwx)
- **umask:** Bits to REMOVE

**Calculation:**

```
File: 666 - 022 = 644 (rw-r--r--)
Directory: 777 - 022 = 755 (rwxr-xr-x)
```

**Common umask values:**

- **022** - Standard (files: 644, dirs: 755)
- **002** - Group-writable (files: 664, dirs: 775)
- **077** - Private (files: 600, dirs: 700)
- **000** - No restrictions (files: 666, dirs: 777)

**Make permanent:** Add to `~/.bashrc`

```bash
umask 022
```

---

## Processes

### What is a Process?

**A running instance of a program.** Each process has:

- **PID** - Process ID (unique identifier)
- **PPID** - Parent Process ID
- **Owner** - User who started it
- **State** - Current status
- **Memory** - RAM allocation
- **CPU time** - Processing time used
- **Priority** - Scheduling priority

---

### ps - Process Snapshot

**Basic usage:**

```bash
ps
```

**Output:**

```
  PID TTY          TIME CMD
 1234 pts/0    00:00:00 bash
 5678 pts/0    00:00:00 ps
```

**Columns:**

- **PID** - Process ID
- **TTY** - Controlling terminal
- **TIME** - Total CPU time used
- **CMD** - Command name

---

**Show all processes (detailed):**

```bash
ps aux
```

**Output:**

```
USER   PID %CPU %MEM    VSZ   RSS TTY   STAT START   TIME COMMAND
root     1  0.0  0.1  21500  3340 ?     Ss   09:00   0:01 /sbin/init
pete  1234  0.0  0.2  22000  4500 pts/0 Ss   10:00   0:00 bash
pete  5678  2.5  1.3 450000 55000 ?     Sl   10:15   0:45 firefox
```

**Columns explained:**

- **USER** - Process owner
- **PID** - Process ID
- **%CPU** - CPU usage percentage
- **%MEM** - Memory usage percentage
- **VSZ** - Virtual memory size (KB)
- **RSS** - Resident set size (physical memory, KB)
- **TTY** - Terminal (? = no terminal)
- **STAT** - Process state (see below)
- **START** - Start time
- **TIME** - Total CPU time
- **COMMAND** - Full command with arguments

---

**Process state codes (STAT):**

- **R** - Running or runnable
- **S** - Sleeping (interruptible)
- **D** - Sleeping (uninterruptible, usually I/O)
- **T** - Stopped (by job control signal)
- **Z** - Zombie (terminated, waiting for parent)
- **<** - High priority
- **N** - Low priority
- **L** - Has pages locked in memory
- **s** - Session leader
- **l** - Multi-threaded
- **+** - In foreground process group

---

**Useful ps options:**

```bash
ps aux                  # All processes, detailed
ps -ef                  # All processes, full format
ps -u pete              # Pete's processes
ps -C firefox           # All firefox processes
ps --forest             # Tree view (shows parent-child)
ps aux --sort=-%mem     # Sort by memory usage
ps aux --sort=-%cpu     # Sort by CPU usage
```

**Examples:**

```bash
# Find processes using lots of memory
ps aux --sort=-%mem | head -10

# Find all python processes
ps aux | grep python

# Show process tree
ps auxf

# Show threads
ps -eLf
```

---

### top - Interactive Process Viewer

**Start top:**

```bash
top
```

**Interactive commands (while top is running):**

- **q** - Quit
- **k** - Kill a process (prompts for PID)
- **M** - Sort by memory usage
- **P** - Sort by CPU usage
- **T** - Sort by time/cumulative time
- **u** - Filter by user
- **1** - Toggle individual CPU cores
- **h** or **?** - Help
- **Space** - Refresh immediately
- **d** - Change update delay

**Top display sections:**

```
top - 10:30:45 up 5 days, 2:15, 3 users, load average: 0.52, 0.58, 0.59
Tasks: 195 total,   1 running, 194 sleeping,   0 stopped,   0 zombie
%Cpu(s):  5.2 us,  1.1 sy,  0.0 ni, 93.5 id,  0.2 wa,  0.0 hi,  0.0 si
MiB Mem :   7823.4 total,   1234.5 free,   3456.7 used,   3132.2 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.   3987.6 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
 1234 pete      20   0  123456   4567   3456 S   2.3   0.6   1:23.45 firefox
```

**Understanding the header:**

**Line 1:** System info

- Current time: 10:30:45
- Uptime: 5 days, 2 hours, 15 minutes
- Users: 3 logged in
- Load average: 1, 5, 15 minute averages

**Line 2:** Task summary

- Total processes and their states

**Line 3:** CPU usage

- **us** - User processes
- **sy** - System (kernel)
- **ni** - Nice (low priority)
- **id** - Idle
- **wa** - I/O wait
- **hi** - Hardware interrupts
- **si** - Software interrupts
- **st** - Steal time (virtual machines)

**Lines 4-5:** Memory

- Physical RAM and swap usage

**Process list columns:**

- **PID** - Process ID
- **USER** - Owner
- **PR** - Priority
- **NI** - Nice value
- **VIRT** - Virtual memory
- **RES** - Resident (physical) memory
- **SHR** - Shared memory
- **S** - State
- **%CPU** - CPU usage
- **%MEM** - Memory usage
- **TIME+** - Total CPU time
- **COMMAND** - Process name

---

### Controlling Processes

**View specific process:**

```bash
ps -p 1234
```

**Process tree:**

```bash
pstree
pstree -p  # With PIDs
```

**Find process by name:**

```bash
pgrep firefox
pgrep -u pete  # Pete's processes
```

**Find with details:**

```bash
ps aux | grep firefox
```

**Kill process by name:**

```bash
pkill firefox
pkill -u pete  # Kill pete's processes
```

---

## Signals and Job Control

### Signals

**What are signals:** Messages sent to processes to control their behavior.

**Common signals:**

| Signal  | Number | Description            | Can Catch? |
| ------- | ------ | ---------------------- | ---------- |
| SIGHUP  | 1      | Hangup (reload config) | Yes        |
| SIGINT  | 2      | Interrupt (Ctrl+C)     | Yes        |
| SIGQUIT | 3      | Quit                   | Yes        |
| SIGKILL | 9      | Kill immediately       | NO         |
| SIGTERM | 15     | Terminate gracefully   | Yes        |
| SIGSTOP | 17/19  | Stop (pause)           | NO         |
| SIGCONT | 18/19  | Continue after stop    | Yes        |

**Key difference:**

- **SIGTERM (15)** - Asks nicely, process can clean up
- **SIGKILL (9)** - Forces immediate termination, no cleanup

---

### kill - Send Signals

**Terminate gracefully (default SIGTERM):**

```bash
kill 1234
```

**Force kill (SIGKILL):**

```bash
kill -9 1234
kill -SIGKILL 1234
kill -KILL 1234
```

**Reload configuration (SIGHUP):**

```bash
kill -HUP 1234
kill -1 1234
```

**Stop (pause) process:**

```bash
kill -STOP 1234
```

**Continue stopped process:**

```bash
kill -CONT 1234
```

**List all signals:**

```bash
kill -l
```

---

### killall - Kill by Name

**Kill all processes with name:**

```bash
killall firefox
```

**Force kill:**

```bash
killall -9 firefox
```

**Interactive (ask for confirmation):**

```bash
killall -i firefox
```

**Kill for specific user:**

```bash
killall -u pete firefox
```

---

### Process Priority (Nice)

**What is niceness:** Priority level from -20 (highest) to 19 (lowest). Default is 0.

**Start process with nice value:**

```bash
nice -n 10 command
nice -n 10 apt upgrade
```

**Change priority of running process:**

```bash
renice 5 -p 1234
renice -5 -p 1234  # Requires root for negative values
```

**View process nice values:**

```bash
ps -el  # Shows NI column
```

**Who can do what:**

- **Regular users:** Can only increase niceness (lower priority, 0 to 19)
- **Root:** Can set any niceness (-20 to 19)

**Use cases:**

- High nice value (10-19): Background tasks, low priority
- Low nice value (-10 to -1): Critical tasks (requires root)

---

### Background and Foreground Jobs

**Start process in background:**

```bash
command &
```

**Example:**

```bash
$ sleep 100 &
[1] 1234
```

- `[1]` is the job number
- `1234` is the PID

---

**View background jobs:**

```bash
jobs
```

**Output:**

```
[1]+  Running                 sleep 100 &
[2]-  Running                 sleep 200 &
[3]   Stopped                 vim file.txt
```

- **[1]+** - Current job
- **[2]-** - Previous job
- **[3]** - Other jobs

---

**Move running process to background:**

1. Start a command:

```bash
sleep 100
```

2. Suspend it with `Ctrl+Z`:

```bash
^Z
[1]+  Stopped                 sleep 100
```

3. Continue in background:

```bash
bg
[1]+ sleep 100 &
```

Or specify job number:

```bash
bg %1
```

---

**Move background job to foreground:**

```bash
fg %1
```

Without number, brings most recent job to foreground:

```bash
fg
```

---

**Kill background job:**

```bash
kill %1        # Kill job 1
kill %firefox  # Kill job by name
```

---

### Keyboard Shortcuts

**Ctrl+C** - Send SIGINT (interrupt/kill)

```bash
$ sleep 100
^C
$
```

**Ctrl+Z** - Send SIGTSTP (suspend/stop)

```bash
$ sleep 100
^Z
[1]+  Stopped                 sleep 100
```

**Ctrl+D** - Send EOF (end of file/input)

- Exits shell if typed at empty prompt
- Signals end of input to commands

\*\*Ctrl+\*\* - Send SIGQUIT (quit with core dump)

---

### Process States

**R (Running)**

- Currently executing on CPU
- Or ready to run (in run queue)

**S (Sleeping - Interruptible)**

- Waiting for an event (user input, network packet)
- Can be interrupted by signals

**D (Sleeping - Uninterruptible)**

- Waiting for I/O (disk, network)
- Cannot be interrupted (even by SIGKILL!)
- Usually brief

**T (Stopped)**

- Suspended by job control (Ctrl+Z)
- Or by debugger

**Z (Zombie)**

- Terminated but not yet reaped by parent
- Takes minimal resources
- Parent should call wait() to clean up

**If you see many zombies:** Parent process has a bug.

---

### /proc Filesystem

**What it is:** Virtual filesystem containing process and system information.

**Process directories:**

```bash
ls /proc
```

Shows directories named by PID.

**View process info:**

```bash
cat /proc/1234/status        # Process status
cat /proc/1234/cmdline       # Command line
cat /proc/1234/environ       # Environment variables
cat /proc/1234/cwd           # Current directory (symlink)
cat /proc/1234/exe           # Executable (symlink)
ls -l /proc/1234/fd/         # Open file descriptors
```

**System info:**

```bash
cat /proc/cpuinfo            # CPU information
cat /proc/meminfo            # Memory information
cat /proc/version            # Kernel version
cat /proc/uptime             # System uptime
cat /proc/loadavg            # Load average
cat /proc/filesystems        # Supported filesystems
cat /proc/mounts             # Mounted filesystems
```

**Real-time process info:**

```bash
watch -n 1 'cat /proc/1234/status | grep VmRSS'  # Watch memory usage
```

---

## Summary

✅ **Users & Groups** - Managing accounts and group membership  
✅ **Passwords** - /etc/passwd, /etc/shadow, /etc/group  
✅ **File Permissions** - Read, write, execute for owner/group/others  
✅ **chmod** - Change permissions (symbolic and numeric)  
✅ **chown/chgrp** - Change ownership  
✅ **Special Permissions** - SUID, SGID, sticky bit  
✅ **Processes** - ps, top for viewing  
✅ **Process Control** - kill, nice, renice  
✅ **Signals** - SIGTERM, SIGKILL, SIGHUP, etc.  
✅ **Job Control** - Background/foreground, Ctrl+Z, fg, bg  
✅ **/proc** - Virtual filesystem for process info

---

## Quick Reference

```bash
# Users
sudo adduser username       # Add user
sudo passwd username        # Change password
sudo userdel -r username    # Delete user and home
sudo usermod -aG group user # Add to group

# Permissions
chmod 755 file             # rwxr-xr-x
chmod 644 file             # rw-r--r--
chmod u+x file             # Add execute for user
chmod -R 755 directory     # Recursive
chown user:group file      # Change ownership

# Processes
ps aux                     # List all processes
ps aux | grep name         # Find specific process
top                        # Interactive viewer
pgrep name                 # Find PID by name

# Process control
kill PID                   # Terminate (SIGTERM)
kill -9 PID                # Force kill (SIGKILL)
killall name               # Kill by name
nice -n 10 command         # Start with low priority
renice 5 -p PID            # Change priority

# Job control
command &                  # Run in background
jobs                       # List jobs
fg %1                      # Foreground job 1
bg %1                      # Background job 1
Ctrl+Z                     # Suspend current job
Ctrl+C                     # Kill current job
```

---

## Practice Exercises

**Exercise 1: User Management**

```bash
# 1. Create a new user
# 2. Set their password
# 3. Add them to a group
# 4. Check /etc/passwd entry
# 5. Switch to that user
```

**Exercise 2: Permissions**

```bash
# 1. Create a file
# 2. View its permissions
# 3. Change to 644 using numeric method
# 4. Change to rwxr-xr-x using symbolic method
# 5. Change ownership
# 6. Create a directory with sticky bit
```

**Exercise 3: Process Management**

```bash
# 1. List all your processes
# 2. Find PID of a specific program
# 3. Check memory usage with top
# 4. Start a background process
# 5. Suspend a process with Ctrl+Z
# 6. Resume it in background
# 7. Kill a process by PID
```

**Exercise 4: Job Control**

```bash
# 1. Start: sleep 300
# 2. Suspend it (Ctrl+Z)
# 3. Start another: sleep 400 &
# 4. List background jobs
# 5. Bring first job to foreground
# 6. Suspend and move to background
# 7. Kill both jobs
```

---

## Troubleshooting

**Problem: Permission denied**

```bash
$ ./script.sh
bash: ./script.sh: Permission denied

# Solution: Add execute permission
chmod +x script.sh
```

**Problem: Can't delete file owned by another user**

```bash
# Solution: Use sudo or change ownership
sudo rm file
# or
sudo chown $USER file
rm file
```

**Problem: Process won't die**

```bash
# Try graceful termination first
kill PID

# If that doesn't work, force it
kill -9 PID
```

**Problem: Process in D state (uninterruptible sleep)**

```bash
# Usually waiting for I/O
# Cannot be killed, usually resolves itself
# If persistent, may indicate hardware/driver issue
# Reboot may be necessary
```

**Problem: Many zombie processes**

```bash
# Find parent process
ps -eo pid,ppid,stat,cmd | grep Z

# Kill parent process (it's not cleaning up children)
kill PARENT_PID
```

---

## Next Steps

Excellent work! You now understand users, permissions, and process management—critical skills for any Linux user.

**Continue to:** [Part 7: System Administration](./07-system-administration.md)

---

[← Back to Part 5](./05-text-editors.md) | [Main Guide](../README.md) | [Next: System Administration →](./07-system-administration.md)
