# Bandit Level 1 → Level 2

## Objective

Retrieve the next level's password from a file named `-` in the home directory.

## Commands Used

### 1. List directory contents

```bash
ls
```

Identified the file named `-`.

### 2. Read the file

```bash
cat ./'-'
```

Used a relative path to access the file whose name begins with a hyphen.

## Concepts Learned

- `ls` — lists files and directories.
- `cat` — displays file contents.
- `./` — refers to the current directory.
- Special filenames can cause problems when passed directly as command-line arguments.
- Using a path such as `./-` prevents the filename from being interpreted as a command option.

## Result

Successfully retrieved the password for Bandit Level 2.

