# DevOpsFetch Documentation

## Overview

DevOpsFetch is a shell script tool designed to retrieve and display various system information such as active ports, Docker images and containers, Nginx domains, user login details, and system activities. The output is formatted as readable ASCII tables for better clarity.

## Installation and Configuration
The installation is performed by the installation script.

This installation script sets up the `DevOpsFetch` tool by copying the main script to the appropriate directory, creating necessary log files, configuring a systemd service and timer, and setting up log rotation. This script ensures that `DevOpsFetch` runs automatically every 5 minutes and logs its output to a specified log file.

## Prerequisites

- The script must be run as the root user or with `sudo` privileges.

## Installation Steps implemented in the script

1. **Copies the Main Script to /usr/local/bin**
   ```sh
   cp devopsfetch /usr/local/bin/devopsfetch
   chmod +x /usr/local/bin/devopsfetch
   ```

   This step copies the `devopsfetch` script to `/usr/local/bin` and makes it executable.

2. **Creates and Set Permissions for the Log File**
   ```sh
   touch /var/log/devopsfetch.log
   chmod 666 /var/log/devopsfetch.log
   ```

   This step creates the log file at `/var/log/devopsfetch.log` and sets the permissions to allow all users to write to the log file.

3. **Creates systemd Service File**
   ```sh
   cat << EOF > /etc/systemd/system/devopsfetch.service
   [Unit]
   Description=DevOpsFetch Monitoring Service
   After=network.target

   [Service]
   ExecStart=/bin/bash -c '/usr/local/bin/devopsfetch -t "$(date -d \"5 minutes ago\" +\"%Y-%m-%d %H:%M:%S\")" "$(date +\"%Y-%m-%d %H:%M:%S\")"'
   Restart=always
   User=root

   [Install]
   WantedBy=multi-user.target
   EOF
   ```

   This step creates a systemd service file that runs the `devopsfetch` script every 5 minutes, capturing activities within that timeframe. The service is configured to restart automatically.

4. **Creates systemd Timer File**
   ```sh
   cat << EOF > /etc/systemd/system/devopsfetch.timer
   [Unit]
   Description=Run DevOpsFetch every 5 minutes

   [Timer]
   OnBootSec=5min
   OnUnitActiveSec=5min
   Persistent=true

   [Install]
   WantedBy=timers.target
   EOF
   ```

   This step creates a systemd timer file that schedules the `devopsfetch` service to run every 5 minutes after booting and every 5 minutes thereafter.

5. **Reloads systemd, Enable and Start the Service and Timer**
   ```sh
   systemctl daemon-reload
   systemctl enable devopsfetch.service
   systemctl enable devopsfetch.timer
   systemctl start devopsfetch.timer
   ```

   This step reloads the systemd manager configuration, enables the `devopsfetch` service and timer, and starts the timer.

6. **Sets Up Log Rotation**
   ```sh
   cat << EOF > /etc/logrotate.d/devopsfetch
   /var/log/devopsfetch.log {
       hourly
       rotate 288
       compress
       missingok
       notifempty
       create 666 root root
   }
   EOF
   ```

   This step sets up log rotation for the `devopsfetch` log file. The logs are rotated hourly, with up to 288 rotations kept (equivalent to 12 days of logs), and old logs are compressed.

7. **Completion Message**
   ```sh
   echo "DevOpsFetch has been installed and configured."

   ```

   This step prints a confirmation message indicating that `DevOpsFetch` has been successfully installed and configured.
   
By following these steps, you will have successfully installed and configured the `DevOpsFetch` tool. The tool will run every 5 minutes, logging its activities to `/var/log/devopsfetch.log`, and the log files will be managed through log rotation.

## Usage Examples

The script can be invoked with various command-line flags to retrieve different types of information:

1. **Active Ports and Services**
   - List all active ports and their associated services:
     ```sh
     devopsfetch -p
     ```
   - Get information for a specific port:
     ```sh
     devopsfetch -p 80
     ```

2. **Docker Images and Containers**
   - List all Docker images and containers:
     ```sh
     devopsfetch -d
     ```
   - Get detailed information for a specific container:
     ```sh
     devopsfetch -d container_name
     ```

3. **Nginx Domains and Configuration**
   - List all Nginx domains and their listening ports:
     ```sh
     devopsfetch -n
     ```
   - Get configuration details for a specific domain:
     ```sh
     devopsfetch -n abasifreke.hng
     ```

4. **Users and Last Login Times**
   - List all users and their last login times:
     ```sh
     devopsfetch -u
     ```
   - Get detailed information for a specific user:
     ```sh
     devopsfetch -u abas
     ```

5. **System Activities**
   - Display activities on a specific date:
     ```sh
     devopsfetch -t '2024-07-03'
     ```
   - Display activities between specific dates:
     ```sh
     devopsfetch -t '2024-07-03 00:00:00' '2024-07-03 23:59:59'
     ```
 
6. **Help**
   - Display help message:
     ```sh
     devopsfetch -h
     ```

## Logging Mechanism

DevOpsFetch logs its output to `/var/log/devopsfetch/devopsfetch.log`. Each command execution appends its output to the log file.

### Retrieve Logs
You can view the logs using standard file viewing commands:

- Display the last few lines of the log file:
  ```sh
  tail -n 50 /var/log/devopsfetch/devopsfetch.log
  ```

- View the entire log file:
  ```sh
  cat /var/log/devopsfetch/devopsfetch.log
  ```

- Follow the log file in real-time:
  ```sh
  tail -f /var/log/devopsfetch/devopsfetch.log
  ```
