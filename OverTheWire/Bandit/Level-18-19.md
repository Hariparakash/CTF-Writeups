# Bandit Level 18 → Level 19

## Objective

Retrieve the password for `bandit19` stored inside the `readme` file in the home directory, even though `.bashrc` has been modified to immediately log out any interactive SSH session upon login.

## Commands Used

### 1. Execute `cat readme` directly over SSH

```bash
ssh -p 2220 bandit18@bandit.labs.overthewire.org cat readme
```

Passed `cat readme` as a remote command argument to `ssh`. Because specifying a command executes it directly without starting an interactive login shell, `.bashrc` was bypassed and the contents of `readme` were printed to the terminal (`<REDACTED_BANDIT19_PASSWORD>`).

### 2. Verify credentials by logging into `bandit19`

```bash
ssh -p 2220 bandit19@bandit.labs.overthewire.org
```

Authenticated successfully as `bandit19` using the retrieved password.

### 3. Exit the SSH session

```bash
exit
```

Closed the remote shell session.

## Concepts Learned

- **`.bashrc` Initialization:** A shell configuration script executed automatically whenever an interactive `bash` session starts.
- **Remote Command Execution via SSH (`ssh user@host <command>`):** Executes a single command on the remote server and returns its output without spawning an interactive shell, bypassing `.bashrc` auto-logout scripts.
- **Interactive vs. Non-Interactive Shells:** Understanding how login startup files behave differently when a command is passed directly to SSH versus opening a standard terminal prompt.

## Troubleshooting and Debugging

### Attempt 1: Standard interactive SSH login

```bash
ssh -p 2220 bandit18@bandit.labs.overthewire.org
```

**Observation:** The connection succeeded, printed `ByeBye !`, and immediately closed:

```text
ByeBye !
Connection to bandit.labs.overthewire.org closed.
```

**Cause:** The `bandit18` user's `.bashrc` file was configured to terminate the session as soon as an interactive shell initialized.

**Correction:** Appended `cat readme` directly to the `ssh` command to read the file non-interactively before `.bashrc` could execute.

### Final Solution

```bash
ssh -p 2220 bandit18@bandit.labs.overthewire.org cat readme
```

Successfully bypassed the `.bashrc` logout restriction and retrieved the password for `bandit19`.

## Alternative Simpler Method

If you want to drop into a full interactive shell instead of running a single command, you can force SSH to allocate a pseudo-terminal (`-t`) and spawn `/bin/sh` directly instead of `bash`, preventing `.bashrc` from loading:

```bash
ssh -p 2220 -t bandit18@bandit.labs.overthewire.org /bin/sh
```

## Result

Successfully bypassed the `.bashrc` auto-logout restriction, read `readme`, and authenticated into Bandit Level 19.
