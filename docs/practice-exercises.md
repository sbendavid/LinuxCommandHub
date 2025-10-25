# Practice Exercises

[← Back to Main Guide](../README.md)

---

## Table of Contents

- [Beginner Level Exercises](#beginner-level-exercises)
- [Intermediate Level Exercises](#intermediate-level-exercises)
- [Advanced Level Exercises](#advanced-level-exercises)
- [Real-World Scenarios](#real-world-scenarios)
- [Challenge Projects](#challenge-projects)

---

## How to Use This Guide

**Before you start:**

1. Set up a safe practice environment (VM or spare machine)
2. Don't practice destructive commands on production systems
3. Read each exercise completely before starting
4. Try to solve problems yourself before checking solutions
5. Take notes on what you learn

**Exercise format:**

- **Objective:** What you'll learn
- **Tasks:** Step-by-step instructions
- **Expected Output:** What success looks like
- **Solution:** Detailed commands (try first without looking!)

---

## Beginner Level Exercises

### Exercise 1: Basic Navigation

**Objective:** Master fundamental navigation commands

**Tasks:**

1. Find out your current directory
2. List all files including hidden ones
3. Navigate to your home directory using two different methods
4. Create a directory called "linux-practice"
5. Navigate into it
6. Go back to your previous directory
7. Go up one directory level
8. Return to linux-practice

**Expected Output:**

```bash
$ pwd
/home/username/linux-practice
$ ls -la
total 8
drwxr-xr-x  2 username username 4096 Oct 24 10:00 .
drwxr-xr-x 15 username username 4096 Oct 24 10:00 ..
```

**Solution:**

```bash
# Task 1
pwd

# Task 2
ls -la

# Task 3 (Method 1)
cd ~
# Task 3 (Method 2)
cd

# Task 4
mkdir linux-practice

# Task 5
cd linux-practice

# Task 6
cd -

# Task 7
cd ..

# Task 8
cd linux-practice
```

---

### Exercise 2: File Creation and Manipulation

**Objective:** Learn to create, copy, move, and delete files

**Tasks:**

1. Create three empty files: file1.txt, file2.txt, file3.txt
2. Create a file named "mydata.txt" with some content
3. Display the content of mydata.txt
4. Copy mydata.txt to mydata_backup.txt
5. Rename file1.txt to document1.txt
6. Create a directory called "backup"
7. Move all .txt files into the backup directory
8. List the contents of backup directory
9. Delete file2.txt from backup directory
10. Remove the backup directory and all its contents

**Solution:**

```bash
# Task 1
touch file1.txt file2.txt file3.txt

# Task 2
echo "This is my practice data" > mydata.txt

# Task 3
cat mydata.txt

# Task 4
cp mydata.txt mydata_backup.txt

# Task 5
mv file1.txt document1.txt

# Task 6
mkdir backup

# Task 7
mv *.txt backup/

# Task 8
ls -l backup/

# Task 9
rm backup/file2.txt

# Task 10
rm -r backup/
```

---

### Exercise 3: Working with Text

**Objective:** Practice text viewing and searching

**Tasks:**

1. Create a file with 20 lines of text (use any method)
2. Display the first 5 lines
3. Display the last 5 lines
4. Search for a specific word in the file
5. Count the number of lines, words, and characters
6. Sort the lines alphabetically
7. Remove duplicate lines

**Solution:**

```bash
# Task 1
for i in {1..20}; do echo "This is line $i" >> sample.txt; done

# Task 2
head -n 5 sample.txt

# Task 3
tail -n 5 sample.txt

# Task 4
grep "line 5" sample.txt

# Task 5
wc sample.txt

# Task 6
sort sample.txt

# Task 7
sort sample.txt | uniq
```

---

### Exercise 4: Permissions Basics

**Objective:** Understand and modify file permissions

**Tasks:**

1. Create a file named "script.sh"
2. Check its current permissions
3. Make it executable for the owner
4. Change permissions to rwxr-xr-x using numeric method
5. Create a directory and set permissions to 755
6. Create a private file (owner-only access)

**Solution:**

```bash
# Task 1
touch script.sh

# Task 2
ls -l script.sh

# Task 3
chmod u+x script.sh

# Task 4
chmod 755 script.sh

# Task 5
mkdir mydir
chmod 755 mydir

# Task 6
touch private.txt
chmod 600 private.txt
```

---

## Intermediate Level Exercises

### Exercise 5: Advanced File Operations

**Objective:** Master wildcards, find, and bulk operations

**Tasks:**

1. Create 10 files: test1.txt through test10.txt
2. Create 5 files: data1.log through data5.log
3. List only .txt files
4. Copy all .log files to a new directory
5. Find all files modified in the last 24 hours
6. Find all .txt files larger than 1KB
7. Rename all .txt files to .bak files
8. Delete all .log files

**Solution:**

```bash
# Task 1
touch test{1..10}.txt

# Task 2
touch data{1..5}.log

# Task 3
ls *.txt

# Task 4
mkdir logs
cp *.log logs/

# Task 5
find . -type f -mtime -1

# Task 6
find . -name "*.txt" -size +1k

# Task 7
for file in *.txt; do mv "$file" "${file%.txt}.bak"; done

# Task 8
rm *.log
```

---

### Exercise 6: Text Processing Pipeline

**Objective:** Combine multiple commands using pipes

**Tasks:**

1. Create a CSV file with sample data (name,age,city)
2. Extract only the second column (age)
3. Sort ages numerically
4. Count unique ages
5. Find the most common age
6. Create a report showing age distribution

**Solution:**

```bash
# Task 1
cat > people.csv << EOF
John,25,NYC
Jane,30,LA
Bob,25,Chicago
Alice,30,NYC
Charlie,35,Boston
EOF

# Task 2
cut -d',' -f2 people.csv

# Task 3
cut -d',' -f2 people.csv | tail -n +2 | sort -n

# Task 4
cut -d',' -f2 people.csv | tail -n +2 | sort | uniq | wc -l

# Task 5
cut -d',' -f2 people.csv | tail -n +2 | sort | uniq -c | sort -rn | head -1

# Task 6
echo "Age Distribution:"
cut -d',' -f2 people.csv | tail -n +2 | sort | uniq -c
```

---

### Exercise 7: Process Management

**Objective:** Learn to monitor and control processes

**Tasks:**

1. Start a long-running process (sleep 300)
2. Check if it's running
3. Find its PID
4. Suspend it with Ctrl+Z
5. Resume it in the background
6. List all background jobs
7. Bring it to the foreground
8. Terminate it gracefully
9. Start another process and kill it forcefully

**Solution:**

```bash
# Task 1
sleep 300

# Task 2 (in another terminal)
ps aux | grep sleep

# Task 3
pgrep sleep

# Task 4 (in the terminal running sleep)
# Press Ctrl+Z

# Task 5
bg

# Task 6
jobs

# Task 7
fg

# Task 8 (after bringing to foreground)
# Press Ctrl+C
# OR get PID and:
kill $(pgrep sleep)

# Task 9
sleep 300 &
kill -9 $(pgrep sleep)
```

---

### Exercise 8: User and Group Management

**Objective:** Practice user administration

**Tasks:**

1. Create a new user "testuser"
2. Set a password for testuser
3. Create a group "developers"
4. Add testuser to developers group
5. Create a shared directory for developers
6. Set appropriate permissions so all developers can read/write
7. Switch to testuser and verify access
8. Remove testuser from developers group
9. Delete testuser

**Solution:**

```bash
# Task 1
sudo adduser testuser

# Task 2
sudo passwd testuser

# Task 3
sudo groupadd developers

# Task 4
sudo usermod -aG developers testuser

# Task 5
sudo mkdir /shared/dev
sudo chgrp developers /shared/dev

# Task 6
sudo chmod 775 /shared/dev

# Task 7
su - testuser
cd /shared/dev
touch test.txt
exit

# Task 8
sudo gpasswd -d testuser developers

# Task 9
sudo userdel -r testuser
```

---

### Exercise 9: Shell Scripting Basics

**Objective:** Create your first shell scripts

**Tasks:**

1. Create a script that prints "Hello, World!"
2. Make it executable and run it
3. Create a script that accepts a name as argument and greets the user
4. Create a backup script that compresses a directory
5. Add error checking to your backup script

**Solution:**

```bash
# Task 1
cat > hello.sh << 'EOF'
#!/bin/bash
echo "Hello, World!"
EOF

# Task 2
chmod +x hello.sh
./hello.sh

# Task 3
cat > greet.sh << 'EOF'
#!/bin/bash
if [ -z "$1" ]; then
    echo "Usage: $0 <name>"
    exit 1
fi
echo "Hello, $1!"
EOF
chmod +x greet.sh
./greet.sh John

# Task 4
cat > backup.sh << 'EOF'
#!/bin/bash
DIR_TO_BACKUP="$1"
BACKUP_NAME="backup-$(date +%Y%m%d-%H%M%S).tar.gz"
tar czf "$BACKUP_NAME" "$DIR_TO_BACKUP"
echo "Backup created: $BACKUP_NAME"
EOF
chmod +x backup.sh

# Task 5
cat > backup_safe.sh << 'EOF'
#!/bin/bash
if [ -z "$1" ]; then
    echo "Error: No directory specified"
    echo "Usage: $0 <directory>"
    exit 1
fi

if [ ! -d "$1" ]; then
    echo "Error: $1 is not a directory"
    exit 1
fi

DIR_TO_BACKUP="$1"
BACKUP_DIR="$HOME/backups"
mkdir -p "$BACKUP_DIR"
BACKUP_NAME="$BACKUP_DIR/backup-$(date +%Y%m%d-%H%M%S).tar.gz"

tar czf "$BACKUP_NAME" "$DIR_TO_BACKUP" 2>/dev/null

if [ $? -eq 0 ]; then
    echo "Backup successful: $BACKUP_NAME"
else
    echo "Backup failed!"
    exit 1
fi
EOF
chmod +x backup_safe.sh
```

---

## Advanced Level Exercises

### Exercise 10: System Monitoring Dashboard

**Objective:** Create a comprehensive system monitoring script

**Tasks:**

1. Create a script that displays:
   - System uptime
   - CPU usage
   - Memory usage
   - Disk usage
   - Top 5 processes by CPU
   - Top 5 processes by memory
2. Format the output nicely
3. Add color coding (green=good, yellow=warning, red=critical)
4. Make it refresh every 5 seconds

**Solution:**

```bash
cat > monitor.sh << 'EOF'
#!/bin/bash

# Colors
RED='\033[0;31m'
YELLOW='\033[1;33m'
GREEN='\033[0;32m'
NC='\033[0m' # No Color

while true; do
    clear
    echo "======================================"
    echo "       SYSTEM MONITOR"
    echo "======================================"
    echo ""

    # Uptime
    echo "Uptime:"
    uptime
    echo ""

    # CPU Usage
    CPU=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | cut -d'%' -f1)
    echo -n "CPU Usage: "
    if (( $(echo "$CPU < 50" | bc -l) )); then
        echo -e "${GREEN}${CPU}%${NC}"
    elif (( $(echo "$CPU < 80" | bc -l) )); then
        echo -e "${YELLOW}${CPU}%${NC}"
    else
        echo -e "${RED}${CPU}%${NC}"
    fi
    echo ""

    # Memory Usage
    echo "Memory Usage:"
    free -h
    echo ""

    # Disk Usage
    echo "Disk Usage:"
    df -h / | tail -1
    echo ""

    # Top 5 CPU processes
    echo "Top 5 CPU Processes:"
    ps aux --sort=-%cpu | head -6 | tail -5
    echo ""

    # Top 5 Memory processes
    echo "Top 5 Memory Processes:"
    ps aux --sort=-%mem | head -6 | tail -5
    echo ""

    echo "Refreshing in 5 seconds... (Ctrl+C to exit)"
    sleep 5
done
EOF
chmod +x monitor.sh
```

---

### Exercise 11: Log Analysis

**Objective:** Parse and analyze system logs

**Tasks:**

1. Find all failed SSH login attempts
2. Extract and count unique IP addresses
3. Find the most common failed login attempts
4. Create a report showing:
   - Total failed attempts
   - Top 5 attacking IPs
   - Time distribution of attacks
5. Export results to a CSV file

**Solution:**

```bash
cat > analyze_ssh.sh << 'EOF'
#!/bin/bash

LOG_FILE="/var/log/auth.log"
REPORT_FILE="ssh_analysis_$(date +%Y%m%d).csv"

echo "Analyzing SSH login attempts..."

# Total failed attempts
TOTAL_FAILED=$(grep "Failed password" "$LOG_FILE" | wc -l)

# Extract IPs
grep "Failed password" "$LOG_FILE" | \
    grep -oE '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' | \
    sort | uniq -c | sort -rn > /tmp/ip_count.txt

# Top 5 IPs
echo "Top 5 Attacking IPs:"
head -5 /tmp/ip_count.txt

# Create CSV report
echo "Count,IP Address" > "$REPORT_FILE"
head -10 /tmp/ip_count.txt | awk '{print $1","$2}' >> "$REPORT_FILE"

echo ""
echo "Total Failed Attempts: $TOTAL_FAILED"
echo "Report saved to: $REPORT_FILE"

# Cleanup
rm /tmp/ip_count.txt
EOF
chmod +x analyze_ssh.sh
sudo ./analyze_ssh.sh
```

---

### Exercise 12: Automated Backup System

**Objective:** Create a comprehensive backup solution

**Tasks:**

1. Create a backup script that:
   - Backs up specified directories
   - Uses date-based filenames
   - Keeps only last 7 backups
   - Logs all operations
   - Sends email notification (optional)
2. Set up a cron job to run daily at 2 AM
3. Test the backup and restore process

**Solution:**

```bash
cat > backup_system.sh << 'EOF'
#!/bin/bash

# Configuration
BACKUP_SOURCES="/home/user/documents /home/user/projects"
BACKUP_DEST="/backup"
LOG_FILE="/var/log/backup.log"
RETENTION_DAYS=7

# Function to log messages
log_message() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" | tee -a "$LOG_FILE"
}

# Create backup directory if it doesn't exist
mkdir -p "$BACKUP_DEST"

# Generate backup filename
BACKUP_FILE="$BACKUP_DEST/backup-$(date +%Y%m%d-%H%M%S).tar.gz"

log_message "Starting backup..."

# Perform backup
if tar czf "$BACKUP_FILE" $BACKUP_SOURCES 2>>/tmp/backup_errors.log; then
    log_message "Backup successful: $BACKUP_FILE"

    # Calculate size
    SIZE=$(du -h "$BACKUP_FILE" | cut -f1)
    log_message "Backup size: $SIZE"
else
    log_message "ERROR: Backup failed!"
    exit 1
fi

# Remove old backups
log_message "Removing backups older than $RETENTION_DAYS days..."
find "$BACKUP_DEST" -name "backup-*.tar.gz" -type f -mtime +$RETENTION_DAYS -delete

# Count remaining backups
BACKUP_COUNT=$(ls -1 "$BACKUP_DEST"/backup-*.tar.gz 2>/dev/null | wc -l)
log_message "Total backups: $BACKUP_COUNT"

log_message "Backup completed successfully"

# Optional: Send email notification
# echo "Backup completed: $BACKUP_FILE" | mail -s "Backup Report" user@example.com
EOF

chmod +x backup_system.sh

# Set up cron job
(crontab -l 2>/dev/null; echo "0 2 * * * /path/to/backup_system.sh") | crontab -

# Test restore
cat > restore_backup.sh << 'EOF'
#!/bin/bash

if [ -z "$1" ]; then
    echo "Usage: $0 <backup_file>"
    echo "Available backups:"
    ls -lh /backup/backup-*.tar.gz
    exit 1
fi

RESTORE_DIR="/tmp/restore-$(date +%Y%m%d-%H%M%S)"
mkdir -p "$RESTORE_DIR"

echo "Restoring $1 to $RESTORE_DIR..."
tar xzf "$1" -C "$RESTORE_DIR"

if [ $? -eq 0 ]; then
    echo "Restore successful!"
    echo "Files restored to: $RESTORE_DIR"
else
    echo "Restore failed!"
    exit 1
fi
EOF
chmod +x restore_backup.sh
```

---

### Exercise 13: Network Port Scanner

**Objective:** Create a simple port scanning script

**Tasks:**

1. Create a script that scans common ports on a target
2. Display open ports
3. Identify services running on open ports
4. Create a summary report

**Solution:**

```bash
cat > portscan.sh << 'EOF'
#!/bin/bash

if [ -z "$1" ]; then
    echo "Usage: $0 <target_ip>"
    exit 1
fi

TARGET="$1"
COMMON_PORTS="20 21 22 23 25 53 80 110 143 443 3306 3389 5432 8080"

echo "Scanning $TARGET for open ports..."
echo "=================================="

OPEN_PORTS=()

for PORT in $COMMON_PORTS; do
    timeout 1 bash -c "echo >/dev/tcp/$TARGET/$PORT" 2>/dev/null
    if [ $? -eq 0 ]; then
        echo "Port $PORT: OPEN"
        OPEN_PORTS+=($PORT)

        # Identify service
        case $PORT in
            20|21) SERVICE="FTP" ;;
            22) SERVICE="SSH" ;;
            23) SERVICE="Telnet" ;;
            25) SERVICE="SMTP" ;;
            53) SERVICE="DNS" ;;
            80) SERVICE="HTTP" ;;
            110) SERVICE="POP3" ;;
            143) SERVICE="IMAP" ;;
            443) SERVICE="HTTPS" ;;
            3306) SERVICE="MySQL" ;;
            3389) SERVICE="RDP" ;;
            5432) SERVICE="PostgreSQL" ;;
            8080) SERVICE="HTTP-Alt" ;;
            *) SERVICE="Unknown" ;;
        esac
        echo "  └─ Service: $SERVICE"
    fi
done

echo ""
echo "Summary:"
echo "--------"
echo "Total open ports: ${#OPEN_PORTS[@]}"
if [ ${#OPEN_PORTS[@]} -gt 0 ]; then
    echo "Open ports: ${OPEN_PORTS[*]}"
fi
EOF
chmod +x portscan.sh
```

---

## Real-World Scenarios

### Scenario 1: Web Server Setup

**Objective:** Set up a complete web server environment

**Requirements:**

1. Install Apache web server
2. Create a website directory structure
3. Set appropriate permissions
4. Create a simple HTML page
5. Configure a virtual host
6. Test the website

**Solution:**

```bash
# Install Apache
sudo apt update
sudo apt install apache2 -y

# Create directory structure
sudo mkdir -p /var/www/mysite.com/public_html
sudo mkdir -p /var/www/mysite.com/logs

# Set ownership
sudo chown -R $USER:$USER /var/www/mysite.com/public_html

# Set permissions
sudo chmod -R 755 /var/www

# Create index page
cat > /var/www/mysite.com/public_html/index.html << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>Welcome to My Site</title>
</head>
<body>
    <h1>Success! The server is working!</h1>
    <p>This is a test page.</p>
</body>
</html>
EOF

# Create virtual host
sudo cat > /etc/apache2/sites-available/mysite.com.conf << 'EOF'
<VirtualHost *:80>
    ServerAdmin admin@mysite.com
    ServerName mysite.com
    ServerAlias www.mysite.com
    DocumentRoot /var/www/mysite.com/public_html
    ErrorLog /var/www/mysite.com/logs/error.log
    CustomLog /var/www/mysite.com/logs/access.log combined
</VirtualHost>
EOF

# Enable site
sudo a2ensite mysite.com.conf
sudo systemctl reload apache2

# Test
curl http://localhost
```

---

### Scenario 2: Database Backup Automation

**Objective:** Create automated MySQL database backups

**Requirements:**

1. Create backup script for MySQL databases
2. Compress backups
3. Rotate old backups
4. Schedule daily backups
5. Send notification on failure

**Solution:**

```bash
cat > mysql_backup.sh << 'EOF'
#!/bin/bash

# Configuration
DB_USER="backup_user"
DB_PASS="secure_password"
BACKUP_DIR="/backup/mysql"
RETENTION_DAYS=30
LOG_FILE="/var/log/mysql_backup.log"
ADMIN_EMAIL="admin@example.com"

# Create backup directory
mkdir -p "$BACKUP_DIR"

# Log function
log() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" >> "$LOG_FILE"
}

# Get list of databases
DATABASES=$(mysql -u"$DB_USER" -p"$DB_PASS" -e "SHOW DATABASES;" | grep -Ev "(Database|information_schema|performance_schema|mysql|sys)")

log "Starting MySQL backup"

for DB in $DATABASES; do
    BACKUP_FILE="$BACKUP_DIR/${DB}-$(date +%Y%m%d-%H%M%S).sql.gz"

    log "Backing up database: $DB"

    mysqldump -u"$DB_USER" -p"$DB_PASS" "$DB" | gzip > "$BACKUP_FILE"

    if [ $? -eq 0 ]; then
        SIZE=$(du -h "$BACKUP_FILE" | cut -f1)
        log "Backup successful: $DB ($SIZE)"
    else
        log "ERROR: Backup failed for $DB"
        echo "MySQL backup failed for database: $DB" | mail -s "Backup Failure" "$ADMIN_EMAIL"
    fi
done

# Remove old backups
find "$BACKUP_DIR" -name "*.sql.gz" -type f -mtime +$RETENTION_DAYS -delete
log "Removed backups older than $RETENTION_DAYS days"

log "Backup completed"
EOF

chmod +x mysql_backup.sh

# Add to crontab (daily at 3 AM)
(crontab -l 2>/dev/null; echo "0 3 * * * /path/to/mysql_backup.sh") | crontab -
```

---

## Challenge Projects

### Project 1: System Health Monitor

Create a comprehensive system health monitoring tool that:

- Checks CPU, memory, disk usage
- Monitors critical services
- Sends alerts when thresholds are exceeded
- Logs all checks
- Generates daily reports

### Project 2: User Management System

Build a user management system that:

- Creates users from a CSV file
- Sets up home directories
- Assigns to appropriate groups
- Generates random passwords
- Sends credentials via email

### Project 3: Log Aggregator

Develop a log aggregation tool that:

- Collects logs from multiple sources
- Parses and categorizes logs
- Identifies errors and warnings
- Creates summary reports
- Archives old logs

### Project 4: Automated Testing Framework

Create a testing framework for your scripts:

- Unit tests for functions
- Integration tests
- Performance tests
- Generates test reports
- CI/CD integration

---

## Tips for Success

1. **Start Small:** Begin with simple exercises and gradually increase complexity
2. **Practice Daily:** Consistency is key to mastering Linux commands
3. **Make Mistakes:** Breaking things (safely) is the best way to learn
4. **Read Documentation:** Always check `man` pages for command details
5. **Keep Notes:** Document what you learn and solutions you find
6. **Join Communities:** Share your solutions and learn from others
7. **Build Projects:** Apply what you learn to real-world problems

---

## Next Steps

After completing these exercises:

1. Review the [Troubleshooting Guide](./troubleshooting.md)
2. Check the [Quick Reference](./quick-reference.md)
3. Explore the main guide sections for deeper learning
4. Build your own projects
5. Contribute your exercises to help others!

---

[← Back to Main Guide](../README.md)
