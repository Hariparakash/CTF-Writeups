# Bandit Level 3 → Level 4

## Objective

Find the next level's password inside a hidden file located in the `inhere` directory.

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

### 3. Display hidden files

```bash
ls -a
```

Revealed the hidden file named `...Hiding-From-You`.

### 4. Read the hidden file

```bash
cat ./'...Hiding-From-You'
```

Displayed the file contents and retrieved the next level's password.

### 5. Exit the SSH session

```bash
exit
```

Closed the remote shell session.

## Concepts Learned

- `ls` — lists directory contents.
- `ls -a` — displays hidden files, including entries beginning with `.`.
- `cd` — changes the current working directory.
- `cat` — displays file contents.
- `.` — represents the current directory.
- `..` — represents the parent directory.
- `./` — specifies a relative path from the current directory.
- File permissions determine whether a file can be read, written, or executed.

## Troubleshooting and Debugging

### Attempt 1: Listing files with `ls`

```bash
ls
```

**Observation:** The directory appeared empty.

**Correction:** Used `ls -a` to reveal hidden files.

### Attempt 2: Trying to read the current directory

```bash
cat ./. 
```

**Error:**

```text
cat: ./.: Is a directory
```

**Cause:** `.` refers to the current directory, not a regular file.

### Attempt 3: Executing the hidden file

```bash
./...Hiding-From-You
```

**Error:**

```text
-bash: ./...Hiding-From-You: Permission denied
```

**Cause:** Attempted to execute the file instead of reading its contents.

**Correction:** Used `cat` to read the file.

### Final Solution

```bash
cat ./'...Hiding-From-You'
```

Successfully accessed the hidden file and retrieved its contents.

## Result

Successfully retrieved the password for Bandit Level 4.

