# Part 7: System Administration

[← Back to Part 6](./06-users-permissions-processes.md) | [Main Guide](../README.md) | [Next: Networking →](./08-networking.md)

---

## Table of Contents

- [Package Management](#package-management)
- [Filesystem Management](#filesystem-management)
- [Boot Process](#boot-process)
- [System Services](#system-services)
- [System Monitoring](#system-monitoring)
- [Task Scheduling](#task-scheduling)
- [System Logging](#system-logging)

---

## Package Management

### tar and gzip - Archiving

**Compress with gzip:**

```bash
gzip myfile          # Creates myfile.gz
gunzip myfile.gz     # Decompresses
```

**Create tar archive:**

```bash
tar cvf archive.tar file1 file2 dir/
```

- `c` - create
- `v` - verbose
- `f` - filename

**Extract tar archive:**

```bash
tar xvf archive.tar
```

- `x` - extract

**Create compressed tar (tar.gz):**

```bash
tar czf archive.tar.gz directory/
```

**Extract compressed tar:**

```bash
tar xzf archive.tar.gz
```

---

### dpkg (Debian/Ubuntu)

**Install package:**

```bash
sudo dpkg -i package.deb
```

**Remove package:**

```bash
sudo dpkg -r package
```

**List installed:**

```bash
dpkg -l
```

**Check if installed:**

```bash
dpkg -l | grep package-name
```

---

### apt (Debian/Ubuntu)

**Update package lists:**

```bash
sudo apt update
```

**Upgrade packages:**

```bash
sudo apt upgrade
```

**Install package:**

```bash
sudo apt install package-name
```

**Remove package:**

```bash
sudo apt remove package-name
```

**Search packages:**

```bash
apt search keyword
```

**Show package info:**

```bash
apt show package-name
```

**Full system upgrade:**

```bash
sudo apt update && sudo apt upgrade -y
```

---

### rpm and yum (Red Hat/CentOS)

**rpm commands:**

```bash
sudo rpm -i package.rpm    # Install
sudo rpm -e package        # Remove
rpm -qa                    # List all
rpm -qi package            # Info
```

**yum commands:**

```bash
sudo yum install package   # Install
sudo yum remove package    # Remove
sudo yum update            # Update all
yum search keyword         # Search
yum info package           # Info
```

---

### Compiling from Source

**Basic steps:**

1. **Install build tools:**

```bash
sudo apt install build-essential
```

2. **Extract source:**

```bash
tar xzf package.tar.gz
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

**Better: Use checkinstall:**

```bash
sudo apt install checkinstall
sudo checkinstall
```

Creates a .deb package for easy removal.

---

## Filesystem Management

### Device Files (/dev)

**View devices:**

```bash
ls /dev
```

**Common devices:**

- `/dev/sda` - First hard disk
- `/dev/sda1` - First partition
- `/dev/sdb` - Second hard disk
- `/dev/null` - Black hole (discards data)
- `/dev/zero` - Produces null bytes
- `/dev/random` - Random numbers

---

### Listing Hardware

**USB devices:**

```bash
lsusb
```

**PCI devices:**

```bash
lspci
```

**SCSI devices:**

```bash
lsscsi
```

**Block devices:**

```bash
lsblk
```

---

### Filesystem Hierarchy

**Important directories:**

- `/` - Root
- `/bin` - Essential binaries
- `/boot` - Boot files
- `/dev` - Devices
- `/etc` - Configuration
- `/home` - User homes
- `/lib` - Libraries
- `/mnt` - Mount point
- `/opt` - Optional software
- `/proc` - Process info
- `/root` - Root's home
- `/sbin` - System binaries
- `/tmp` - Temporary files
- `/usr` - User programs
- `/var` - Variable data (logs)

---

### Filesystem Types

**Common types:**

- **ext4** - Standard Linux
- **xfs** - High performance
- **btrfs** - Advanced features
- **ntfs** - Windows
- **fat32** - Universal

**View filesystem types:**

```bash
df -T
```

---

### Disk Partitioning

**View partitions:**

```bash
sudo parted -l
```

**Or:**

```bash
sudo fdisk -l
```

**Interactive partitioning:**

```bash
sudo parted /dev/sdb
```

Commands in parted:

- `print` - Show partitions
- `mkpart` - Create partition
- `rm` - Remove partition
- `quit` - Exit

---

### Creating Filesystems

**Format partition:**

```bash
sudo mkfs.ext4 /dev/sdb1
```

**Other formats:**

```bash
sudo mkfs.xfs /dev/sdb1
sudo mkfs.vfat /dev/sdb1
```

---

### Mounting Filesystems

**Mount manually:**

```bash
sudo mount /dev/sdb1 /mnt/mydrive
```

**Mount with type:**

```bash
sudo mount -t ext4 /dev/sdb1 /mnt/mydrive
```

**Unmount:**

```bash
sudo umount /mnt/mydrive
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

### /etc/fstab - Automatic Mounting

**Edit fstab:**

```bash
sudo nano /etc/fstab
```

**Format:**

```
UUID=xxxx-xxxx  /mnt/mydrive  ext4  defaults  0  2
```

**Fields:**

1. Device (UUID or path)
2. Mount point
3. Filesystem type
4. Options
5. Dump (backup)
6. Pass (fsck order)

---

### Swap Space

**Create swap partition:**

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

**View swap:**

```bash
swapon --show
free -h
```

---

### Disk Usage

**Check disk space:**

```bash
df -h
```

**Check directory size:**

```bash
du -sh /path/to/directory
```

**Find large files:**

```bash
du -ah /home | sort -rh | head -20
```

---

### Filesystem Check

**Check filesystem:**

```bash
sudo fsck /dev/sdb1
```

**⚠️ Never run on mounted filesystem!**

**Force check:**

```bash
sudo fsck -f /dev/sdb1
```

---

## Boot Process

### Four Stages

1. **BIOS/UEFI** - Hardware initialization
2. **Bootloader** - Loads kernel
3. **Kernel** - Initializes system
4. **Init** - Starts services

---

### Bootloader (GRUB)

**Update GRUB:**

```bash
sudo update-grub
```

**GRUB config:**

```bash
/boot/grub/grub.cfg
```

---

### Kernel

**View kernel version:**

```bash
uname -r
```

**Kernel location:**

- `/boot/vmlinuz` - Kernel
- `/boot/initrd.img` - Initial RAM disk

**List loaded modules:**

```bash
lsmod
```

**Load module:**

```bash
sudo modprobe module_name
```

**Remove module:**

```bash
sudo modprobe -r module_name
```

---

## System Services

### systemd (Modern)

**List services:**

```bash
systemctl list-units --type=service
```

**Service status:**

```bash
systemctl status servicename
```

**Start service:**

```bash
sudo systemctl start servicename
```

**Stop service:**

```bash
sudo systemctl stop servicename
```

**Restart service:**

```bash
sudo systemctl restart servicename
```

**Enable at boot:**

```bash
sudo systemctl enable servicename
```

**Disable at boot:**

```bash
sudo systemctl disable servicename
```

**View logs:**

```bash
journalctl -u servicename
```

---

### System V (Legacy)

**List services:**

```bash
service --status-all
```

**Control service:**

```bash
sudo service servicename start
sudo service servicename stop
sudo service servicename restart
```

---

### Power Management

**Shutdown:**

```bash
sudo shutdown -h now
sudo poweroff
```

**Reboot:**

```bash
sudo shutdown -r now
sudo reboot
```

**Schedule shutdown:**

```bash
sudo shutdown -h +60    # In 60 minutes
sudo shutdown -h 22:00  # At 10 PM
```

**Cancel shutdown:**

```bash
sudo shutdown -c
```

---

## System Monitoring

### top - Process Monitor

**Start top:**

```bash
top
```

**Sort by memory:** Press `M`  
**Sort by CPU:** Press `P`  
**Kill process:** Press `k`  
**Help:** Press `h`  
**Quit:** Press `q`

---

### uptime - System Load

```bash
uptime
```

**Output:**

```
10:30:45 up 5 days, 2:15, 3 users, load average: 0.52, 0.58, 0.59
```

**Load average:** 1-minute, 5-minute, 15-minute

---

### iostat - I/O Statistics

```bash
iostat
```

**Continuous monitoring:**

```bash
iostat 2
```

Updates every 2 seconds.

---

### vmstat - Virtual Memory

```bash
vmstat
```

**Continuous:**

```bash
vmstat 2 10
```

2-second intervals, 10 times.

---

### free - Memory Usage

```bash
free -h
```

**Output:**

```
              total        used        free      shared  buff/cache   available
Mem:          7.8Gi       2.3Gi       3.1Gi       156Mi       2.4Gi       5.2Gi
Swap:         2.0Gi          0B       2.0Gi
```

---

### df - Disk Space

```bash
df -h
```

**Specific filesystem:**

```bash
df -h /home
```

---

### du - Directory Usage

```bash
du -sh /path/to/directory
```

**Top 10 largest:**

```bash
du -sh /* | sort -rh | head -10
```

---

## Task Scheduling

### cron - Scheduled Tasks

**Edit crontab:**

```bash
crontab -e
```

**Format:**

```
* * * * * command
│ │ │ │ │
│ │ │ │ └─ Day of week (0-7, 0=Sunday)
│ │ │ └─── Month (1-12)
│ │ └───── Day of month (1-31)
│ └─────── Hour (0-23)
└───────── Minute (0-59)
```

**Examples:**

**Every day at 2:30 AM:**

```
30 2 * * * /path/to/script.sh
```

**Every Monday at 9:00 AM:**

```
0 9 * * 1 /path/to/script.sh
```

**Every hour:**

```
0 * * * * /path/to/script.sh
```

**Every 5 minutes:**

```
*/5 * * * * /path/to/script.sh
```

**View crontab:**

```bash
crontab -l
```

**Remove crontab:**

```bash
crontab -r
```

---

### at - One-time Tasks

**Install at:**

```bash
sudo apt install at
```

**Schedule command:**

```bash
echo "command" | at 10:00 PM
at 2:30 PM tomorrow
at now + 1 hour
```

**List scheduled:**

```bash
atq
```

**Remove scheduled:**

```bash
atrm job_number
```

---

## System Logging

### Important Log Files

**System logs:**

- `/var/log/syslog` - General system log
- `/var/log/messages` - System messages
- `/var/log/kern.log` - Kernel log
- `/var/log/auth.log` - Authentication log
- `/var/log/dmesg` - Boot messages

---

### Viewing Logs

**View log:**

```bash
cat /var/log/syslog
```

**Follow log:**

```bash
tail -f /var/log/syslog
```

**View with less:**

```bash
less /var/log/syslog
```

**View boot messages:**

```bash
dmesg
```

---

### journalctl (systemd)

**View all logs:**

```bash
journalctl
```

**Recent entries:**

```bash
journalctl -n 50
```

**Follow logs:**

```bash
journalctl -f
```

**Specific service:**

```bash
journalctl -u servicename
```

**Time range:**

```bash
journalctl --since "2025-01-01" --until "2025-01-31"
```

**Today only:**

```bash
journalctl --since today
```

---

### logger - Create Log Entries

```bash
logger "My custom log message"
logger -s "Message to screen too"
```

---

## Summary

✅ **Package management** - apt, yum, dpkg  
✅ **Filesystem** - Partitioning, mounting  
✅ **Boot process** - BIOS, bootloader, kernel  
✅ **Services** - systemd, System V  
✅ **Monitoring** - top, iostat, vmstat  
✅ **Scheduling** - cron, at  
✅ **Logging** - syslog, journalctl

---

## Quick Reference

```bash
# Packages
sudo apt update                 # Update lists
sudo apt install package        # Install
sudo apt remove package         # Remove

# Disk
df -h                          # Disk space
du -sh directory               # Directory size
sudo mount /dev/sdb1 /mnt      # Mount
sudo umount /mnt               # Unmount

# Services
sudo systemctl start service   # Start
sudo systemctl stop service    # Stop
sudo systemctl enable service  # Enable at boot

# Monitoring
top                            # Processes
free -h                        # Memory
df -h                          # Disk space
uptime                         # Load average

# Logs
tail -f /var/log/syslog       # Follow log
journalctl -f                  # Follow systemd logs
dmesg                          # Boot messages

# Cron
crontab -e                     # Edit schedule
crontab -l                     # List schedule
```

---

## Practice Exercises

**Exercise 1: Package Management**

```bash
# 1. Update package lists
# 2. Install a package
# 3. Check if it's installed
# 4. Remove the package
```

**Exercise 2: Disk Management**

```bash
# 1. Check disk usage
# 2. Find largest directories
# 3. View mounted filesystems
# 4. Check swap usage
```

**Exercise 3: Service Management**

```bash
# 1. List all services
# 2. Check status of a service
# 3. Restart a service
# 4. View service logs
```

**Exercise 4: Scheduling**

```bash
# 1. Create a cron job to run daily
# 2. List your cron jobs
# 3. Schedule a one-time task with at
```

---

## Next Steps

Excellent! You now have system administration skills.

**Continue to:** [Part 8: Networking](./08-networking.md)

---

[← Back to Part 6](./06-users-permissions-processes.md) | [Main Guide](../README.md) | [Next: Networking →](./08-networking.md)
