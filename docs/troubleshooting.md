# Troubleshooting Guide

[← Back to Main Guide](../README.md)

---

## Table of Contents

- [General Troubleshooting Methodology](#general-troubleshooting-methodology)
- [File and Directory Issues](#file-and-directory-issues)
- [Permission Problems](#permission-problems)
- [Process Issues](#process-issues)
- [Network Problems](#network-problems)
- [Disk and Filesystem Issues](#disk-and-filesystem-issues)
- [Package Management Problems](#package-management-problems)
- [System Performance Issues](#system-performance-issues)
- [Boot and Startup Problems](#boot-and-startup-problems)
- [Common Error Messages](#common-error-messages)

---

## General Troubleshooting Methodology

### The 5-Step Approach

1. **Identify the Problem**

   - What exactly isn't working?
   - When did it start?
   - What changed recently?

2. **Gather Information**

   - Check error messages
   - Review logs
   - Document current state

3. **Analyze**

   - What are the possible causes?
   - What can you test?

4. **Implement Solution**

   - Try fixes one at a time
   - Document what you try

5. **Verify and Document**
   - Did it work?
   - Document the solution

---

## File and Directory Issues

### Problem: "No such file or directory"

**Symptoms:**

```bash
$ cat myfile.txt
cat: myfile.txt: No such file or directory
```

**Common Causes:**

- File doesn't exist
- Wrong directory
- Typo in filename
- Case sensitivity (Linux is case-sensitive)

**Diagnosis:**

```bash
# Check current directory
pwd

# List files
ls -la

# Search for the file
find ~ -name "myfile.txt"
find ~ -iname "myfile.txt"  # Case-insensitive
```

**Solutions:**

```bash
# Navigate to correct directory
cd /correct/path

# Check spelling and case
ls -la | grep -i myfile

# Create the file if it should exist
touch myfile.txt
```

---

### Problem: Cannot delete file - "Directory not empty"

**Symptoms:**

```bash
$ rmdir mydir
rmdir: failed to remove 'mydir': Directory not empty
```

**Diagnosis:**

```bash
# Check what's inside
ls -la mydir/

# Check for hidden files
ls -la mydir/ | grep "^\."
```

**Solutions:**

```bash
# Remove directory and contents
rm -r mydir/

# Or remove contents first
rm -rf mydir/*
rm -rf mydir/.*  # Be careful with this!
rmdir mydir
```

---

### Problem: Cannot create file - "Text file busy"

**Symptoms:**

```bash
$ rm script.sh
rm: cannot remove 'script.sh': Text file busy
```

**Common Cause:** File is being executed

**Diagnosis:**

```bash
# Find processes using the file
lsof script.sh
fuser script.sh
```

**Solution:**

```bash
# Kill the process
kill <PID>

# Or force kill
kill -9 <PID>

# Then remove the file
rm script.sh
```

---

### Problem: Cannot access mounted drive

**Symptoms:**

```bash
$ cd /mnt/usbdrive
bash: cd: /mnt/usbdrive: No such file or directory
```

**Diagnosis:**

```bash
# Check if drive is mounted
mount | grep /mnt
df -h | grep /mnt

# Check available devices
lsblk
sudo fdisk -l
```

**Solutions:**

```bash
# Create mount point if it doesn't exist
sudo mkdir -p /mnt/usbdrive

# Mount the device
sudo mount /dev/sdb1 /mnt/usbdrive

# Make permanent by adding to /etc/fstab
echo "/dev/sdb1 /mnt/usbdrive ext4 defaults 0 0" | sudo tee -a /etc/fstab
```

---

## Permission Problems

### Problem: "Permission denied"

**Symptoms:**

```bash
$ cat /var/log/syslog
cat: /var/log/syslog: Permission denied
```

**Diagnosis:**

```bash
# Check file permissions
ls -l /var/log/syslog

# Check your user
whoami
id

# Check file ownership
stat /var/log/syslog
```

**Solutions:**

**Option 1: Use sudo**

```bash
sudo cat /var/log/syslog
```

**Option 2: Change permissions (if appropriate)**

```bash
sudo chmod 644 /var/log/syslog
```

**Option 3: Change ownership**

```bash
sudo chown $USER:$USER /var/log/syslog
```

---

### Problem: Cannot execute script

**Symptoms:**

```bash
$ ./myscript.sh
bash: ./myscript.sh: Permission denied
```

**Diagnosis:**

```bash
# Check if file has execute permission
ls -l myscript.sh
```

**Solution:**

```bash
# Add execute permission
chmod +x myscript.sh

# Now run it
./myscript.sh
```

---

### Problem: "Operation not permitted" even with sudo

**Symptoms:**

```bash
$ sudo rm important_file
rm: cannot remove 'important_file': Operation not permitted
```

**Common Cause:** File has immutable attribute

**Diagnosis:**

```bash
# Check file attributes
lsattr important_file
```

**Solution:**

```bash
# Remove immutable attribute
sudo chattr -i important_file

# Now you can delete it
sudo rm important_file
```

---

### Problem: Wrong ownership after copying files

**Symptoms:**
Files copied by root are owned by root and not accessible

**Solution:**

```bash
# Change ownership recursively
sudo chown -R $USER:$USER /path/to/directory

# Or specific user
sudo chown -R username:groupname /path/to/directory
```

---

## Process Issues

### Problem: Process won't die

**Symptoms:**

```bash
$ kill 1234
# Process still running
```

**Diagnosis:**

```bash
# Check process state
ps aux | grep 1234

# Check what the process is doing
sudo lsof -p 1234
```

**Solutions:**

**Escalate signal strength:**

```bash
# Try TERM (graceful)
kill 1234

# Wait a few seconds, then try KILL (force)
kill -9 1234

# Or use killall
killall -9 process_name
```

---

### Problem: Too many processes

**Symptoms:**

```bash
-bash: fork: Cannot allocate memory
-bash: fork: retry: Resource temporarily unavailable
```

**Diagnosis:**

```bash
# Check number of processes
ps aux | wc -l

# Check processes by user
ps aux | grep username | wc -l

# Check system limits
ulimit -a
```

**Solutions:**

```bash
# Kill unnecessary processes
pkill -u username process_name

# Increase user limits (edit /etc/security/limits.conf)
sudo nano /etc/security/limits.conf
# Add:
# username hard nproc 10000

# Or kill all user processes (careful!)
sudo pkill -u username
```

---

### Problem: Zombie processes

**Symptoms:**

```bash
$ ps aux | grep Z
user  1234  0.0  0.0      0     0 ?        Z    10:00   0:00 [defunct]
```

**Common Cause:** Parent process not cleaning up child processes

**Diagnosis:**

```bash
# Find parent process
ps -eo pid,ppid,stat,cmd | grep Z

# Check parent
ps -p <PPID>
```

**Solution:**

```bash
# Kill parent process (not the zombie)
kill <PPID>

# Or restart the parent service
sudo systemctl restart service_name

# Zombies can't be killed directly - they're already dead!
```

---

### Problem: Process in D state (uninterruptible sleep)

**Symptoms:**
Process won't respond to any signal, even SIGKILL

**Diagnosis:**

```bash
# Check process state
ps aux | grep " D "
```

**Common Causes:**

- Waiting for I/O from failed disk
- NFS mount issue
- Hardware problem

**Solutions:**

```bash
# Wait - usually resolves itself
# If persistent:

# Check for disk errors
sudo dmesg | grep -i error

# Check I/O wait
iostat 1 10

# May require reboot if hardware issue
sudo reboot
```

---

## Network Problems

### Problem: "Network is unreachable"

**Symptoms:**

```bash
$ ping google.com
connect: Network is unreachable
```

**Diagnosis:**

```bash
# Check network interface
ip link show

# Check IP address
ip addr show

# Check routing table
ip route show

# Check if interface is up
ip link show | grep state
```

**Solutions:**

```bash
# Bring interface up
sudo ip link set eth0 up

# Get IP from DHCP
sudo dhclient eth0

# Or set static IP
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip route add default via 192.168.1.1

# Restart networking
sudo systemctl restart networking
```

---

### Problem: Can ping IP but not domain names

**Symptoms:**

```bash
$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=118 time=10.2 ms

$ ping google.com
ping: google.com: Temporary failure in name resolution
```

**Common Cause:** DNS not configured

**Diagnosis:**

```bash
# Check DNS configuration
cat /etc/resolv.conf

# Test DNS manually
nslookup google.com
dig google.com
```

**Solutions:**

```bash
# Add DNS servers
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
echo "nameserver 8.8.4.4" | sudo tee -a /etc/resolv.conf

# Or edit directly
sudo nano /etc/resolv.conf

# For permanent fix (Ubuntu/Debian)
sudo nano /etc/systemd/resolved.conf
# Add: DNS=8.8.8.8 8.8.4.4
sudo systemctl restart systemd-resolved
```

---

### Problem: SSH connection refused

**Symptoms:**

```bash
$ ssh user@server
ssh: connect to host server port 22: Connection refused
```

**Diagnosis:**

```bash
# Check if SSH is running
sudo systemctl status sshd

# Check if port 22 is listening
sudo ss -tulpn | grep :22

# Check firewall
sudo ufw status
sudo iptables -L | grep 22
```

**Solutions:**

```bash
# Start SSH service
sudo systemctl start sshd
sudo systemctl enable sshd

# Check configuration
sudo nano /etc/ssh/sshd_config
# Ensure: Port 22

# Restart SSH
sudo systemctl restart sshd

# Allow through firewall
sudo ufw allow 22/tcp
```

---

### Problem: Slow network / High latency

**Symptoms:**
Very slow response times, timeouts

**Diagnosis:**

```bash
# Check latency
ping -c 10 gateway
ping -c 10 8.8.8.8

# Check for packet loss
ping -c 100 gateway | grep loss

# Check interface errors
ip -s link show eth0

# Check DNS response
time nslookup google.com

# Monitor bandwidth
iftop -i eth0
```

**Solutions:**

```bash
# Restart network interface
sudo ip link set eth0 down
sudo ip link set eth0 up

# Restart networking
sudo systemctl restart networking

# Check MTU settings
ip link show eth0

# Try different DNS servers
sudo nano /etc/resolv.conf
# Add: nameserver 1.1.1.1

# Check for hardware issues
sudo ethtool eth0
```

---

### Problem: "No route to host"

**Symptoms:**

```bash
$ ssh user@192.168.1.100
ssh: connect to host 192.168.1.100 port 22: No route to host
```

**Diagnosis:**

```bash
# Check if host is reachable
ping 192.168.1.100

# Check routing table
ip route show

# Check if on same network
ip addr show
```

**Solutions:**

```bash
# Add route if needed
sudo ip route add 192.168.1.0/24 via 192.168.1.1

# Check firewall on remote host
# May need to access physically or through console

# Verify gateway
ip route show | grep default
```

---

## Disk and Filesystem Issues

### Problem: "No space left on device"

**Symptoms:**

```bash
$ touch newfile
touch: cannot touch 'newfile': No space left on device
```

**Diagnosis:**

```bash
# Check disk space
df -h

# Find large files
du -sh /* | sort -rh | head -10
find / -type f -size +100M 2>/dev/null

# Check inodes (can also run out)
df -i
```

**Solutions:**

```bash
# Clean package cache (Debian/Ubuntu)
sudo apt clean
sudo apt autoremove

# Clean logs
sudo journalctl --vacuum-time=7d
sudo find /var/log -name "*.log" -type f -mtime +30 -delete

# Remove old kernels (Ubuntu)
sudo apt autoremove --purge

# Find and remove large files
find /home -type f -size +1G -exec ls -lh {} \;

# Empty trash
rm -rf ~/.local/share/Trash/*
```

---

### Problem: Filesystem is read-only

**Symptoms:**

```bash
$ touch test
touch: cannot touch 'test': Read-only file system
```

**Common Cause:** Filesystem errors detected, mounted read-only for protection

**Diagnosis:**

```bash
# Check mount status
mount | grep "read-only"

# Check for errors
sudo dmesg | grep -i error
```

**Solutions:**

```bash
# Remount as read-write
sudo mount -o remount,rw /

# If that fails, filesystem needs repair
# Boot from live USB and run:
sudo fsck /dev/sda1

# Or in single-user mode
sudo fsck -f /dev/sda1
```

---

### Problem: Cannot unmount busy filesystem

**Symptoms:**

```bash
$ sudo umount /mnt/disk
umount: /mnt/disk: target is busy
```

**Diagnosis:**

```bash
# Find what's using it
lsof +D /mnt/disk
fuser -mv /mnt/disk
```

**Solutions:**

```bash
# Kill processes using the mount
sudo fuser -km /mnt/disk

# Then unmount
sudo umount /mnt/disk

# Force unmount (last resort)
sudo umount -l /mnt/disk  # Lazy unmount
sudo umount -f /mnt/disk  # Force unmount
```

---

### Problem: Disk errors / Bad sectors

**Symptoms:**

- Slow disk performance
- I/O errors in logs
- System freezes

**Diagnosis:**

```bash
# Check system logs
sudo dmesg | grep -i error
sudo tail -100 /var/log/syslog | grep -i error

# Check SMART status
sudo apt install smartmontools
sudo smartctl -a /dev/sda

# Run disk check
sudo badblocks -v /dev/sda
```

**Solutions:**

```bash
# Backup data immediately!

# Check filesystem (unmount first!)
sudo umount /dev/sda1
sudo fsck -f /dev/sda1

# Mark bad blocks
sudo fsck -c /dev/sda1

# Consider replacing disk if many errors
```

---

## Package Management Problems

### Problem: "Unable to locate package"

**Symptoms:**

```bash
$ sudo apt install package-name
Reading package lists... Done
E: Unable to locate package package-name
```

**Diagnosis:**

```bash
# Check if repositories are updated
sudo apt update

# Search for package
apt search package-name

# Check enabled repositories
cat /etc/apt/sources.list
ls /etc/apt/sources.list.d/
```

**Solutions:**

```bash
# Update package lists
sudo apt update

# Enable universe/multiverse repos (Ubuntu)
sudo add-apt-repository universe
sudo add-apt-repository multiverse
sudo apt update

# Check package name spelling
apt search partial-name
```

---

### Problem: Broken packages / Dependency issues

**Symptoms:**

```bash
$ sudo apt install package
The following packages have unmet dependencies:
 package : Depends: dependency but it is not going to be installed
```

**Diagnosis:**

```bash
# Check broken packages
sudo apt --fix-broken install
dpkg -l | grep "^iU"

# Check held packages
apt-mark showhold
```

**Solutions:**

```bash
# Try to fix broken packages
sudo apt --fix-broken install
sudo dpkg --configure -a

# Clean cache
sudo apt clean
sudo apt autoclean

# Update and upgrade
sudo apt update
sudo apt upgrade

# Force install dependencies
sudo apt install -f

# Last resort - reinstall
sudo apt remove package
sudo apt autoremove
sudo apt install package
```

---

### Problem: "dpkg was interrupted"

**Symptoms:**

```bash
E: dpkg was interrupted, you must manually run 'sudo dpkg --configure -a'
```

**Solution:**

```bash
# Reconfigure packages
sudo dpkg --configure -a

# If that fails
sudo rm /var/lib/dpkg/lock
sudo rm /var/lib/dpkg/lock-frontend
sudo rm /var/cache/apt/archives/lock
sudo dpkg --configure -a
sudo apt update
```

---

## System Performance Issues

### Problem: System is very slow

**Diagnosis:**

```bash
# Check CPU usage
top
htop

# Check memory
free -h

# Check disk I/O
iostat 1 10
iotop

# Check load average
uptime

# Check running processes
ps aux --sort=-%cpu | head
ps aux --sort=-%mem | head
```

**Solutions:**

**High CPU:**

```bash
# Identify CPU hogs
top
# Press 'k' to kill process

# Nice down resource-heavy processes
renice 19 -p <PID>
```

**High Memory:**

```bash
# Free memory
sudo sync
echo 3 | sudo tee /proc/sys/vm/drop_caches

# Kill memory hogs
pkill process-name

# Add swap if needed
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

**High Disk I/O:**

```bash
# Find I/O intensive processes
sudo iotop

# Check for disk errors
sudo dmesg | grep -i error
```

---

### Problem: System freezes / Hangs

**Diagnosis:**

```bash
# Check logs after reboot
sudo journalctl -xb -1  # Previous boot
sudo dmesg | less

# Check for OOM (Out of Memory) kills
sudo grep -i "killed process" /var/log/syslog
```

**Prevention:**

```bash
# Enable Magic SysRq keys
echo 1 | sudo tee /proc/sys/kernel/sysrq

# During freeze, use: Alt+SysRq+REISUB
# R - Switch keyboard to raw mode
# E - Send SIGTERM to all processes
# I - Send SIGKILL to all processes
# S - Sync disks
# U - Unmount and remount read-only
# B - Reboot

# Add more swap
# Monitor system with:
vmstat 1
```

---

## Boot and Startup Problems

### Problem: System won't boot - Grub rescue

**Symptoms:**

```
grub rescue>
```

**Diagnosis:**

```bash
# List partitions
ls
ls (hd0,1)/
```

**Solution:**

```bash
# Find boot partition
ls (hd0,1)/boot/grub
ls (hd0,2)/boot/grub

# Set root (adjust based on findings)
set root=(hd0,1)
set prefix=(hd0,1)/boot/grub
insmod normal
normal

# After booting, reinstall grub
sudo update-grub
sudo grub-install /dev/sda
```

---

### Problem: System boots to black screen

**Diagnosis:**

- Try different TTY: Ctrl+Alt+F1 through F6

**Solutions:**

```bash
# Boot into recovery mode from GRUB
# Select "Advanced options" > "Recovery mode"

# Check display manager
sudo systemctl status gdm
sudo systemctl status lightdm

# Restart display manager
sudo systemctl restart gdm

# Check X server logs
cat /var/log/Xorg.0.log | grep -i error

# Reconfigure display
sudo dpkg-reconfigure gdm3
```

---

### Problem: Service won't start at boot

**Diagnosis:**

```bash
# Check service status
sudo systemctl status service-name

# Check if enabled
sudo systemctl is-enabled service-name

# View logs
sudo journalctl -u service-name
```

**Solutions:**

```bash
# Enable service
sudo systemctl enable service-name

# Start service
sudo systemctl start service-name

# Check dependencies
systemctl list-dependencies service-name

# Fix failed units
sudo systemctl reset-failed
```

---

## Common Error Messages

### "bash: command not found"

**Cause:** Command not installed or not in PATH

**Solutions:**

```bash
# Check if command exists
which command-name
whereis command-name

# Install if missing (example for Debian/Ubuntu)
sudo apt install package-name

# Check PATH
echo $PATH

# Add directory to PATH
export PATH=$PATH:/new/directory

# Make permanent (add to ~/.bashrc)
echo 'export PATH=$PATH:/new/directory' >> ~/.bashrc
source ~/.bashrc
```

---

### "Segmentation fault"

**Cause:** Program tried to access memory it shouldn't

**Diagnosis:**

```bash
# Run with debugging
gdb program
run

# Check core dump (if enabled)
ulimit -c unlimited
# Run program again
gdb program core
```

**Solutions:**

- Usually a program bug
- Update or reinstall the program
- Report bug to developers

---

### "Cannot allocate memory"

**Cause:** System out of RAM and swap

**Solutions:**

```bash
# Free memory
sudo sync
echo 3 | sudo tee /proc/sys/vm/drop_caches

# Add swap space
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# Make permanent
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# Kill memory-intensive processes
ps aux --sort=-%mem | head
kill <PID>
```

---

### "Too many open files"

**Cause:** Process hit file descriptor limit

**Diagnosis:**

```bash
# Check current limits
ulimit -n

# Check system-wide limits
cat /proc/sys/fs/file-nr
```

**Solutions:**

```bash
# Increase limit for current session
ulimit -n 4096

# Permanent change (edit /etc/security/limits.conf)
sudo nano /etc/security/limits.conf
# Add:
* soft nofile 4096
* hard nofile 8192

# For specific user
username soft nofile 4096
username hard nofile 8192

# Reboot or re-login
```

---

## Emergency Recovery

### Boot from Live USB

When system won't boot normally:

1. **Boot from live USB/DVD**

2. **Mount your system:**

```bash
# Find your root partition
sudo fdisk -l

# Mount it
sudo mount /dev/sda1 /mnt

# Mount other necessary filesystems
sudo mount --bind /dev /mnt/dev
sudo mount --bind /proc /mnt/proc
sudo mount --bind /sys /mnt/sys

# Chroot into your system
sudo chroot /mnt
```

3. **Now you can:**

- Fix GRUB: `grub-install /dev/sda && update-grub`
- Repair filesystem: `fsck /dev/sda1`
- Reset password: `passwd username`
- Edit config files
- Reinstall packages

---

## Troubleshooting Checklist

When facing any problem:

- [ ] Read the error message carefully
- [ ] Check logs: `/var/log/syslog`, `dmesg`, `journalctl`
- [ ] Verify file permissions
- [ ] Check disk space: `df -h`
- [ ] Check memory: `free -h`
- [ ] Check processes: `top` or `htop`
- [ ] Test with elevated privileges: `sudo`
- [ ] Search online for error message
- [ ] Check documentation: `man command`
- [ ] Ask for help with details

---

## Getting Help

**Before asking for help, gather:**

```bash
# System information
uname -a
lsb_release -a

# Error messages (exact text)

# What you've tried

# Relevant logs
sudo journalctl -xe
tail -50 /var/log/syslog
```

**Where to get help:**

- Stack Overflow
- Ask Ubuntu / Unix Stack Exchange
- Distribution forums
- IRC channels
- Reddit: r/linux4noobs, r/linuxquestions

---

## Prevention Tips

1. **Regular backups**
2. **Keep system updated**
3. **Monitor disk space**
4. **Check logs regularly**
5. **Document system changes**
6. **Test in development first**
7. **Use version control for configs**
8. **Monitor system health**

---

**Remember:** Every error is a learning opportunity. Don't panic, read carefully, and work through problems systematically!

---

[← Back to Main Guide](../README.md) | [Practice Exercises](./practice-exercises.md)
