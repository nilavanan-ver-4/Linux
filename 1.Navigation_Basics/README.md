# Linux Navigation Basics

## 1. Basic Navigation: Getting Started
At the beginner level, you’ll learn how to move around the file system using the terminal, understand the structure, and perform simple tasks.

### Key Concepts:
*   **Root Directory (`/`)**: The top-level directory that contains all other directories and files.
*   **Home Directory (`~`)**: Your user’s personal directory (e.g., `/home/username`).
*   **Path Types**:
    *   **Absolute Path**: Full path from the root (e.g., `/home/username/documents`).
    *   **Relative Path**: Path relative to your current location (e.g., `./documents` or `documents`).
*   **Common Directories**:
    *   `/home`: User home directories.
    *   `/etc`: Configuration files.
    *   `/var`: Logs and temporary files.
    *   `/bin`: Essential binary (executable) files.
    *   `/usr`: User-installed software and files.

### Essential Commands:

*   **`pwd` (Print Working Directory)**: Shows your current directory.
    ```bash
    $ pwd
    /home/username
    ```

*   **`ls` (List)**: Lists files and directories in your current location.
    ```bash
    $ ls
    documents downloads pictures
    ```
    *   Use `ls -l` for detailed output:
    ```bash
    $ ls -l
    total 12
    drwxr-xr-x 2 username username 4096 Jul 15 10:00 documents
    drwxr-xr-x 2 username username 4096 Jul 15 10:00 downloads
    drwxr-xr-x 2 username username 4096 Jul 15 10:00 pictures
    ```
    *   Use `ls -a` to show hidden files (starting with `.`):
    ```bash
    $ ls -a
    . .. .bashrc documents downloads pictures
    ```

*   **`cd` (Change Directory)**: Moves to another directory.
    *   `cd documents`: Go to the `documents` subdirectory.
    *   `cd /etc`: Go to the absolute path `/etc`.
    *   `cd ..`: Move up one directory.
    *   `cd ~`: Go to your home directory.
    *   `cd -`: Return to the previous directory.

### What to Do:
1.  Open a terminal in a Linux distro (e.g., Ubuntu).
2.  Run `pwd` to see where you are.
3.  Use `ls` to list files, then `cd documents` to enter a folder.
4.  Try `cd ..` to go back and `cd ~` to return home.
5.  Explore `/etc` or `/var` with `ls` (but don’t modify anything yet).

**Why**: These commands let you explore the file system and understand its structure.

### Practice:
1.  Create a folder:
    ```bash
    $ mkdir myfolder
    ```
2.  Navigate into it:
    ```bash
    $ cd myfolder
    ```
3.  List its contents (it will be empty):
    ```bash
    $ ls
    ```

---

## 2. Intermediate Navigation: Working Efficiently
At this level, you’ll build on the basics to navigate more efficiently, use shortcuts, and understand file system nuances.

### Key Concepts:
*   **Hidden Files**: Files starting with a dot (e.g., `.bashrc`) are hidden and used for configurations.
*   **Wildcards**: Use `*` (matches any characters) or `?` (matches one character) for flexible navigation.
    *   **Example**: `ls *.txt` lists all files ending in `.txt`.
*   **Tab Completion**: Press the `Tab` key to auto-complete file or directory names.
*   **Symbolic Links**: Files that point to other files or directories (like shortcuts).
    *   Use `ls -l` to identify links (marked with `l` in permissions).

### Useful Commands:

*   **`ls -lh`**: List files in a human-readable format (e.g., file sizes in KB/MB).
    ```bash
    $ ls -lh
    total 12K
    drwxr-xr-x 2 username username 4.0K Jul 15 10:00 documents
    drwxr-xr-x 2 username username 4.0K Jul 15 10:00 downloads
    drwxr-xr-x 2 username username 4.0K Jul 15 10:00 pictures
    ```

*   **`tree`**: Display the directory structure as a tree.
    *   **Install**: `sudo apt install tree` (Ubuntu).
    ```bash
    $ tree /home/username
    /home/username
    ├── documents
    │   └── report.docx
    ├── downloads
    │   └── image.jpg
    └── pictures
        └── vacation.png
    ```

*   **`find`**: Search for files or directories.
    *   Find a file by name in the current directory:
    ```bash
    $ find . -name "myfile.txt"
    ```
    *   Find all `.log` files in `/var/log`:
    ```bash
    $ find /var/log -name "*.log"
    ```

*   **`locate`**: Faster search using a database.
    *   **Update database**: `sudo updatedb`
    ```bash
    $ locate myfile.txt
    /home/username/documents/myfile.txt
    ```

*   **`cd ../../`**: Move up two directories.

### What to Do:
1.  Use `ls -a` to view hidden files like `.bashrc`.
2.  Try `find /home -name "*.txt"` to locate all text files.
3.  Use `tree` to visualize the structure of your home directory.
4.  Practice tab completion by typing `cd doc` and pressing `Tab`.

**Why**: These tools make navigation faster and help you find files without manually browsing.

---

## 3. Advanced Navigation: Power User Techniques
At the advanced level, you’ll master navigation with scripts, advanced tools, and system-level understanding.

### Key Concepts:
*   **File System Types**: Linux supports multiple file systems (e.g., `ext4`, `NFS`, `tmpfs`).
*   **Mount Points**: External drives or network shares are mounted to directories.
*   **Inodes**: Unique identifiers for files.
*   **Hard vs. Soft Links**:
    *   **Hard Link**: A direct reference to an inode.
    *   **Soft Link**: A reference to a file path.

### Advanced Commands:

*   **`stat`**: Display detailed file metadata.
    ```bash
    $ stat file.txt
      File: file.txt
      Size: 1024       Blocks: 8          IO Block: 4096   regular file
    Device: 801h/2049d Inode: 123456      Links: 1
    Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
    ```

*   **`du -sh /path`**: Check the size of a directory.
    ```bash
    $ du -sh /var/log
    1.2G    /var/log
    ```

*   **`realpath`**: Resolve the absolute path of a file or link.
    ```bash
    $ realpath ./file.txt
    /home/username/documents/file.txt
    ```

*   **`pushd` and `popd`**: Stack-based directory navigation.
    ```bash
    $ pwd
    /home/username
    $ pushd /etc
    /etc ~
    $ pwd
    /etc
    $ popd
    ~
    $ pwd
    /home/username
    ```

*   **`ln`**: Create links.
    *   **Soft Link**:
    ```bash
    $ ln -s /path/to/original.txt /path/to/link.txt
    ```
    *   **Hard Link**:
    ```bash
    $ ln /path/to/original.txt /path/to/hardlink.txt
    ```

*   **`df -T`**: Show disk usage and file system types.
    ```bash
    $ df -T
    Filesystem     Type     1K-blocks      Used Available Use% Mounted on
    /dev/sda1      ext4     482333440 112333440 345000000  25% /
    ```

### Scripting for Navigation:

*   **Bash Script** (save as `navigate.sh`):
    ```bash
    #!/bin/bash
    echo "Current directory: $(pwd)"
    cd /var/log
    echo "Now in: $(pwd)"
    ls -lh | grep ".log"
    ```
    *   Make executable: `chmod +x navigate.sh`
    *   Run: `./navigate.sh`

*   **Aliases**: Add to `~/.bashrc` for shortcuts.
    ```bash
    alias logs='cd /var/log'
    ```
    *   Reload shell: `source ~/.bashrc`
    *   Use alias: `logs`