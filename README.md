# DevOpsFetch Documentation

## Overview

DevOpsFetch is a shell script tool designed to retrieve and display various system information such as active ports, Docker images and containers, Nginx domains, user login details, and system activities. The output is formatted as readable ASCII tables for better clarity.

## Installation and Configuration

1. **Clone the Repository**
   ```sh
   git clone <repository_url>
   cd <repository_directory>
   ```

2. **Make the Script Executable**
   ```sh
   chmod +x devopsfetch
   ```

3. **Move the Script to a Directory in Your PATH**
   ```sh
   sudo mv devopsfetch /usr/local/bin/devopsfetch
   ```

4. **Set Up Log Directory and Permissions**
   ```sh
   sudo mkdir -p /var/log/devopsfetch
   sudo touch /var/log/devopsfetch/devopsfetch.log
   sudo chown $(whoami) /var/log/devopsfetch/devopsfetch.log
   ```

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
