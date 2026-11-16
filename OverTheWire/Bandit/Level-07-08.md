# Bandit Level 7 → Level 8

## Objective

Find the password stored in `data.txt` on the same line as the word `millionth`.

## Commands Used

### 1. List the current directory

```bash
ls
```

Confirmed that `data.txt` was present.

### 2. Inspect the file

```bash
cat data.txt
```

Displayed the file contents, but the output was too large to search manually.

### 3. Search for the target word

```bash
grep "millionth" data.txt
```

This returned the line containing `millionth`, along with the corresponding value.

### 4. Exit

```bash
exit
```

## Concepts Learned

- **`grep`** searches text for lines matching a specified pattern.
- **`grep "pattern" filename`** searches for a pattern inside a file.
- **`cat`** displays file contents, but is not practical for manually searching large files.

## Result

Successfully located the required line using `grep` and retrieved the next level's password. The password itself is intentionally excluded from this write-up.
