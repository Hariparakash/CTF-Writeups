# Bandit Level 13 → Level 14

## Objective

The next level requires authentication using an SSH private key instead of a password. The key is available in the current level's home directory.

## 1. Inspect the home directory

```bash
ls
file HINT
file sshkey.private
cat HINT
```

The `file` command identified `sshkey.private` as an OpenSSH private key.

The hint explained that direct login from one Bandit account to another through localhost is blocked.

## 2. Understand SSH key authentication

```bash
man ssh
ssh -i sshkey.private
```

The second command was incomplete and displayed the usage information.

The `-i` option specifies the private key file, but SSH also requires a destination.

## 3. Attempt SSH from the remote server

```bash
ssh -p 2220 bandit14@bandit.labs.overthewire.org -i sshkey.private
```

The connection was rejected because the server blocks connections from localhost.

This confirmed the hint: the connection must be initiated from the local machine.

## 4. Transfer the private key to the local machine

After exiting the Bandit server, I used `scp`:

```bash
scp -P 2220 bandit13@bandit.labs.overthewire.org:~/sshkey.private ~/
```

This copied `sshkey.private` from the remote Bandit account into my local home directory.

## 5. Fix private key permissions

First, I attempted to use the key:

```bash
ssh -p 2220 bandit14@bandit.labs.overthewire.org -i sshkey.private
```

SSH rejected it because its permissions were too open (`0640`).

I checked the `chmod` manual and corrected the permissions:

```bash
chmod 600 sshkey.private
```

`600` means:
- Owner: read and write
- Group: no permissions
- Others: no permissions

SSH requires private keys to be protected from access by other users.

## 6. Connect using the private key

```bash
ssh -p 2220 bandit14@bandit.labs.overthewire.org -i sshkey.private
```

Authentication succeeded, and I logged in as `bandit14`.

## 7. Exit

```bash
exit
```

## Concepts Learned

- **SSH:** Secure remote login protocol.
- **Private key authentication:** Uses a private key to authenticate without entering the account password.
- **`ssh -i`:** Specifies the identity file.
- **`scp`:** Securely copies files between systems.
- **`chmod 600`:** Restricts file access to the owner.
- **SSH key security:** Private keys must not be accessible to other users.
- **Localhost restrictions:** A server can block connections originating from itself.

## Result

Successfully authenticated as `bandit14` using the provided private SSH key.
