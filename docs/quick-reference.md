# Quick Reference Guide

[← Back to Main Guide](../README.md)

---

## Navigation

```bash
pwd                         # Print working directory
cd /path/to/dir             # Change directory
cd ~                        # Go home
cd ..                       # Go up one level
cd -                        # Go to previous directory
ls                          # List files
ls -la                      # List all with details
ls -lh                      # List with human-readable sizes
```

---

## File Operations

```bash
touch file.txt              # Create file
cat file.txt                # Display file
less file.txt               # View file (paginated)
head file.txt               # First 10 lines
tail file.txt               # Last 10 lines
tail -f file.txt            # Follow file updates

cp file1 file2              # Copy file
cp -r dir1 dir2             # Copy directory
mv file1 file2              # Move/rename
rm file.txt                 # Delete file
rm -r directory             # Delete directory
rm -rf directory            # Force delete (DANGEROUS!)

mkdir directory             # Create directory
mkdir -p path/to/dir        # Create nested directories
rmdir directory             # Remove empty directory

find /path -name "*.txt"    # Find files
find /path -type f -size +10M  # Find large files
```

---

## File Viewing & Editing

```bash
cat file.txt                # Display entire file
less file.txt               # Page through file
head -n 20 file.txt         # First 20 lines
tail -n 20 file.txt         # Last 20 lines

vim file.txt                # Edit with Vim
nano file.txt               # Edit with Nano
emacs file.txt              # Edit with Emacs
```

---

## Text Processing

```bash
grep "pattern" file         # Search in file
grep -r "pattern" /path     # Recursive search
grep -i "pattern" file      # Case-insensitive
grep -v "pattern" file      # Invert match
grep -n "pattern" file      # Show line numbers

sort file.txt               # Sort lines
sort -r file.txt            # Reverse sort
sort -n file.txt            # Numeric sort
uniq file.txt               # Remove duplicates
cut -d "," -f 2 file.csv    # Extract column
tr 'a-z' 'A-Z'              # Translate characters

wc file.txt                 # Count lines, words, chars
wc -l file.txt              # Count lines only
```

---

## Pipes & Redirection

```bash
command > file              # Redirect output (overwrite)
command >> file             # Redirect output (append)
command < file              # Redirect input
command 2> file             # Redirect errors
command &> file             # Redirect all
command 2>&1                # Redirect errors to output

cmd1 | cmd2                 # Pipe output
cmd | tee file              # Output to file and screen
```

---

## Permissions

```bash
chmod 755 file              # rwxr-xr-x
chmod 644 file              # rw-r--r--
chmod u+x file              # Add execute for user
chmod g-w file              # Remove write for group
chmod a+r file              # Add read for all

chown user:group file       # Change owner and group
chown user file             # Change owner only
chgrp group file            # Change group only
chown -R user:group dir/    # Recursive
```

**Permission Values:**

- 4 = read (r)
- 2 = write (w)
- 1 = execute (x)
- 7 = rwx, 6 = rw-, 5 = r-x, 4 = r--, 0 = ---

---

## Users & Groups

```bash
whoami                      # Current user
id                          # User and group IDs
sudo command                # Run as root
su                          # Switch to root
su - username               # Switch user

sudo adduser username       # Add user
sudo passwd username        # Change password
sudo userdel username       # Delete user
sudo usermod -aG group user # Add user to group
```

---

## Processes

```bash
ps                          # Show processes
ps aux                      # Show all processes
ps aux | grep process       # Find specific process
top                         # Interactive process viewer
htop                        # Better top (if installed)

kill PID                    # Terminate process
kill -9 PID                 # Force kill
killall name                # Kill by name
pkill name                  # Kill by name

nice -n 10 command          # Start with priority
renice 5 -p PID             # Change priority

command &                   # Run in background
jobs                        # List background jobs
fg %1                       # Bring to foreground
bg %1                       # Resume in background
Ctrl+Z                      # Suspend process
Ctrl+C                      # Kill process
```

---

## System Information

```bash
uname -a                    # System information
uname -r                    # Kernel version
hostname                    # System hostname
uptime                      # System uptime
date                        # Current date/time
cal                         # Calendar
w                           # Who is logged in
last                        # Login history

df -h                       # Disk space
du -sh directory            # Directory size
free -h                     # Memory usage
lscpu                       # CPU information
lsblk                       # Block devices
lsusb                       # USB devices
lspci                       # PCI devices
```

