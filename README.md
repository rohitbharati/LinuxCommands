# OS

## What is an Operating System?
An Operating System (OS) is system software that acts as an interface between the user and the underlying hardware.  
It manages system resources, including the CPU, memory, storage, and input/output devices, ensuring smooth execution of applications.  

### Key Functions of an OS:
- **Process Management** – Handles the execution of multiple processes.
- **Memory Management** – Allocates and deallocates memory to processes.
- **File System Management** – Manages files, directories, and storage devices.
- **Device Management** – Facilitates communication between hardware and software.
- **User Interface** – Provides CLI (Command Line Interface) or GUI (Graphical User Interface).

## Mostly used OS
- **Windows** – Microsoft’s OS, widely used for desktops.
- **macOS (OSX)** – Apple’s OS, primarily for Mac computers.
- **IBM AIX** – Enterprise UNIX system by IBM.
- **HP-UX** – Hewlett-Packard's UNIX operating system.
- **Solaris** – UNIX-based OS developed by Sun Microsystems (now Oracle).
- **Linux** – Open-source UNIX-like OS, used widely in servers and cloud computing.

---

## Linux File System Hierarchy

The Linux file system follows a hierarchical structure with a root (`/`) directory containing subdirectories.  

| Directory Name | Description |
|--------------|-------------|
| `/` | Root directory, starting point of the filesystem. |
| `/root` | Home directory for the root user (superuser). |
| `/home/` | Home directory for normal users (`ubuntu`, `ec2-user`, etc.). |
| `/usr` | Contains installed software and user applications. |
| `/bin` | Essential system binaries (commands used by all users). |
| `/sbin` | System binaries, mainly used by the root user. |
| `/var` | Stores variable data like logs, databases, and temporary files. |
| `/etc` | Contains system-wide configuration files. |
| `/tmp` | Temporary files storage (cleared on reboot). |
| `/dev` | Device files (represents hardware components like disks, USBs). |
| `/proc` | Virtual filesystem containing system process information. |

---

## Basic Linux Commands

| Command | Description |
|---------|-------------|
| `date` | Displays the current system date and time. |
| `cal` | Displays the calendar of the current month/year. |
| `whoami` | Shows the current logged-in user. |
| `finger` | Displays detailed information about a user. |
| `users` | Shows all logged-in users. |
| `who` | Displays who is currently online. |
| `w` | Shows detailed user activity, uptime, and load. |
| `man <command>` | Displays the manual page for a command. |

Example usage:
```bash
man ls
```
This will show the detailed manual for the `ls` command.

---

## Files in Linux

| Command | Description |
|---------|-------------|
| `pwd` | Prints the present working directory. |
| `ls` | Lists the files and directories. |
| `ls -l` | Detailed list of files with permissions, owner, size, and timestamp. |
| `ls -a` | Lists all files, including hidden files (starting with `.`). |
| `ls -lt` | Lists files sorted by last modified time. |
| `ls -ltr` | Lists files by last modified time in reverse order. |
| `cat filename` | Displays the content of a file. |
| `less filename` | Displays file content page by page (use `q` to exit). |
| `more filename` | Similar to `less`, but exits after displaying content. |
| `head filename` | Shows the first 10 lines of a file. |
| `tail filename` | Shows the last 10 lines of a file. |

---

## Create and Delete Files

| Command | Description |
|---------|-------------|
| `cat > filename` | Creates a new file and writes to it until `Ctrl+C` is pressed. |
| `cat >> filename` | Appends content to an existing file. |
| `rm filename` | Deletes a file (permanently, no recycle bin). |
| `rm -f filename` | Force delete a file without confirmation. |
| `rmdir directory` | Deletes an empty directory. |
| `rm -rf directory` | Recursively deletes a directory and its contents. |

⚠ **Warning:** `rm -rf /` can delete the entire system.

---

## Managing Files and Directories

| Command | Description |
|---------|-------------|
| `cp file1 file2` | Copies `file1` to `file2`. |
| `mv file1 file2` | Moves or renames a file. |
| `find /path -name filename` | Searches for a file by name. |
| `diff file1 file2` | Compares two files line by line. |
| `file filename` | Identifies the file type. |

---

## `grep` - Global Regular Expression Print

