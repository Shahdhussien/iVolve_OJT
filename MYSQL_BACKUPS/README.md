Lab 1: Shell Scripting Basics
Shell Scripting Basics including MySQL installation, backup script creation, and setting up a cron job.

Objective

-Install MySQL database.
-Create a directory to store backups.
-Write a shell script to take a MySQL database backup using mysqldump.
-Schedule the script to run daily at 5:00 PM using cron.


1️⃣ Install MySQL Database

```
yum install mysql-server
systemctl start mysqld
systemctl enable mysqld
```

2️⃣ Create a Directory for Backups

```
mkdir -p ./mysql_backups
```

3️⃣ Create Shell Script for MySQL Backup

```
#!/bin/bash
BACKUP_DIR=~/mysql_backups
DATE=$ (date +%F)
FILENAME=MySQL_backup_${DATE}.sql

mysqldump -u root -p redhat --all-databases › "$BACKUP_DIR/${FILENAME}"
```

4️⃣ Set Up Cron Job to Run Daily at 5:00 PM

```
crontab -e
```
```
0 17 * * * ~/mysql_backup.sh
```

list the backup files

![Alt Text](./images/ls.jpg)