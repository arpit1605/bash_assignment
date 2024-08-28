 **************** Bash Assignment ******************

 1. Create a Directory Structure:
Write a script that creates the following directory structure:
   /home/user/
       ├── projects/
       │   ├── project1/
       │   ├── project2/
       │   └── project3/
       ├── documents/
       └── downloads/

Bash script to Create a Directory Structure:
#!/bin/bash

# Set the base directory:
DIR="/home/user"

# Create the directory structure:
mkdir "$DIR/projects"
mkdir "$DIR/projects/project"{1,2,3}
mkdir "$DIR/documents"
mkdir "$DIR/downloads"
echo "The directory structure has been created successfully."

Save the script: createDirectoryStructure.sh
Make the script executable: sudo chmod +x createDirectoryStructure.sh
Execute the Bash script: sudo ./createDirectoryStructure.sh



2. File Backup:
Write a script that takes a directory as an argument and creates a backup of all `.txt` files in that directory. The backup files should be copied to a new directory named `backup` with a timestamp.

Bash script to take the backup of the files:
#!/bin/bash

# Retrieve the directory provided as an argument:
SRC_DIR="$1"

# Check whether the source directory exists or not:
if [ ! -d "$SRC_DIR" ]; then
    echo "The source directory $SRC_DIR does not exist!"
    exit 1
fi

# Create the backup directory with a timestamp:
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
BKP_DIR="${SRC_DIR}/backup_${TIMESTAMP}"
mkdir "$BKP_DIR"