| Command | Description |
|---------|-------------|
| `grep hello filename` | Searches for "hello" in a file (case-sensitive). |
| `grep -i hello filename` | Case-insensitive search. |
| `ls -l | grep filename` | Filters `ls -l` output to show only specific filenames. |
| `grep ^d filename` | Finds lines starting with "d". |

---

## `sed` - Stream Editor

Used for search and replace operations.

| Command | Description |
|---------|-------------|
| `sed 's/FROM/IMAGE/' Dockerfile` | Replaces `FROM` with `IMAGE` in output only. |
| `sed 's/FROM/IMAGE/g' Dockerfile` | Replaces all occurrences per line. |
| `sed -i 's/FROM/IMAGE/' Dockerfile` | Modifies the file. |
| `sed -n '5,10p' Dockerfile` | Displays lines 5 to 10. |
| `sed '5,10d' Dockerfile` | Deletes lines 5 to 10 in output. |

Example:
```bash
sed 's/old_text/new_text/' filename
```

---

## User Management

### Types of Users:
- **Root User (Super User)** – Admin with full system access.
- **System Users** – Created by software like Docker or Apache.
- **Normal Users** – Created by administrators for human users.

To view system users:
```bash
cat /etc/passwd
```

### Creating a New User
When a user is created:
- A home directory is created (`/home/username`).
- A unique UID and GID are assigned.
- An entry is added to `/etc/passwd`.

Command:
```bash
useradd -u <UserID> -G <GroupID> -g <PrimaryGroup> -d <HomeDir> -c "Comment" -s <Shell> username
```

Example:
```bash
useradd -u 1003 -G docker -g users -d /home/john -c "John Doe" -s /bin/bash john
```

To modify an existing user:
```bash
usermod -G docker rohit-test
```

To set a password:
```bash
passwd username
```

To switch users:
```bash
su username
```

---

## Enabling Password Authentication in EC2

By default, AWS EC2 disables password authentication.  
To enable it:
1. Edit `/etc/ssh/sshd_config`
2. Find the line:
   ```bash
   PasswordAuthentication no
   ```
   Change it to:
   ```bash
   PasswordAuthentication yes
   ```
3. Restart SSH:
   ```bash
   sudo systemctl restart sshd
   ```

---
# File Permissions Explained

## Understanding `ls -l` Output
When we run the `ls -l` command, we get an output like this:

```bash
-rw-rw-r--  1 ec2-user ec2-user      51159 Mar 26  2024 apps.txt
drwxrwxr-x  2 ec2-user ec2-user        161 Dec 15  2021 backup
```

### Breakdown of the first column (`-rw-rw-r--`)

The first column represents file type and permissions:
```
-rw-rw-r--
```
This can be divided into four parts for better understanding:

| Section | Representation | Meaning |
|---------|---------------|---------|
| `-` | File Type | `-` (Regular File), `d` (Directory), `b` (Block Device), `c` (Character Device) |
| `rw-` | Owner Permissions | Read (`r`), Write (`w`), No Execute (`-`) |
| `rw-` | Group Permissions | Read (`r`), Write (`w`), No Execute (`-`) |
| `r--` | Others Permissions | Read (`r`), No Write (`-`), No Execute (`-`) |

### Explanation:
- **Owner**: The first `rw-` means the owner of the file (`ec2-user`) has **read and write** access.
- **Group**: The second `rw-` means users in the `ec2-user` group also have **read and write** access.
- **Others**: The last `r--` means other users can **only read** the file but cannot modify or execute it.

---

## File Types in Linux

Linux classifies files into different types:

| Symbol | File Type | Example |
|--------|-----------|---------|
| `-` | Normal File | Text files, scripts, binary files (`.txt`, `.sh`, etc.) |
| `b` | Block File | Hard disk partitions, floppy disks |
| `c` | Character File | Input devices like keyboard, mouse |
| `d` | Directory | Folders containing files |
| `l` | Link File | Shortcuts to other files |

Example of directory permissions:
```bash
drwx------ 27 ec2-user ec2-user 4096 Mar 19 ec2-user
```

| Section | Meaning |
|---------|---------|
| `drwx------` | Directory with read, write, execute only for owner (`ec2-user`) |
| `27` | Number of file links |
| `ec2-user` | Owner of the directory |
| `ec2-user` | Primary group of the owner |
| `4096` | Size of the directory in bytes |
| `Mar 19` | Last modified date |
| `ec2-user` | Directory name |

