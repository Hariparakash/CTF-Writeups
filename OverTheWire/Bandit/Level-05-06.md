# Bandit Level 5 → Level 6

## Objective

Find the password inside the `inhere` directory. The target file must be human-readable, exactly 1033 bytes, and not executable.

## Commands Used

### 1. List directory contents

```bash
ls
```

Identified the `inhere` directory.

### 2. Enter the directory

```bash
cd inhere
```

Changed the current working directory to `inhere`.

### 3. Search for a file of exactly 1033 bytes

```bash
find -size 1033c
```

Located the file:

```text
./maybehere07/.file2
```

The `c` suffix specifies that the size is measured in bytes.

### 4. Read the identified file

```bash
cat ./maybehere07/.file2
```

Displayed the file contents and retrieved the next level's password.

### 5. Exit the SSH session

```bash
exit
```

Closed the remote shell session.

## Concepts Learned

- `find` — searches for files and directories.
- `-size` — filters files based on their size.
- `1033c` — specifies an exact size of 1033 bytes.
- `cat` — displays file contents.
- `cd` — changes the current working directory.
- Hidden files can also be found using `find`.

## Troubleshooting and Debugging

### Attempt 1: Missing size argument

```bash
find -size
```

**Error:**
```text
find: missing argument to '-size'
```

**Cause:** The `-size` option requires a size value.

### Attempt 2: Invalid size argument

```bash
find -size -a
```

**Error:**
```text
find: invalid -size type 'a'
```

**Cause:** `-a` is not a valid size specification.

### Attempt 3: Incorrect size syntax

```bash
find -size c=1033
```

**Error:**
```text
find: Invalid argument 'c=1033' to -size
```

**Cause:** The size value was written in the wrong format.

### Final Solution

```bash
find -size 1033c
```

Successfully located the file containing the next password.

## Additional Verification

For a more complete verification of all three challenge conditions, these commands can also be used:

```bash
find . -type f -size 1033c ! -executable
file ./maybehere07/.file2
```

The first filters for regular, non-executable files of exactly 1033 bytes. The second checks the identified file's type.

*These verification commands are additional learning notes; they were not part of the original terminal history.*

## Result

Successfully retrieved the password for Bandit Level 6.

