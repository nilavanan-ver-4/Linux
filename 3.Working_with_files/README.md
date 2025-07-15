# Working with Files

## 1. File Permissions

**Purpose**: Control who can read, write, or execute a file using permissions (e.g., `rwxr-xr-x` for owner, group, others).

**Related Commands**: `chmod` (change permissions), `chown` (change ownership), `ls -l` (view permissions).

**Example**:

1.  **Create a file and check initial permissions**:
    ```bash
    touch my_document.txt
    ls -l my_document.txt
    ```
    *Expected Output*: `-rw-r--r-- 1 user user 0 Jul 15 09:32 my_document.txt` (read/write for owner, read-only for group and others).

2.  **Change permissions to make it executable for the owner**:
    ```bash
    chmod u+x my_document.txt
    ls -l my_document.txt
    ```
    *Expected Output*: `-rwxr--r-- 1 user user 0 Jul 15 09:32 my_document.txt` (owner can now execute).

3.  **Change permissions to allow read/write for everyone (less secure, for demonstration)**:
    ```bash
    chmod 666 my_document.txt
    ls -l my_document.txt
    ```
    *Expected Output*: `-rw-rw-rw- 1 user user 0 Jul 15 09:32 my_document.txt`.

**Why**: Permissions are crucial for security and proper file access control in a multi-user environment.

## 2. Archiving and Compressing

**Purpose**: Combine multiple files into a single archive (e.g., `.tar`) and compress it (e.g., `.gz`, `.bz2`) to save space or transfer easily.

**Related Commands**: `tar` (archive), `gzip`/`bzip2` (compress), `gunzip`/`bunzip2` (decompress).

**Example**:

1.  **Create some dummy files**:
    ```bash
    echo "Content of file A" > fileA.txt
    echo "Content of file B" > fileB.txt
    mkdir my_folder
    echo "Content of file C" > my_folder/fileC.txt
    ```

2.  **Create a `.tar` archive of files and a folder**:
    ```bash
    tar -cvf my_archive.tar fileA.txt fileB.txt my_folder/
    ls
    ```
    *Expected Output*: `fileA.txt fileB.txt my_archive.tar my_folder`.

3.  **Compress the `.tar` archive using `gzip`**:
    ```bash
    gzip my_archive.tar
    ls
    ```
    *Expected Output*: `fileA.txt fileB.txt my_archive.tar.gz my_folder`.

4.  **Decompress and extract the archive**:
    ```bash
    gunzip my_archive.tar.gz
    tar -xvf my_archive.tar
    ls
    ```
    *Expected Output*: You will see `fileA.txt`, `fileB.txt`, and `my_folder` (with `fileC.txt` inside) restored.

**Why**: Essential for backups, packaging software, or efficient transfer of multiple files over networks.

## 3. Copying and Renaming

**Purpose**: Duplicate files (`cp`) or rename/move them (`mv`) within the file system.

**Related Commands**: `cp` (copy), `mv` (move/rename).

**Example**:

1.  **Create a source file**:
    ```bash
    echo "Original content" > source.txt
    ```

2.  **Copy `source.txt` to `destination.txt`**:
    ```bash
    cp source.txt destination.txt
    ls
    ```
    *Expected Output*: `destination.txt source.txt`.

3.  **Rename `destination.txt` to `renamed.txt`**:
    ```bash
    mv destination.txt renamed.txt
    ls
    ```
    *Expected Output*: `renamed.txt source.txt`.

4.  **Move `source.txt` into a new directory**:
    ```bash
    mkdir my_docs
    mv source.txt my_docs/
    ls my_docs/
    ```
    *Expected Output*: `source.txt` (inside `my_docs` directory).

**Why**: Fundamental operations for organizing and managing files and directories.

## 4. Soft Links / Hard Links

**Purpose**: Create references to files. Soft links (symbolic links) point to a file path, while hard links point to the same inode (data block).

**Related Commands**: `ln` (create links), `ls -l` (view links).

**Example**:

1.  **Create an original file**:
    ```bash
    echo "This is the original file content." > original_file.txt
    ```

2.  **Create a soft link to `original_file.txt`**:
    ```bash
    ln -s original_file.txt soft_link.txt
    ls -l
    ```
    *Expected Output*: `lrwxrwxrwx 1 user user 17 Jul 15 09:32 soft_link.txt -> original_file.txt` (arrow indicates a soft link).

