# System Manipulation - Study Notes

## Overview
Understanding system manipulation is essential for interacting effectively with computer systems. This includes knowing how to execute programs, manage drivers, use shells, and understand user roles.

---

## What Do We Do on Machines?

### Categories of Activities
- **Create**
  - Develop software applications
  - Write scripts
  - Design system configurations
- **Play**
  - Engage with multimedia content
  - Play games
  - Use interactive applications
- **Work**
  - Perform data analysis
  - Edit documents
  - Conduct other productivity-related tasks

### Executing Programs
- **Launching Applications**: Starting software to perform specific tasks.
- **Running Processes**: Managing ongoing operations in the background or foreground.
- **Computer's Role**: Utilizes the operating system to manage resources, schedule tasks, and execute instructions efficiently.

---

## Key Components

### Driver
- **Definition**: Software that enables communication between the operating system and hardware devices.
- **Functions**:
  - **Translation of Commands**: Converts OS instructions into device-specific commands.
  - **Communication Management**: Handles data exchange between the system and external hardware.
- **Importance**: Essential for the operation of hardware like printers, graphics cards, and network adapters.

### Shell
- **Definition**: A command-line interface for interacting with the operating system.
- **Main Features**:
  - **Command Interpretation**: Executes user-entered commands.
  - **Script Execution**: Automates repetitive tasks through scripts.
  - **Environment Management**: Manages user sessions and system settings.
- **Common Shells**:
  - **Bash (Bourne Again Shell)**: Popular in Unix-like systems.
  - **PowerShell**: Developed by Microsoft for Windows.
  - **Zsh (Z Shell)**: An advanced version of the Bourne Shell with extra features.

### User
- **Definition**: An objects attached to processes and files that interacts with the operating system.
- **Characteristics**:
  - **Ownership of Resources**: Owns processes, files, and other resources.
  - **User Accounts**: Each user has a unique account with specific permissions.
  - **Process Association**: Processes are tied to the user who initiated them.
- **Types of Users**:
  - **Administrative Users**: Have elevated privileges for system-wide changes.
  - **Standard Users**: Limited access for everyday tasks.
  - **Guest Users**: Restricted access for temporary use.

---

## Additional Key Concepts

### Processes and Threads
- **Process**
  - An instance of a running program.
  - Contains program code and current activity.
  - Isolated from other processes for stability and security.
- **Thread**
  - The smallest unit of processing within a process.
  - Allows parallel execution of tasks within the same application.

### File Systems
- **File System**
  - Manages files on storage devices.
  - Examples: NTFS, FAT32, ext4, APFS.
- **File Permissions**
  - Define access rights (read, write, execute) for users and groups.

### Inter-Process Communication (IPC)
- **IPC Mechanisms**
  - **Pipes**: Enable data flow between processes.
  - **Sockets**: Facilitate communication over networks.
  - **Shared Memory**: Allows multiple processes to access the same memory space.
  - **Message Queues**: Enable processes to send and receive messages.

### Security and Permissions
- **User Authentication**: Verifying user identities before granting access.
- **Access Control**: Managing permissions to restrict resource access.
- **Security Policies**: Rules to protect system integrity and data.

### System Calls
- **Definition**: Interface between applications and the operating system.
- **Functions**: Request services like file operations, process control, and network communication.

---

## Common Shell Commands

| Command | Description                      |
| ------- | -------------------------------- |
| `ls`    | List directory contents          |
| `cd`    | Change current directory         |
| `mkdir` | Create a new directory           |
| `rm`    | Remove files or directories      |
| `cp`    | Copy files or directories        |
| `mv`    | Move or rename files/directories |
| `grep`  | Search text using patterns       |
| `echo`  | Display text/string              |
| `chmod` | Change file permissions          |
| `ps`    | Display running processes        |
| `lsof`  | You can see                      |

### Example: Simple Shell Script

```bash:path/to/scripts/example.sh
#!/bin/bash

# This script updates the system and cleans up unnecessary files

echo "Updating system..."
sudo apt-get update && sudo apt-get upgrade -y

echo "Cleaning up..."
sudo apt-get autoremove -y
sudo apt-get clean

echo "System update and cleanup complete."
```

---

## Understanding File Permissions

### Categories
1. **Owner**: The user who owns the file.
2. **Group**: A set of users sharing access rights.
3. **Others**: All other users.

### Permissions
- **Read (`r`)**: View the file or list directory contents.
- **Write (`w`)**: Modify the file or directory.
- **Execute (`x`)**: Execute a file or traverse a directory.

### Example: Changing File Permissions

```bash:path/to/scripts/change_permissions.sh
#!/bin/bash

# Grant execute permission to the owner
chmod u+x script.sh

# Remove write permission from the group
chmod g-w document.txt

# Set read and execute permissions for others
chmod o=rx public_folder/
```

---
## Glossary

- **Operating System (OS)**: Manages hardware and software resources, providing common services for computer programs.
- **Command Interpreter**: Component that processes user commands.
- **Process**: A running program instance with its own code and activity.
- **Driver**: Software enabling OS to communicate with hardware devices.
- **Shell**: Interface for accessing OS services, typically via command-line.
- **User Account**: Defines a user's permissions and rights within the OS.
- **Inter-Process Communication (IPC)**: Methods for processes to communicate and synchronize actions.
- **System Call**: Programs' request interface to the OS kernel for services.
- **Fork**: Duplicate of the shell
- **CWD**: Current working directory
- **Zombie**: Transition, a process waiting for the father to accept the new state to exit

---
## Summary

- **Creating, Playing, Working**: Fundamental activities on computers.
- **Drivers, Shells, Users**: Key components that enable hardware communication, command execution, and user management.
- **Processes & Threads**: Core units of execution within the OS.
- **File Systems & Permissions**: Manage and protect data on storage devices.
- **IPC, Security, System Calls**: Facilitate communication, ensure security, and allow program-OS interactions.

---

## Further Study

- **Operating System Concepts by Silberschatz, Galvin, and Gagne**
- **The Linux Command Line by William Shotts**
- **Microsoft PowerShell Documentation**
