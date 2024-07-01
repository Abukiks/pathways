# Linux Basics

## Overview
- Linux CLI
- Vi Editor
- Package Management
- Service Management

---

## Shell Types
Shells are used to communicate with the operating system. Different shells behave differently, affecting features like autocompletion and command history. Common shell types include:

- **Bourne Shell (sh)**
- **C Shell (csh or tcsh)**
- **Z Shell (zsh)**
- **Bourne Again Shell (bash)**

Older shells have limitations compared to newer ones, such as support for arithmetic operations. Identify the shell in use with the `echo $SHELL` command.

## Basic Commands

### General Commands
- **`echo`**: Print text to the screen.
  ```sh
  $ echo hi
  hi
  ```
- **`ls`**: List files and folders.
  ```sh
  $ ls
  File.txt my_dir1 file2.conf
  ```
- **`cd`**: Change directory.
  ```sh
  $ cd abukiks
  ```
- **`pwd`**: Print the current working directory.
  ```sh
  $ pwd
  /home/abukiks
  ```
- **`mkdir`**: Create a new directory.
  ```sh
  $ mkdir new-abukiks
  ```
- **`;`**: Separate multiple commands on one line.
  ```sh
  $ cd new-abukiks; mkdir www; pwd
  /home/abukiks/new-abukiks
  ```

### Directory Commands
- **Create a Directory Hierarchy**: 
  ```sh
  $ mkdir -p /tmp/asia/india/bangalore
  ```
- **Remove a Directory**:
  ```sh
  $ rm -r /tmp/new-abukiks
  ```
- **Copy a Directory**:
  ```sh
  $ cp -r new-abukiks /tmp/old-abukiks
  ```

### File Commands
- **Create a New File**:
  ```sh
  $ touch new-abukiks.txt
  ```
- **Add Contents to a File**:
  ```sh
  $ cat > new-abukiks.txt
  This is some sample content
  (press Ctrl+D to quit)
  ```
- **View Contents of a File**:
  ```sh
  $ cat new-abukiks.txt
  ```
- **Copy a File**:
  ```sh
  $ cp old-abukiks.txt new-abukiks.txt
  ```
- **Move (Rename) a File**:
  ```sh
  $ mv new-abukiks.txt /root
  ```
- **Remove (Delete) a File**:
  ```sh
  $ rm new-abukiks.txt
  ```

## Vi Editor
- **Open a File in Vi**:
  ```sh
  $ vi index.html
  ```

### Modes
- **Command Mode**: Press `Esc` key
  - Arrow keys or `hjlk` to move
  - `x` to delete a character, `dd` to delete a line
  - `yy` to copy a line, `p` to paste
  - `Ctrl+u` to scroll up, `Ctrl+d` to scroll down
  - `:` to type commands
  - `:w` to save, `:q` to quit, `:wq` to save and quit
  - `/` to find text, `n` to find the next occurrence
- **Insert Mode**: Press `i`
- **Last Line Mode**: Press `:`

## User Accounts
- **Current User**:
  ```sh
  $ whoami
  abukiks
  ```
- **User Details**:
  ```sh
  $ id
  uid=1001(abukiks) gid=1001(abukiks) groups=1001(abukiks)
  ```
- **Switch User**:
  ```sh
  $ su jayz
  ```
- **Switch to Another System**:
  ```sh
  $ ssh jayz@192.168.1.2
  ```
- **Run as Root**:
  ```sh
  $ sudo ls /root
  ```

## Download Files
- **`curl` and `wget`**: Download files from the internet.

## Check OS Version
- **View OS Version**:
  ```sh
  $ cat /etc/os-release
  ```

## Package Managers

### RPM (Red Hat Package Manager)
- **Install Package**:
  ```sh
  $ rpm -i telnet.rpm
  ```
- **Uninstall Package**:
  ```sh
  $ rpm -e telnet
  ```
- **Query Package**:
  ```sh
  $ rpm -q telnet
  ```

### YUM (Yellowdog Updater Modified)
- **Install Package**:
  ```sh
  $ yum install ansible
  ```
- **Remove Package**:
  ```sh
  $ yum remove ansible
  ```
- **List All Packages**:
  ```sh
  $ yum list ansible
  ```
- **Show All Available Versions**:
  ```sh
  $ yum --showduplicates list ansible
  ```

## YUM Repos
- **Show Configured Repositories**:
  ```sh
  $ yum repolist
  ```
- **List Repository Files**:
  ```sh
  $ ls /etc/yum.repos.d/
  ```

## Services
- **Start Service**:
  ```sh
  $ service httpd start
  $ systemctl start httpd
  ```
- **Stop Service**:
  ```sh
  $ service httpd stop
  $ systemctl stop httpd
  ```
- **Check Service Status**:
  ```sh
  $ service httpd status
  $ systemctl status httpd
  ```
- **Enable Service on Startup**:
  ```sh
  $ service httpd enable
  $ systemctl enable httpd
  ```
- **Disable Service on Startup**:
  ```sh
  $ service httpd disable
  $ systemctl disable httpd
  ```

### Creating a Custom Service
Specify the service configuration under `/etc/systemd/system`:
```
[Unit]
Description=This is a Python Application

[Service]
ExecStart=/usr/bin/python3 /opt/code/app.py
ExecStartPre=/bin/bash /opt/code/configure_db.sh
ExecStartPost=/bin/bash /opt/code/email_status.sh
Restart=always

[Install]
WantedBy=multi-user.target
```