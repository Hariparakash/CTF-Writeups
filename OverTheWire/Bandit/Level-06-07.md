# Bandit Level 6 → Level 7

## Objective

Find a file somewhere on the server that satisfies all three conditions:

- Owned by user `bandit7`.
- Owned by group `bandit6`.
- Exactly 33 bytes in size.

## Commands Used

### 1. Check the home directory

```bash
ls
ls -a
```

Checked the home directory and confirmed that the target was not immediately visible there.

### 2. Search for files of the required size

```bash
find -size 33c
```

Searched the current directory tree for files exactly 33 bytes in size. No matching file was found.

### 3. Search the entire filesystem

```bash
find / -size 33c
```

Started searching from the filesystem root.

### 4. Filter by file ownership

```bash
find / -user bandit7 -group bandit6
```

Filtered the search results by owner and group.

### 5. Apply all three conditions

```bash
find / -user bandit7 -group bandit6 -size 33c
```

Combined the ownership and file-size conditions.

The search identified:

```text
/var/lib/dpkg/info/bandit7.password
```

### 6. Read the identified file

```bash
cat /var/lib/dpkg/info/bandit7.password
```

Displayed the file contents and retrieved the password for Bandit Level 7.

### 7. Exit the SSH session

```bash
exit
```

Closed the remote shell session.

## Concepts Learned

- `find` — searches for files and directories.
- `/` — represents the root directory of the filesystem.
- `-user` — filters by file owner.
- `-group` — filters by group ownership.
- `-size 33c` — searches for files exactly 33 bytes in size.
- Multiple `find` conditions can be combined.
- `cat` — displays file contents.
- Linux permissions restrict access to files and directories.

## Troubleshooting and Debugging

### Attempt 1: Searching only the current directory

```bash
find -size 33c
```

**Observation:** No matching file was found.

**Cause:** The search started from the current directory, while the target was somewhere else on the server.

**Correction:** Started the search from `/`.

### Attempt 2: Incorrect `find` syntax

```bash
find / 33c
```

**Observation:** This did not apply a file-size filter.

**Cause:** The `-size` option was missing.

**Correction:**

```bash
find / -size 33c
```

### Attempt 3: Permission errors during filesystem search

**Observation:** The terminal displayed numerous `Permission denied` messages.

**Cause:** The current user did not have permission to inspect certain directories.

**Correction:** Continued the search. The permission errors did not prevent `find` from locating accessible matching files.

## Improved Search Command

The following version is more precise because it searches for regular files and suppresses permission-error messages:

```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

- `-type f` — searches only regular files.
- `2>/dev/null` — redirects error messages to `/dev/null`.

This is an improved version for future use, not a command shown in my original terminal history.

## Result

Successfully located the file matching all three conditions and retrieved the password for Bandit Level 7.