---

## Checking Directory Permissions

To check current users permissions for a file or directory:
```bash
ls -l
```

To check actual permission settings:
```bash
ls -ld /path
```

Example:
```bash
[root@server ~]# ls -ld /home
drwxr-xr-x 5 root root 56 Mar 25 14:11 /home
```
This means:
- **Owner (`root`) has full (rwx) access.**
- **Group (`root`) has read and execute (`r-x`) access.**
- **Others (`r-x`) can only list (`ls`) and access (`cd`).**

For a private directory:
```bash
[root@server ~]# ls -ld /home/rohit-test
drwx------ 2 rohit-test rohit-test 100 Mar 26 14:12 /home/rohit-test
```
- Only **owner (`rohit-test`)** has full access.
- No one else can access it.

---

## Changing File Permissions

Linux allows setting permissions in two ways:
1. **Symbolic (ugo)**
2. **Absolute (numeric)**

---

### Symbolic Mode (`ugo`)

Symbolic mode allows changing permissions using **user (u), group (g), others (o).**

#### Syntax:
```bash
chmod [who] [+/-/=] [permissions] filename
```

| Symbol | Meaning |
|--------|---------|
| `u` | User (Owner) |
| `g` | Group |
| `o` | Others |
| `+` | Add Permission |
| `-` | Remove Permission |
| `=` | Set Exact Permission |

#### Examples:

1. **Grant full permissions to the owner and group, read-only to others:**
   ```bash
   chmod u=rwx,g=rwx,o=r filename
   ```
2. **Grant all users full permissions:**
   ```bash
   chmod ugo=rwx filename
   ```
3. **Add execute (`x`) permission for others (`o`):**
   ```bash
   chmod o+x file1
   ```
   Before: `drwx------ file1`  
   After:  `drwx-----x file1`
4. **Remove execute (`x`) permission for user (`u`):**
   ```bash
   chmod u-x file1
   ```
   Example: If a script fails with "Permission Denied," use:
   ```bash
   chmod u+x script.sh
   ```

---

### Absolute Mode (Numeric)

In absolute mode, we define permissions using numbers:

| Number | Permission |
|--------|-----------|
| `1` | Execute (`x`) |
| `2` | Write (`w`) |
| `4` | Read (`r`) |

The final value is the sum of these numbers.

#### Examples:

| Command | Meaning |
|---------|---------|
| `chmod 777 filename` | **Everyone** gets full access (`rwxrwxrwx`). |
| `chmod 444 filename` | **Read-only** for all (`r--r--r--`). |
| `chmod 744 filename` | **Owner: Full (`rwx`), Others: Read (`r--`)** |
| `chmod 721 filename` | **Owner: Full (`rwx`), Group: Read (`r--`), Others: Execute (`--x`)** |
| `chmod 765 filename` | **Owner: Full (`rwx`), Group: Read-Write (`rw-`), Others: Read-Execute (`r-x`)** |
| `chmod 000 filename` | **Revoke all permissions.** |

---

## Changing File Ownership

Only the **root** user can change file ownership.

### Usage:
```bash
chown user:group filename
```

| Command | Meaning |
|---------|---------|
| `chown user filename` | Changes only the **owner** of the file. |
| `chown :group filename` | Changes only the **group** of the file. |
| `chown user:group filename` | Changes **both user and group**. |

If ownership is changed, file access can be affected based on permissions.

---

## `file` Command in Linux

The `file` command identifies the type of a given file.

### Example Output:
```bash
[ec2-user@server ~]$ file *
apache-log4j-2.16.0-bin:          directory
apache-log4j-2.16.0-bin.tar.gz:   gzip compressed data
apps.txt:                         ASCII text, with very long lines
backup:                           directory
Dockerfile:                       ASCII text
server_ssl.cert:                  PEM certificate
test-new-buildpack:               directory
vault-test:                       directory
```

This helps in quickly identifying:
- **Directories**
- **Text files**
- **Compressed files**
- **Certificates**
- **Scripts**

---

This enhanced version makes it easier to read and understand file permissions, types, and ownership in Linux. 🚀
# OS

