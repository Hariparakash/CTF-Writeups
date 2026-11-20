# Bandit Level 19 → Level 20

## Objective

Use the `setuid` binary (`bandit20-do`) in the home directory to execute commands with `bandit20`'s privileges and read the password for `bandit20` from `/etc/bandit_pass/bandit20`.

## Commands Used

### 1. Authenticate as `bandit19`

```bash
ssh -p 2220 bandit19@bandit.labs.overthewire.org
```

Logged into `bandit19` over SSH on port `2220`.

### 2. Inspect directory contents and permissions

```bash
ls -l
ls
ls -a
```

Listed the files in the home directory and identified an executable named `bandit20-do` owned by `bandit20` with the `setuid` bit set (`-rwsr-x---`).

### 3. Identify the file type

```bash
file bandit20-do
```

Confirmed that `bandit20-do` is a `setuid ELF 32-bit LSB executable`.

### 4. Check binary usage

```bash
./bandit20-do
```

Executed the binary without arguments, which displayed its usage instructions (`Run a command as another user. Example: ./bandit20-do whoami`).

### 5. Verify privilege escalation

```bash
./bandit20-do whoami
```

Ran `whoami` through `bandit20-do`, confirming that commands passed to it execute as `bandit20`.

### 6. Read the `bandit20` password file

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

Executed `cat` with `bandit20` privileges to read `/etc/bandit_pass/bandit20` and retrieved the password (`<REDACTED_BANDIT20_PASSWORD>`).

### 7. Exit the SSH session

```bash
exit
```

Closed the remote shell session.

## Concepts Learned

- **`setuid` (Set User ID) Bit (`-rwsr-x---`)** — a special Linux file permission (`s` in the owner's execute position) that allows any user with execute permission to run the binary with the effective privileges of the file's owner (`bandit20`) rather than the user running it (`bandit19`).
- `file` — inspects file headers (magic bytes) to determine the binary architecture and `setuid` status.
- **Privilege Escalation via SUID Binaries** — how misconfigured or helper `setuid` executables can be used to read restricted files owned by higher-privileged users.
- `/etc/bandit_pass/` — the directory where OverTheWire stores level passwords, readable only by each corresponding user.

## Troubleshooting and Debugging

### Attempt 1: Typo in filename with `file`

```bash
file bandit20-d
```

**Error:**

```text
bandit20-d: cannot open `bandit20-d' (No such file or directory)
```

**Cause:** Omitted the trailing `o` in `bandit20-do`.

**Correction:** Ran `file bandit20-do` with the complete filename.

### Attempt 2: Typo in relative path prefix

```bash
,/bandit20-do
```

**Error:**

```text
-bash: ,/bandit20-do: No such file or directory
```

**Cause:** Typed a comma `,` instead of a dot `.` for the current directory prefix (`./`).

**Correction:** Replaced `,/` with `./bandit20-do` to execute the binary in the current directory.

### Final Solution

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

Successfully leveraged the `setuid` binary to read `/etc/bandit_pass/bandit20` and retrieve the password.

## Alternative Simpler Method

Instead of running individual commands one by one through `./bandit20-do`, you can spawn an interactive shell (`sh -p`, where `-p` preserves the effective `setuid` privileges instead of dropping them) to operate directly as `bandit20`:

```bash
./bandit20-do /bin/sh -p
```

## Result

Successfully inspected the `setuid` permissions on `bandit20-do`, executed `cat` as `bandit20`, and retrieved the password for Bandit Level 20.
