#!/bin/bash

# MySQL database credentials
DB_USER="root"
DB_PASS="password"
DB_NAME="pg_db"

# Backup destination directory
BACKUP_DIR="backup"

# Date format for the backup file
DATE=$(date +"%Y%m%d")

# Create backup directory if it doesn't exist
mkdir -p $BACKUP_DIR

# Generate backup file name
BACKUP_FILE="$BACKUP_DIR/$DB_NAME-$DATE.sql"

# MySQL dump command
MYSQLDUMP_CMD="mysqldump --user=$DB_USER --password=$DB_PASS --databases $DB_NAME > $BACKUP_FILE"

# Execute the backup command
eval $MYSQLDUMP_CMD

# Check if the backup was successful
if [ $? -eq 0 ]; then
    echo "Database backup created successfully: $BACKUP_FILE"
    
    # Local backup file path
    LOCAL_BACKUP_FILE="$BACKUP_FILE"
    
    # Google Drive folder ID
    DRIVE_FOLDER_ID="your_folder_id"

    # Check if rclone is installed
    if ! command -v rclone &> /dev/null; then
        echo "rclone command not found. Please install rclone (https://rclone.org/downloads/) and configure it."
        exit 1
    fi

    # Upload the local backup file to Google Drive
    echo "Uploading local backup file to Google Drive..."
    rclone copy "$LOCAL_BACKUP_FILE" "drive:$DRIVE_FOLDER_ID"
else
    echo "Error creating database backup"
fi
