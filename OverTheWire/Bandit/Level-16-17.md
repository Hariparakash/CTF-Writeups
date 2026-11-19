# Bandit Level 16 → Level 17

## Objective

Identify which port in the `31000` to `32000` range on `localhost` speaks SSL/TLS, submit the current `bandit16` password to retrieve an RSA private key, and use it to log into `bandit17`.

## Commands Used

### 1. Scan for open ports and service versions in the target range

```bash
nmap -sV -p 31000-32000 localhost
```

Scanned ports `31000` through `32000` on `localhost` with service detection (`-sV`) to identify which open ports were running SSL/TLS services (`ssl/unknown`) versus standard echo services.

### 2. Connect to the SSL/TLS service and submit the password

```bash
openssl s_client -connect localhost:31790 -ign_eof -quiet
```

Connected to the active SSL/TLS port (`31790`), entered the `bandit16` password, and received `Correct!` followed by an RSA private key (`-----BEGIN RSA PRIVATE KEY-----`).

### 3. Save the RSA private key to a temporary file

```bash
mktemp -d
nano /tmp/tmp.XXXXXX/bandit17.key
```

Created a secure temporary directory and saved the returned RSA private key into a file.

### 4. Restrict file permissions on the private key

```bash
chmod 600 /tmp/tmp.XXXXXX/bandit17.key
```

Set read and write permissions exclusively for the file owner so SSH would accept the private key.

### 5. Authenticate as `bandit17` using the private key

```bash
ssh -i /tmp/tmp.XXXXXX/bandit17.key -p 2220 bandit17@localhost
```

Logged directly into `bandit17` on `localhost` port `2220` using the retrieved SSH identity file.

### 6. Exit the SSH sessions

```bash
exit
exit
```

Closed both the `bandit17` and `bandit16` shell sessions.

## Concepts Learned

- `nmap -sV -p <range>` — scans a specific port range and probes open ports to determine service and SSL/TLS encryption status.
- `openssl s_client -ign_eof` — prevents OpenSSL from closing the connection immediately upon reaching End-Of-File so the server's response can be received.
- **SSH Private Key Authentication (`ssh -i`)** — authenticates to an SSH server using a cryptographic private key instead of a password.
- `chmod 600` — restricts file permissions to owner read/write only (`-rw-------`), which OpenSSH enforces for private keys to prevent unauthorized local users from reading them.
- `mktemp -d` — creates a uniquely named temporary directory inside `/tmp` to avoid filename collisions on shared systems.

## Troubleshooting and Debugging

### Attempt 1: Connecting to an open non-SSL port (`31046` / `31518`)

```bash
openssl s_client -connect localhost:31046
```

**Observation:** The TLS handshake failed with an SSL protocol error (`wrong version number` / `packet length too long`).

**Cause:** Those ports were running plain-text echo servers rather than SSL/TLS services.

**Correction:** Used `nmap -sV -p 31000-32000 localhost` to filter specifically for ports running `ssl/` services (`31790`).

### Attempt 2: Using the private key with open file permissions

```bash
ssh -i /tmp/tmp.XXXXXX/bandit17.key -p 2220 bandit17@localhost
```

**Error:**

```text
Permissions 0644 for '/tmp/tmp.XXXXXX/bandit17.key' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
```

**Cause:** Newly created files default to `0644` (`-rw-r--r--`), allowing other users on the system to read the private key.

**Correction:** Ran `chmod 600` on the key file before re-running the `ssh -i` command.

### Final Solution

```bash
chmod 600 /tmp/tmp.XXXXXX/bandit17.key && ssh -i /tmp/tmp.XXXXXX/bandit17.key -p 2220 bandit17@localhost
```

Successfully authenticated as `bandit17` using the retrieved private key.

## Alternative Simpler Method

You can automate finding the SSL port, retrieving the private key, saving it with strict permissions, and connecting in a single streamlined sequence using `ncat --ssl` (Nmap's Netcat replacement) and piping directly into a file:

```bash
KEY=\((mktemp) && chmod 600 "\)KEY"
cat /etc/bandit_pass/bandit16 | ncat --ssl localhost 31790 | sed -n '/-----BEGIN/,/-----END/p' > "\$KEY"
ssh -i "\$KEY" -p 2220 bandit17@localhost
```

## Result

Successfully identified the SSL/TLS port (`31790`), retrieved the RSA private key, restricted its file permissions, and logged into Bandit Level 17.
