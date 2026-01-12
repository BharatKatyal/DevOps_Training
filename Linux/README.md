# Linux Foundation Certified Systems Administrator (LFCS) Notes

## Docker Quick Start

```bash
docker run -it ubuntu bash
```

---

## File Permissions in Linux

### Basic Commands
- `chgrp` - Change group ownership
- `groups` - Show user's groups
- `ls -la` - List files with detailed permissions

### Permission Structure
```
-rw-r--r-- 1 root root 0 Jan 4 21:53 test.txt
[-rw-][r--][r--]
[Owner][Group][Others]
```

**Permission Notation:**
- `u=` User/Owner
- `g=` Group
- `o=` Others

### Changing Permissions with chmod

**Change group permissions to read and write:**
```bash
chmod g=rw test.txt
# Result: -rw-rw-r-- 1 root root 0 Jan 4 21:53 test.txt
```

**Set multiple permissions at once:**
```bash
chmod u=rw,g=r,o= test.txt
# Result: -rw-r----- 1 root root 0 Jan 4 21:53 test.txt
```

### Special Permissions

#### SUID (Set User ID)
Allows a user to execute a file with the permissions of the file owner.

**Example:** Allow Jawad to run a file as root without giving him root permissions.

```bash
chmod 4664 suidfile
# Before: -rw-r--r-- 1 root root 0 Jan 4 22:24 suidfile
# After:  -rwSrw-r-- 1 root root 0 Jan 4 22:24 suidfile
```

#### SGID (Set Group ID)
Applied to both executable files and directories. When set on a file, users in the same group can write to it.

```bash
chmod 2664 sgidfile
# Before: -rw-r--r-- 1 root root 0 Jan 4 22:27 sgidfile
# After:  -rw-rwSr-- 1 root root 0 Jan 4 22:27 sgidfile
```

#### Sticky Bit (SBIT)
Restricts deletion of files in a directory to the owner of the file. Only the file owner, directory owner, and root can delete the file.

```bash
mkdir stickydir
chmod +t stickydir/
# Before: drwxr-xr-x 2 root root 4096 Jan 4 22:30 stickydir
# After:  drwxr-xr-t 2 root root 4096 Jan 4 22:30 stickydir
```

---

## Searching for Files with `find`

### Basic Syntax
```bash
find [/path/to/search] [search parameters]
```

### Search by Name
```bash
find /bin/ -name file1.txt              # Search in /bin/ directory
find -name file1.txt                    # Search in current directory
find /usr/share/ -name '*.py'           # Find all Python files
find -name bharat                       # Case sensitive
find -iname bharat                      # Case insensitive
find -name "f*"                         # Wildcard - files starting with 'f'
```

### Search by Modification Time
```bash
find -mmin 5                            # Modified exactly 5 minutes ago
find -mmin -5                           # Modified in last 5 minutes
find -mmin +5                           # Modified more than 5 minutes ago

find -mtime 0                           # Modified in last 24 hours
find -mtime 1                           # Modified 24-48 hours ago
```

### Search by Size
```bash
find -size 512k                         # Exactly 512 kilobytes
find -size +512k                        # Greater than 512 kilobytes
find -size -512k                        # Less than 512 kilobytes
```

### Combining Search Criteria
```bash
find -name "f*" -size +512k             # AND operator (both conditions)
find -name "f*" -o -size +512k          # OR operator (either condition)
```

---

## Compare and Manipulate Files

### File Viewing Commands

**cat** - Display file contents
```bash
cat file.txt
```

**tac** - Display file contents in reverse (backwards cat)
```bash
tac file.txt
```

**tail** - Display end of file (default: last 10 lines)
```bash
tail logs.csv                           # Last 10 lines
tail -n 20 logs.csv                     # Last 20 lines
```

**head** - Display beginning of file (default: first 10 lines)
```bash
head logs.csv                           # First 10 lines
head -n 20 logs.csv                     # First 20 lines
head -c 20 logs.csv                     # First 20 characters
```

### sed - Stream Editor

**Purpose:** Search and replace text in files

**Basic Syntax:**
```bash
sed 's/old/new/g' file.txt
```
- `s` = substitute
- `g` = global (all occurrences on each line; without it, only first occurrence per line)
- Must wrap pattern in single quotes

**Examples:**
```bash
# Replace all occurrences of 'originalbucket' with 'newbucket'
sed 's/originalbucket/newbucket/g' file.txt

# In-place edit (modify the file directly)
sed -i 's/originalbucket/newbucket/g' file.txt
```

**Use Case:** Change all references to an S3 bucket name
```
originalbucket.s3.amazonaws.com → newbucket.s3.amazonaws.com
```

---

## Vim Editor

### Basic Commands
- `dd` - Cut/delete current line