## What is an Operating System?
An Operating System (OS) is system software that acts as an interface between the user and the underlying hardware.  
It manages system resources, including the CPU, memory, storage, and input/output devices, ensuring smooth execution of applications.  

### Key Functions of an OS:
- **Process Management** – Handles the execution of multiple processes.
- **Memory Management** – Allocates and deallocates memory to processes.
- **File System Management** – Manages files, directories, and storage devices.
- **Device Management** – Facilitates communication between hardware and software.
- **User Interface** – Provides CLI (Command Line Interface) or GUI (Graphical User Interface).

## Mostly used OS
- **Windows** – Microsoft’s OS, widely used for desktops.
- **macOS (OSX)** – Apple’s OS, primarily for Mac computers.
- **IBM AIX** – Enterprise UNIX system by IBM.
- **HP-UX** – Hewlett-Packard's UNIX operating system.
- **Solaris** – UNIX-based OS developed by Sun Microsystems (now Oracle).
- **Linux** – Open-source UNIX-like OS, used widely in servers and cloud computing.

---

## Linux File System Hierarchy

The Linux file system follows a hierarchical structure with a root (`/`) directory containing subdirectories.  

| Directory Name | Description |
|--------------|-------------|
| `/` | Root directory, starting point of the filesystem. |
| `/root` | Home directory for the root user (superuser). |
| `/home/` | Home directory for normal users (`ubuntu`, `ec2-user`, etc.). |
| `/usr` | Contains installed software and user applications. |
| `/bin` | Essential system binaries (commands used by all users). |
| `/sbin` | System binaries, mainly used by the root user. |
| `/var` | Stores variable data like logs, databases, and temporary files. |
| `/etc` | Contains system-wide configuration files. |
| `/tmp` | Temporary files storage (cleared on reboot). |
| `/dev` | Device files (represents hardware components like disks, USBs). |
| `/proc` | Virtual filesystem containing system process information. |

---

## Basic Linux Commands

| Command | Description |
|---------|-------------|
| `date` | Displays the current system date and time. |
| `cal` | Displays the calendar of the current month/year. |
| `whoami` | Shows the current logged-in user. |
| `finger` | Displays detailed information about a user. |
| `users` | Shows all logged-in users. |
| `who` | Displays who is currently online. |
| `w` | Shows detailed user activity, uptime, and load. |
| `man <command>` | Displays the manual page for a command. |

Example usage:
```bash
man ls
```
This will show the detailed manual for the `ls` command.

---

## Files in Linux

| Command | Description |
|---------|-------------|
| `pwd` | Prints the present working directory. |
| `ls` | Lists the files and directories. |
| `ls -l` | Detailed list of files with permissions, owner, size, and timestamp. |
| `ls -a` | Lists all files, including hidden files (starting with `.`). |
| `ls -lt` | Lists files sorted by last modified time. |
| `ls -ltr` | Lists files by last modified time in reverse order. |
| `cat filename` | Displays the content of a file. |
| `less filename` | Displays file content page by page (use `q` to exit). |
| `more filename` | Similar to `less`, but exits after displaying content. |
| `head filename` | Shows the first 10 lines of a file. |
| `tail filename` | Shows the last 10 lines of a file. |

---

## Create and Delete Files

| Command | Description |
|---------|-------------|
| `cat > filename` | Creates a new file and writes to it until `Ctrl+C` is pressed. |
| `cat >> filename` | Appends content to an existing file. |
| `rm filename` | Deletes a file (permanently, no recycle bin). |
| `rm -f filename` | Force delete a file without confirmation. |
| `rmdir directory` | Deletes an empty directory. |
| `rm -rf directory` | Recursively deletes a directory and its contents. |

⚠ **Warning:** `rm -rf /` can delete the entire system.

---

## Managing Files and Directories

| Command | Description |
|---------|-------------|
| `cp file1 file2` | Copies `file1` to `file2`. |
| `mv file1 file2` | Moves or renames a file. |
| `find /path -name filename` | Searches for a file by name. |
| `diff file1 file2` | Compares two files line by line. |
| `file filename` | Identifies the file type. |

---

## `grep` - Global Regular Expression Print

