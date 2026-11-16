# Bandit Level 2 → Level 3

## Objective

Find the next level's password in a file whose name contains multiple spaces.

## Commands Used

### 1. List directory contents

```bash
ls
```

Used to identify the filename.

### 2. Read the file

```bash
cat ./'--spaces in this filename--'
```

Used to access and display the contents of a filename containing spaces.

### 3. Exit the SSH session

```bash
exit
```

Used to close the remote shell session.

## Concepts Learned

- `ls` — lists directory contents.
- `cat` — displays file contents.
- Spaces separate command-line arguments unless quoted or escaped.
- Single quotes preserve the literal characters inside them.
- `./` specifies a path relative to the current directory.
- Filenames must match exactly.
- A trailing backslash (`\`) escapes the newline and continues a command on the next line.
- `Ctrl+C` interrupts the current command.

## Troubleshooting and Debugging

### Attempt 1: Unquoted filename

```bash
cat ./--spaces in this filename--
```

**Error:**
```text
cat: ./--spaces: No such file or directory
cat: in: No such file or directory
cat: this: No such file or directory
cat: filename--: No such file or directory
```

**Cause:** The shell interpreted spaces as argument separators, so `cat` received multiple arguments instead of one filename.

### Attempt 2: Replacing spaces with underscores

```bash
cat ./--spaces_in_this_filename--
```

**Error:**
```text
cat: ./--spaces_in_this_filename--: No such file or directory
```

**Cause:** Replacing spaces with underscores changed the filename. Linux requires the exact filename.

### Final Solution

```bash
cat ./'--spaces in this filename--'
```

**Why it worked:** Single quotes preserved the spaces, allowing the shell to pass the complete filename as one argument.

## Result

Successfully retrieved the password for Bandit Level 3.