3.  **Create a hard link to `original_file.txt`**:
    ```bash
    ln original_file.txt hard_link.txt
    ls -l
    ```
    *Expected Output*: `-rw-r--r-- 2 user user 30 Jul 15 09:32 hard_link.txt` (note the link count `2` for `original_file.txt` and `hard_link.txt`).

4.  **Modify the original file and observe changes in links**:
    ```bash
    echo "New content added." >> original_file.txt
    cat soft_link.txt
    cat hard_link.txt
    ```
    *Expected Output*: Both `soft_link.txt` and `hard_link.txt` will show the updated content, demonstrating they point to the same data.

5.  **Delete the original file and observe link behavior**:
    ```bash
    rm original_file.txt
    cat soft_link.txt
    cat hard_link.txt
    ```
    *Expected Output*: `soft_link.txt` will likely show an error (`No such file or directory`) because its target is gone. `hard_link.txt` will still display the content because it's a direct link to the data, not the name.

**Why**: Links are useful for creating shortcuts, maintaining file integrity (hard links), or managing files across different locations without duplicating data.

## Practical Example: Combining "Working with Files"

Let’s create a scenario where you manage a project folder using these skills in the Linux shell.

### Step-by-Step Example

1.  **Set Up a Project Directory and initial files**:
    ```bash
    mkdir my_project
    cd my_project
    echo "Task 1 details" > task1.md
    echo "Task 2 details" > task2.md
    ls
    ```
    *Expected Output*: `task1.md task2.md`.

2.  **Set File Permissions for a sensitive document**:
    ```bash
    echo "Confidential info" > sensitive_doc.txt
    chmod 600 sensitive_doc.txt
    ls -l sensitive_doc.txt
    ```
    *Expected Output*: `-rw------- 1 user user 0 Jul 15 09:32 sensitive_doc.txt` (only owner can read/write).

3.  **Archive and Compress project files for backup**:
    ```bash
    tar -czvf project_backup.tar.gz task1.md task2.md sensitive_doc.txt
    ls
    ```
    *Expected Output*: `project_backup.tar.gz sensitive_doc.txt task1.md task2.md`.

4.  **Copy and Rename a task file for a new version**:
    ```bash
    cp task1.md task1_v2.md
    mv task1_v2.md current_task.md
    ls
    ```
    *Expected Output*: `current_task.md project_backup.tar.gz sensitive_doc.txt task1.md task2.md`.

5.  **Create a soft link to the current task for easy access from outside the directory**:
    ```bash
    ln -s $(pwd)/current_task.md ../shortcut_to_task.md
    ls -l ../shortcut_to_task.md
    ```
    *Expected Output*: `lrwxrwxrwx 1 user user 35 Jul 15 09:32 ../shortcut_to_task.md -> /path/to/my_project/current_task.md`.

6.  **Clean Up (using `rm` and `rmdir`)**:
    ```bash
    cd ..
    rm shortcut_to_task.md
    rm -r my_project/
    ls
    ```
    *Expected Output*: `my_project` directory and its contents are removed.

### Full Script for Reference

Save as `manage_project_files.sh`:

```bash
#!/bin/bash

# 1. Set up project directory and initial files
mkdir my_project
cd my_project
echo "Task 1 details" > task1.md
echo "Task 2 details" > task2.md

# 2. Set file permissions for a sensitive document
echo "Confidential info" > sensitive_doc.txt
chmod 600 sensitive_doc.txt

# 3. Archive and Compress project files for backup
tar -czvf project_backup.tar.gz task1.md task2.md sensitive_doc.txt

# 4. Copy and Rename a task file for a new version
cp task1.md task1_v2.md
mv task1_v2.md current_task.md

# 5. Create a soft link to the current task
ln -s $(pwd)/current_task.md ../shortcut_to_task.md

# Display current state (optional)
echo "\n--- Current state of my_project ---"
ls -l
echo "\n--- Shortcut outside project ---"
ls -l ../shortcut_to_task.md

# 6. Clean Up
cd ..
rm shortcut_to_task.md
rm -r my_project/

echo "\nCleanup complete."
```

Make executable: `chmod +x manage_project_files.sh`.
Run: `./manage_project_files.sh`.

### How This Relates to the Roadmap

*   **File Permissions**: `chmod` secures files, demonstrating access control.
*   **Archiving and Compressing**: `tar` and `gzip` manage file storage and packaging.
*   **Copying and Renaming**: `cp` and `mv` organize and version files.
*   **Soft Links / Hard Links**: `ln` provides flexible file access and management.

These skills are enabled by "Shell and Other Basics" (e.g., using `cd` to navigate, `ls -l` for listing, `echo` for creating file content, `rm` for cleanup).