| Command | Description |
|---------|-------------|
| `grep hello filename` | Searches for "hello" in a file (case-sensitive). |
| `grep -i hello filename` | Case-insensitive search. |
| `ls -l | grep filename` | Filters `ls -l` output to show only specific filenames. |
| `grep ^d filename` | Finds lines starting with "d". |

---

## `sed` - Stream Editor

Used for search and replace operations.

| Command | Description |
|---------|-------------|
| `sed 's/FROM/IMAGE/' Dockerfile` | Replaces `FROM` with `IMAGE` in output only. |
| `sed 's/FROM/IMAGE/g' Dockerfile` | Replaces all occurrences per line. |
| `sed -i 's/FROM/IMAGE/' Dockerfile` | Modifies the file. |
| `sed -n '5,10p' Dockerfile` | Displays lines 5 to 10. |
| `sed '5,10d' Dockerfile` | Deletes lines 5 to 10 in output. |

Example:
```bash
sed 's/old_text/new_text/' filename
```

---

## User Management

### Types of Users:
- **Root User (Super User)** – Admin with full system access.
- **System Users** – Created by software like Docker or Apache.
- **Normal Users** – Created by administrators for human users.

To view system users:
```bash
cat /etc/passwd
```

### Creating a New User
When a user is created:
- A home directory is created (`/home/username`).
- A unique UID and GID are assigned.
- An entry is added to `/etc/passwd`.

Command:
```bash
useradd -u <UserID> -G <GroupID> -g <PrimaryGroup> -d <HomeDir> -c "Comment" -s <Shell> username
```

Example:
```bash
useradd -u 1003 -G docker -g users -d /home/john -c "John Doe" -s /bin/bash john
```

To modify an existing user:
```bash
usermod -G docker rohit-test
```

To set a password:
```bash
passwd username
```

To switch users:
```bash
su username
```

---

## Enabling Password Authentication in EC2

By default, AWS EC2 disables password authentication.  
To enable it:
1. Edit `/etc/ssh/sshd_config`
2. Find the line:
   ```bash
   PasswordAuthentication no
   ```
   Change it to:
   ```bash
   PasswordAuthentication yes
   ```
3. Restart SSH:
   ```bash
   sudo systemctl restart sshd
   ```

---
# File Permissions Explained

## Understanding `ls -l` Output
When we run the `ls -l` command, we get an output like this:

```bash
-rw-rw-r--  1 ec2-user ec2-user      51159 Mar 26  2024 apps.txt
drwxrwxr-x  2 ec2-user ec2-user        161 Dec 15  2021 backup
```

### Breakdown of the first column (`-rw-rw-r--`)

The first column represents file type and permissions:
```
-rw-rw-r--
```
This can be divided into four parts for better understanding:

| Section | Representation | Meaning |
|---------|---------------|---------|
| `-` | File Type | `-` (Regular File), `d` (Directory), `b` (Block Device), `c` (Character Device) |
| `rw-` | Owner Permissions | Read (`r`), Write (`w`), No Execute (`-`) |
| `rw-` | Group Permissions | Read (`r`), Write (`w`), No Execute (`-`) |
| `r--` | Others Permissions | Read (`r`), No Write (`-`), No Execute (`-`) |

### Explanation:
- **Owner**: The first `rw-` means the owner of the file (`ec2-user`) has **read and write** access.
- **Group**: The second `rw-` means users in the `ec2-user` group also have **read and write** access.
- **Others**: The last `r--` means other users can **only read** the file but cannot modify or execute it.

---

## File Types in Linux

Linux classifies files into different types:

| Symbol | File Type | Example |
|--------|-----------|---------|
| `-` | Normal File | Text files, scripts, binary files (`.txt`, `.sh`, etc.) |
| `b` | Block File | Hard disk partitions, floppy disks |
| `c` | Character File | Input devices like keyboard, mouse |
| `d` | Directory | Folders containing files |
| `l` | Link File | Shortcuts to other files |

Example of directory permissions:
```bash
drwx------ 27 ec2-user ec2-user 4096 Mar 19 ec2-user
```

| Section | Meaning |
|---------|---------|
| `drwx------` | Directory with read, write, execute only for owner (`ec2-user`) |
| `27` | Number of file links |
| `ec2-user` | Owner of the directory |
| `ec2-user` | Primary group of the owner |
| `4096` | Size of the directory in bytes |
| `Mar 19` | Last modified date |
| `ec2-user` | Directory name |

