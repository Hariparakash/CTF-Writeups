# Bandit Level 4 → Level 5

## Objective

Find the password for the next level in the only human-readable file inside the `inhere` directory.

## Commands Used

### 1. List directory contents

```bash
ls
```

Used to identify the `inhere` directory.

### 2. Enter the directory

```bash
cd inhere
```

Changed the current working directory to `inhere`.

### 3. Identify file types

```bash
file ./-file*
```

Used to inspect the file types of all files in the directory and identify the human-readable file.

### 4. Read the identified file

```bash
cat ./-file07
```

Displayed the contents of the identified human-readable file and retrieved the next level's password.

### 5. Exit the SSH session

```bash
exit
```

Closed the remote shell session.

## Concepts Learned

- `ls` — lists directory contents.
- `cd` — changes the current working directory.
- `file` — identifies file types by examining their contents.
- `cat` — displays file contents.
- `*` — wildcard character that matches multiple filenames.
- `./` — specifies a relative path from the current directory.
- Files without familiar extensions can still contain readable text or binary data.

## Result

Successfully identified the only human-readable file and retrieved the password for Bandit Level 5.

