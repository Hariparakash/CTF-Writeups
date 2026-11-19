# Bandit Level 17 → Level 18

## Objective

Find the password for `bandit18`, which is the only line that has been changed between `passwords.old` and `passwords.new` in the home directory.

## Commands Used

### 1. Authenticate as `bandit17` using the SSH private key

```bash
ssh -i bandit17.key -p 2220 bandit17@bandit.labs.overthewire.org
```

Logged into `bandit17` using the RSA private key retrieved from the previous level.

### 2. Review the `diff` manual

```bash
man diff
```

Checked the manual page for `diff` to understand how it compares two files line by line and what flags control its output format.

### 3. Compare `passwords.old` and `passwords.new`

```bash
cat | diff passwords.old passwords.new
```

Compared the old file against the new file in the proper chronological order (`FILE1` = `passwords.old`, `FILE2` = `passwords.new`). Line 42 showed the modified password:

```text
42c42
< <REDACTED_OLD_PASSWORD>
---
> <REDACTED_BANDIT18_PASSWORD>
```

### 4. Exit the SSH session

```bash
exit
```

Closed the remote shell session.

## Concepts Learned

- `diff FILE1 FILE2` — compares two files line by line and outputs the differences needed to change `FILE1` into `FILE2`.
- **Argument Order in `diff`** — placing the older file first (`passwords.old`) and the newer file second (`passwords.new`) ensures `<` marks the removed/old line and `>` marks the added/new line.
- `42c42` Notation — indicates that line `42` in the first file was **c**hanged (`c`) to line `42` in the second file.
- `man <command>` — opens the built-in manual page to inspect command syntax and flags.

## Troubleshooting and Debugging

### Attempt 1: Running `cat diff` instead of `diff`

```bash
cat diff -e passwords.new passwords.old
```

**Error:**

```text
cat: diff: No such file or directory
```

**Cause:** Passed `diff` as a filename argument to `cat`, causing `cat` to print the entire contents of `passwords.new` and `passwords.old` and complain that no file named `diff` existed.

**Correction:** Removed `cat` so `diff` runs directly as the command.

### Attempt 2: Reversing the file order with `-e`

```bash
cat | diff -e passwords.new passwords.old
```

**Observation:** Outputted the old line instead of the new password:

```text
42c
<REDACTED_OLD_PASSWORD>
.
```

**Cause:** Reversing the argument order (`passwords.new` first, `passwords.old` second) made `diff` show what was needed to revert the new file back to the old file, returning the outdated password instead of the `bandit18` password. Additionally, `cat |` was unnecessary since `diff` reads file arguments directly.

**Correction:** Swapped the arguments to `diff passwords.old passwords.new` so `>` clearly indicated the updated password for `bandit18`.

### Final Solution

```bash
diff passwords.old passwords.new
```

Successfully isolated line 42 and retrieved the updated password for `bandit18`.

## Alternative Simpler Method

Instead of running `cat | diff passwords.old passwords.new`, run `diff` directly without `cat`, or use unified format (`-u`) for an easier-to-read `+` / `-` view:

```bash
diff -u passwords.old passwords.new
```

## Result

Successfully compared `passwords.old` and `passwords.new` and retrieved the password for Bandit Level 18.