---

## Checking Directory Permissions

To check current users permissions for a file or directory:
```bash
ls -l
```

To check actual permission settings:
```bash
ls -ld /path
```

Example:
```bash
[root@server ~]# ls -ld /home
drwxr-xr-x 5 root root 56 Mar 25 14:11 /home
```
This means:
- **Owner (`root`) has full (rwx) access.**
- **Group (`root`) has read and execute (`r-x`) access.**
- **Others (`r-x`) can only list (`ls`) and access (`cd`).**

For a private directory:
```bash
[root@server ~]# ls -ld /home/rohit-test
drwx------ 2 rohit-test rohit-test 100 Mar 26 14:12 /home/rohit-test
```
- Only **owner (`rohit-test`)** has full access.
- No one else can access it.

---

## Changing File Permissions

Linux allows setting permissions in two ways:
1. **Symbolic (ugo)**
2. **Absolute (numeric)**

---

### Symbolic Mode (`ugo`)

Symbolic mode allows changing permissions using **user (u), group (g), others (o).**

#### Syntax:
```bash
chmod [who] [+/-/=] [permissions] filename
```

| Symbol | Meaning |
|--------|---------|
| `u` | User (Owner) |
| `g` | Group |
| `o` | Others |
| `+` | Add Permission |
| `-` | Remove Permission |
| `=` | Set Exact Permission |

#### Examples:

1. **Grant full permissions to the owner and group, read-only to others:**
   ```bash
   chmod u=rwx,g=rwx,o=r filename
   ```
2. **Grant all users full permissions:**
   ```bash
   chmod ugo=rwx filename
   ```
3. **Add execute (`x`) permission for others (`o`):**
   ```bash
   chmod o+x file1
   ```
   Before: `drwx------ file1`  
   After:  `drwx-----x file1`
4. **Remove execute (`x`) permission for user (`u`):**
   ```bash
   chmod u-x file1
   ```
   Example: If a script fails with "Permission Denied," use:
   ```bash
   chmod u+x script.sh
   ```

---

### Absolute Mode (Numeric)

In absolute mode, we define permissions using numbers:

| Number | Permission |
|--------|-----------|
| `1` | Execute (`x`) |
| `2` | Write (`w`) |
| `4` | Read (`r`) |

The final value is the sum of these numbers.

#### Examples:

| Command | Meaning |
|---------|---------|
| `chmod 777 filename` | **Everyone** gets full access (`rwxrwxrwx`). |
| `chmod 444 filename` | **Read-only** for all (`r--r--r--`). |
| `chmod 744 filename` | **Owner: Full (`rwx`), Others: Read (`r--`)** |
| `chmod 721 filename` | **Owner: Full (`rwx`), Group: Read (`r--`), Others: Execute (`--x`)** |
| `chmod 765 filename` | **Owner: Full (`rwx`), Group: Read-Write (`rw-`), Others: Read-Execute (`r-x`)** |
| `chmod 000 filename` | **Revoke all permissions.** |

---

## Changing File Ownership

Only the **root** user can change file ownership.

### Usage:
```bash
chown user:group filename
```

| Command | Meaning |
|---------|---------|
| `chown user filename` | Changes only the **owner** of the file. |
| `chown :group filename` | Changes only the **group** of the file. |
| `chown user:group filename` | Changes **both user and group**. |

If ownership is changed, file access can be affected based on permissions.

---

## `file` Command in Linux

The `file` command identifies the type of a given file.

### Example Output:
```bash
[ec2-user@server ~]$ file *
apache-log4j-2.16.0-bin:          directory
apache-log4j-2.16.0-bin.tar.gz:   gzip compressed data
apps.txt:                         ASCII text, with very long lines
backup:                           directory
Dockerfile:                       ASCII text
server_ssl.cert:                  PEM certificate
test-new-buildpack:               directory
vault-test:                       directory
```

This helps in quickly identifying:
- **Directories**
- **Text files**
- **Compressed files**
- **Certificates**
- **Scripts**

---

# System and Network Utilities in Linux

## `uname` - Get System Information
The `uname` command provides detailed information about the host system.

