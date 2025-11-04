Project Title:

Automated Backup System using Bash Script

Project Overview:

This project automates the process of creating and managing backups using a Bash script.
It allows you to back up a selected folder, restore data when needed, and generate daily reports — all through simple terminal commands.

Key Features:

Automated Backups: Compresses and stores selected folders into a timestamped .tar.gz file.

Dry Run Mode: Tests the backup process without actually creating files.

Restore Option: Restores any backup file to a chosen location.

Report Generation: Creates a backup summary in the reports/ folder after each run.

Error Handling: Checks for missing directories and invalid arguments.

Folder Structure:
automated-backss/
│
├── bin/           → Contains the main backup.sh script
├── backups/       → Stores all created backup files
├── restore/       → Used for restoring backups
├── reports/       → Stores daily backup reports
└── config/        → Contains configuration files (optional)

How It Works:

The user runs the script in VS Code terminal:

./bin/backup.sh /path/to/folder


The script compresses the given folder into a backup file inside backups/.

A summary report is automatically created in the reports/ folder.

If required, backups can be restored with:

./bin/backup.sh --restore <backup_file> --to <destination>

Outcome:

A fully functional local backup automation system that logs all activities, generates reports, and simplifies restoring previous data — ideal for learning automation and scripting fundamentals in DevOps.