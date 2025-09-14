# File Management

This chapter covers **file types, basic and advanced file operations, links, permissions, compression, and searching**.

---

## 1. File Types

Linux classifies files as:

* **Regular files**: contain data, text, or program instructions.
* **Directories**: folders containing files and subdirectories.
* **Block device files**: for devices that handle data in blocks (e.g., disks, `/dev/sda`).
* **Character device files**: for devices that handle data as streams of characters (e.g., `/dev/tty`).
* **Symbolic links**: pointers to other files or directories.

Check file type with:

```bash
ls -l
file filename
```

---

## 2. Creating Files and Directories

```bash
# Create empty file
touch file.txt

# Create a directory
mkdir mydir

# Create multiple directories
mkdir dir1 dir2 dir3
```

---

## 3. Copying and Moving Files/Directories

```bash
# Copy file
cp file.txt file_copy.txt

# Copy directory recursively
cp -r dir1 dir2

# Move or rename files/directories
mv file.txt newfile.txt
mv dir1 /path/to/destination/
```

**Difference between `link` and `copy`:**

* `link` creates a reference (hard or soft) without duplicating data.
* `copy` creates a separate file with its own inode and data.

---

## 4. Links

```bash
# Hard link
ln file.txt hardlink_file.txt

# Symbolic (soft) link
ln -s file.txt symlink_file.txt
```

**Notes:**

* Hard links share the same inode; changes in one reflect in the other.
* Soft links are separate inodes pointing to the original file; can link across filesystems.

---

## 5. File Compression

```bash
# gzip / gunzip
gzip file.txt
gunzip file.txt.gz

# bzip2 / bunzip2
bzip2 file.txt
bunzip2 file.txt.bz2

# tar
ar -cf archive.tar /path/to/dir       # create archive
tar -xf archive.tar                    # extract archive
tar -czf archive.tar.gz /path/to/dir   # gzip compressed
tar -xzf archive.tar.gz                # extract gzip archive
tar -cjf archive.tar.bz2 /path/to/dir  # bzip2 compressed
tar -xjf archive.tar.bz2               # extract bzip2 archive
```

---

## 6. Editing Files

```bash
# Using vim
vim file.txt
# Basic vim commands
# i = insert mode, Esc + :w = save, Esc + :q = quit, Esc + :wq = save & quit
```

---

## 7. File and Directory Permissions

```bash
# View permissions
ls -l

# Change permissions (symbolic)
chmod u+rwx,g+rx,o-r file.txt

# Change permissions (numeric)
chmod 750 file.txt  # u=rwx, g=rx, o=none
```

**Special Permissions:**

* **Setuid**: executes file with owner’s privileges

```bash
chmod u+s executable
```

---

## 8. Searching Files

```bash
# Find files by name
find /path -name "*.txt"

# Find by type
find /path -type f       # regular files
find /path -type d       # directories

# Find by size, owner, modification time
find /path -size +5M
find /path -user root
find /path -mtime -7

# Execute command on found files
find /path -name "*.log" -exec gzip {} \;

# Ask for confirmation before executing
find /path -name "*.log" -ok rm {} \;
```

---

✅ **Practice Tips**

* Create directories, files, and experiment with copying vs linking.
* Try changing file permissions both ways (symbolic and numeric).
* Compress files with gzip/bzip2 and archive them with tar.
* Use `find` with `-exec` and `-ok` to process files automatically.
* Explore special permissions like **setuid**.