```bash
uname -a
```
**Example Output:**
```
Linux ip-10-0-0-51.eu-central-1.compute.internal 4-265.amzn2.x86_64 #1 SMP Fri Jun 14 09:53:28 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
```
- Displays system name, kernel version, architecture, and more.

---

## `du` - Disk Usage
The `du` command is used to check the size of directories and files.

| Option | Description |
|--------|-------------|
| `du -m` | Displays disk usage in MB. |
| `du -h` | Displays disk usage in a human-readable format (KB, MB, GB). |

---

## `whereis` and `which` - Locate Installed Commands

- **`whereis`** – Shows all locations where a command is installed.
- **`which`** – Displays the exact path of the executable being used.

```bash
whereis docker
```
**Example Output:**
```
docker: /usr/bin/docker /etc/docker /usr/libexec/docker /usr/share/man/man1/docker.1.gz
```

```bash
which docker
```
**Example Output:**
```
/bin/docker
```
---

## `df` - Disk Space and Mounted Filesystems
The `df` command reports the available and used disk space.

```bash
df -h
```
**Example Output:**
```
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        2.0G     0  2.0G   0% /dev
tmpfs           2.0G     0  2.0G   0% /dev/shm
tmpfs           2.0G  448K  2.0G   1% /run
tmpfs           2.0G     0  2.0G   0% /sys/fs/cgroup
/dev/xvda1       50G   47G  3.2G  94% /
tmpfs           393M     0  393M   0% /run/user/1000
```
- The `-h` flag makes the output human-readable.

---

## Software Management with `yum`
**YUM (Yellowdog Updater, Modified)** is used to manage software packages on Amazon Linux, RedHat, and CentOS.

### Common YUM Commands
| Command | Description |
|---------|-------------|
| `yum install <package>` | Installs a package. |
| `yum remove <package>` | Removes an installed package. |
| `yum update <package>` | Updates a specific package. |
| `yum list available` | Lists all available packages. |
| `yum list installed` | Lists all installed packages. |
| `yum info <package>` | Displays details about a package. |

```bash
yum info docker
```
**Example Output (Installed Packages):**
```
Name        : docker
Arch        : x86_64
Version     : 25.0.6
Release     : 1.amzn2.0.2
Size        : 169 M
Repo        : installed
Summary     : Automates deployment of containerized applications
```
**Example Output (Available Packages):**
```
Name        : docker
Arch        : x86_64
Version     : 25.0.8
Release     : 1.amzn2.0.1
Size        : 45 M
Repo        : amzn2extra-docker/2/x86_64
```

---

# Networking in Linux

## `hostname` - Identify or Change Server Name
The hostname is the server's network identity.

### Check the current hostname:
```bash
cat /etc/hostname
hostname
```
**Example Output:**
```
ip-10-51.eu-central-1.compute.internal
```

### Change hostname:
- **Permanently:** Modify `/etc/hostname` and reboot.
- **Temporarily:** 
  ```bash
  hostname new-hostname
  ```

To restart the system after hostname change:
```bash
init 6
```

---

## `ping` - Check Network Connectivity
The `ping` command tests connectivity to a remote host.

```bash
ping google.com
```
- Helps in troubleshooting network issues.

---

## `wget` - Download Files from the Internet
The `wget` command is used to download files from the internet.

Example:
```bash
wget https://example.com/file.zip
```

---

## `ifconfig` - View Network Interfaces
The `ifconfig` command displays network interface details, including private IP addresses.

```bash
ifconfig
```
- If Docker is installed, you may see `docker0` with a `172.*` IP range.
- `eth0` represents the host's primary network interface.

---

## `netstat` - View Active Connections and Ports
The `netstat -pluntu` command lists all processes along with their ports.

```bash
netstat -pluntu
```
**Example Output:**
```
tcp6       0      0 :::22                   :::*                    LISTEN      3066/sshd
```
- This shows that SSH (port 22) is active.

---

## `telnet` - Check if a Port is Open
The `telnet` command tests whether a port is open.

```bash
telnet localhost 80
```
**Example Output (Port Closed):**
```
Trying 127.0.0.1...
telnet: connect to address 127.0.0.1: Connection refused
```

```bash
telnet localhost 22
```
**Example Output (Port Open - SSH Running):**
```
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
SSH-2.0-OpenSSH_7.4
```
To exit, press `Ctrl + C`.

---