---

## Package Management

**Debian/Ubuntu (apt):**

```bash
sudo apt update             # Update package lists
sudo apt upgrade            # Upgrade packages
sudo apt install package    # Install package
sudo apt remove package     # Remove package
apt search keyword          # Search packages
apt show package            # Show package info
```

**Red Hat/CentOS (yum):**

```bash
sudo yum update             # Update packages
sudo yum install package    # Install package
sudo yum remove package     # Remove package
yum search keyword          # Search packages
yum info package            # Show package info
```

---

## Archives & Compression

```bash
tar czf archive.tar.gz dir/ # Create compressed archive
tar xzf archive.tar.gz      # Extract compressed archive
tar czf backup.tar.gz file1 file2  # Archive files
tar xzf archive.tar.gz -C /path    # Extract to path

gzip file                   # Compress file
gunzip file.gz              # Decompress file
zip archive.zip files       # Create zip
unzip archive.zip           # Extract zip
```

---

## Networking

```bash
ip addr show                # Show IP addresses
ip link show                # Show interfaces
ip route show               # Show routes
ifconfig                    # Show network config (legacy)

ping hostname               # Test connectivity
ping -c 4 hostname          # Ping 4 times
traceroute hostname         # Trace route
nslookup domain             # DNS lookup
dig domain                  # Detailed DNS query
host domain                 # Simple DNS lookup

netstat -tuln               # Show listening ports
ss -tuln                    # Socket statistics (modern)
sudo lsof -i :80            # What's using port 80

wget URL                    # Download file
curl URL                    # Transfer data
curl -I URL                 # Show headers only
scp file user@host:/path    # Secure copy
rsync -avz src/ dest/       # Sync files

ssh user@hostname           # SSH connect
ssh -p port user@host       # SSH on specific port
```

---

## Service Management

**systemd (Modern):**

```bash
sudo systemctl start service    # Start service
sudo systemctl stop service     # Stop service
sudo systemctl restart service  # Restart service
sudo systemctl status service   # Service status
sudo systemctl enable service   # Enable at boot
sudo systemctl disable service  # Disable at boot
systemctl list-units --type=service  # List services
```

**SysV (Legacy):**

```bash
sudo service name start     # Start service
sudo service name stop      # Stop service
sudo service name restart   # Restart service
service --status-all        # List all services
```

---

## Logs

```bash
tail -f /var/log/syslog     # Follow system log
tail -f /var/log/auth.log   # Follow auth log
less /var/log/syslog        # View system log
dmesg                       # Kernel messages
journalctl                  # View systemd logs
journalctl -f               # Follow systemd logs
journalctl -u service       # Service logs
journalctl --since today    # Today's logs
```

---

## Disk & Filesystem

```bash
df -h                       # Disk space
du -sh directory            # Directory size
du -h --max-depth=1         # Size of subdirectories
lsblk                       # List block devices
sudo fdisk -l               # List partitions
sudo parted -l              # Partition info

sudo mount /dev/sdb1 /mnt   # Mount filesystem
sudo umount /mnt            # Unmount filesystem
sudo mkfs.ext4 /dev/sdb1    # Create filesystem
sudo fsck /dev/sdb1         # Check filesystem
```

---

## Search & Find

```bash
find /path -name "file"     # Find by name
find /path -type f          # Find files only
find /path -type d          # Find directories only
find /path -size +100M      # Find large files
find /path -mtime -7        # Modified in last 7 days
find /path -user username   # Find by owner

locate filename             # Quick find (uses database)
sudo updatedb               # Update locate database

which command               # Show command path
whereis command             # Show binary, source, manual
```

---

## Help & Documentation

```bash
man command                 # Manual page
command --help              # Help message
help command                # Built-in help
whatis command              # One-line description
apropos keyword             # Search man pages
info command                # Info page
```

---

## Vim Quick Reference

```
# Modes
Esc         Normal mode
i           Insert mode
v           Visual mode
:           Command mode

# Navigation
h j k l     Left, down, up, right
w / b       Next/previous word
0 / $       Start/end of line
gg / G      Start/end of file

# Editing
dd          Delete line
yy          Copy line
p           Paste
u           Undo
Ctrl+r      Redo
.           Repeat

# Save/Exit
:w          Save
:q
```