# Copy all the .txt files to the backup directory:
cp "$SRC_DIR"/*.txt "$BKP_DIR"

# Check whether the copy operation was successful or not:
if [ $? -eq 0 ]; then
    echo "The backup process is completed and placed at the location: $BKP_DIR"
else
    echo "The backup process failed!"
    exit 1
fi

Save the script: fileBackup.sh
Make the script executable: sudo chmod +x fileBackup.sh
Execute the Bash script: sudo ./fileBackup.sh /path/to/your/directory



3. User Information:
Write a script that displays the following information about the user:
   - Username
   - User ID
   - Group ID
   - Home Directory
   - Shell being used

Bash script to display the User Information:
#!/bin/bash

# Function to display the user information:
displayUserInfo() {
    echo "Username: $(whoami)"
    echo "User ID: $(id -u)"
    echo "Group ID: $(id -g)"
    echo "Home Directory: $HOME"
    echo "Shell: $SHELL"
}

# Call the displayUserInfo function to display the user information:
displayUserInfo

Save the script: displayUserInfo.sh
Make the script executable: sudo chmod +x displayUserInfo.sh
Execute the Bash script: sudo ./displayUserInfo.sh



4. Disk Usage Alert:
Write a script that checks the disk usage of the root filesystem. If the disk usage is above 80%, the script should send an email alert to the system administrator.

Bash script to check the disk usage and send an alert:
#!/bin/bash

# Set the threshold for disk usage to 80%:
THRESHOLD_VAL=80

# Check disk usage of the root filesystem:
DISK_USAGE=$(df / | grep / | awk '{ print $5 }' | sed 's/%//g')

# Check if usage is above the threshold_value:
if [ "$DISK_USAGE" -gt "$THRESHOLD_VAL" ]; then
    # Replace with the actual email address:
    RECIPIENT_EMAIL="example@gmail.com"
    SUBJECT="Disk Usage Alert: / is at ${DISK_USAGE}%"
    BODY="Warning: The disk usage of the root filesystem (/) is at ${DISK_USAGE}%. Please take necessary steps to free space."
    # Send an alert via email:
    echo "$BODY" | mail -s "$SUBJECT" "$RECIPIENT_EMAIL"
fi

Save the script: diskUsageAlert.sh
Make the script executable: sudo chmod +x diskUsageAlert.sh
Execute the Bash script: sudo ./diskUsageAlert.sh



5. File Permission Checker:
Write a script that takes a file as an argument and checks if the file has read, write, and execute permissions. The script should display appropriate messages for each permission.

Bash script to check the file permissions:
#!/bin/bash

file=$1

# Check whether the file exists or not:
if [ -f "$FILE" ]; then
    echo "File does not exist."
    exit 1
fi

# Check the read permissions:
if [ -r "$file" ]; then
    echo "The file has read permission."
else
    echo "The file does not have read permission."
fi

# Check the write permissions:
if [ -w "$file" ]; then
    echo "The file has write permission."
else
    echo "The file does not have write permission."
fi

# Check the execute permissions:
if [ -x "$file" ]; then
    echo "The file has execute permission."
else
    echo "The file does not have execute permission."
fi

Save the script: filePermissionChecker.sh
Make the script executable: sudo chmod +x filePermissionChecker.sh
Execute the Bash script: sudo ./filePermissionChecker.sh /path/to/your/file



6. Automated Backup:
Write a script that compresses the `/home/user/documents` directory into a tarball named `documents_backup.tar.gz` and moves it to the `/home/user/backup` directory. This script should be scheduled to run daily using cron.

Bash script to perform an automated backup:
#!/bin/bash

# Define the source and destination directories:
SRC_DIR="/home/user/documents"
BKP_DIR="/home/user/backup"
# Define the backup file name:
BKP_FILE="documents_backup.tar.gz"

# Create a backup directory if it doesn't exist:
mkdir "$BKP_DIR"

# Compress the source directory into a tarball:
tar -cvzf "$BKP_FILE" -C "$SRC_DIR" .

# Move the backup file into the backup directory:
mv "$BKP_FILE" "$BKP_DIR"

# Check if the tarball was created successfully:
if [ $? -eq 0 ]; then
    echo "The backup process is compeleted and the backup file is placed at: $BKP_DIR/$BKP_FILE"
else
    echo "The backup process has failed!"
    exit 1
fi


Save the script: automatedBackup.sh
Make the script executable: sudo chmod +x automatedBackup.sh
Schedule the script to run daily: 
crontab -e
Add below lines to the crontab file to run the script every day at 12:00 AM:
0 0 * * * /path/to/automatedBackup.sh
Verify the script is listed correctly in the crontab:
crontab -l



7. Process Monitor:
Write a script that checks if a specific process (e.g., `apache2`) is running. If the process is not running, the script should start the process and log the action to a file.

Bash script to monitor the process:
#!/bin/bash

# Seting the process name and log file variables:
PROCESS_NAME="apache2"
LOG_FILE="/var/log/process_monitor.log"

# Check whether the process is running or not:
if pgrep "$PROCESS_NAME" > /dev/null; then
    echo "$(date): The process $PROCESS_NAME is running." >> "$LOG_FILE"
else
    echo "$(date): The process $PROCESS_NAME is not running."
    echo "Starting the process $PROCESS_NAME." >> "$LOG_FILE"
    # Starting the process:
    systemctl start "$PROCESS_NAME"
    # Check whether the process started successfully or not:
    if [ $? -eq 0 ]; then
        echo "$(date): The process $PROCESS_NAME started successfully." >> "$LOG_FILE"
    else
        echo "$(date): Failed to start the process: $PROCESS_NAME." >> "$LOG_FILE"
    fi
fi

Save the script: processMonitor.sh
Make the script executable: sudo chmod +x processMonitor.sh
Execute the Bash script: sudo ./processMonitor.sh



8. Text File Analysis:
Write a script that takes a text file as an argument and counts the number of lines, words, and characters in the file. The script should display the counts.

Bash script to analyze the text file:
#!/bin/bash

file=$1

# Check whether the file exists or not:
if [ ! -e "$file" ]; then
    echo "The input file does not exists!"
    exit 1
fi

# Count the number of lines, words, and characters:
lineCount=$(wc -l < "$file")
wordCount=$(wc -w < "$file")
characterCount=$(wc -m < "$file")

# Display the file count statistics:
echo "***** File Statistics *****"
echo "No of Lines       : $lineCount"
echo "No of Words       : $wordCount"
echo "No of Characters  : $characterCount"

Save the script: textFileAnalysis.sh
Make the script executable: sudo chmod +x textFileAnalysis.sh
Execute the Bash script: sudo ./textFileAnalysis.sh /path/to/your/file



9. System Information Report:
Write a script that generates a system information report. The report should include:
   - System uptime
   - Memory usage
   - CPU load
   - Disk usage
   - Running processes
The report should be saved to a file named `system_report.txt`.

Bash script to generate the system information report:
#!/bin/bash

REPORT_FILE="system_report.txt"

# Get the system uptime:
UPTIME=$(uptime -p)

# Get the memory_usage usage:
MEMORY_USAGE=$(free -h)

# Get the CPU load:
CPU_USAGE=$(top -bn1 | grep "load average:" | awk '{print $10 $11 $12}' | sed 's/,//g')

# Get the disk usage:
DISK_USAGE=$(df /| grep /| awk '{print $5}')

# Get the top 15 running processes with respect to the memory usage:
PROCESSES=$(ps aux --sort=-%mem | head -n 16)

echo "*********************************************************** System Information Report **********************************************************" > $REPORT_FILE
echo " " >> $REPORT_FILE

echo "System Uptime:" >> $REPORT_FILE
echo "$UPTIME" >> $REPORT_FILE
echo " " >> $REPORT_FILE

echo "Memory Usage:" >> $REPORT_FILE
echo "$MEMORY_USAGE" >> $REPORT_FILE
echo " " >> $REPORT_FILE

echo "CPU Usage:" >> $REPORT_FILE
echo "$CPU_USAGE" >> $REPORT_FILE
echo " " >> $REPORT_FILE

echo "Disk Usage:" >> $REPORT_FILE
echo "$DISK_USAGE" >> $REPORT_FILE
echo " " >> $REPORT_FILE

echo "Top 15 Running processes (sorted by higher memory usage):" >> $REPORT_FILE
echo "$PROCESSES" >> $REPORT_FILE
echo " " >> $REPORT_FILE
echo "************************************************************************************************************************************************" >> $REPORT_FILE

# Print message to indicate completion:
echo "System report generated and saved to $REPORT_FILE"

Save the script: systemInformationReport.sh
Make the script executable: sudo chmod +x systemInformationReport.sh
Execute the Bash script: sudo ./systemInformationReport.sh



10. Simple Calculator:
Write a script that acts as a simple calculator. The script should prompt the user to enter two numbers and an operator (+, -, *, /) and then display the result of the operation.

Bash script to perform simple calculations:
#!/bin/bash

# Function to perform the user-desired operation:
calculate() {
    case $operator in
        +)
            result=$(echo "$num1 + $num2" | bc)
            ;;
        -)
            result=$(echo "$num1 - $num2" | bc)
            ;;
        \*)
            result=$(echo "$num1 * $num2" | bc)
            ;;
        /)
            if [ "$num2" -eq 0 ]; then
                echo "Divide by 0 Error!!!"
                exit 1
            else
	        result=$(echo "$num1 / $num2" | bc)
	    fi
            ;;
        *)
            echo "Invalid operator. Please use one of +, -, *, /."
            exit 1
            ;;
    esac
    echo "The resultant value is: $result"
}

# Prompt the user to input 2 numbers and the operator:
read -p "Enter the first number: " num1
read -p "Enter the second number: " num2
read -p "Enter the operation you wish to perform (+, -, *, /): " operator

# Call the calculate function:
calculate

Save the script: simpleCalculator.sh
Make the script executable: sudo chmod +x simpleCalculator.sh
Execute the Bash script: sudo ./simpleCalculator.sh



