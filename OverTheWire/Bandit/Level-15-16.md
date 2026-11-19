# Bandit Level 15 → Level 16

## Objective

Retrieve the next level's password by submitting the current level's password to port `30001` on `localhost` using SSL/TLS encryption.

## Commands Used

### 1. Connect to the SSL/TLS service

```bash
openssl s_client -connect localhost:30001
```

Initiated an SSL/TLS handshake with the local service on port `30001` and opened an encrypted session.

### 2. Submit the current password

```text
<REDACTED_BANDIT15_PASSWORD>
```

Entered the `bandit15` password directly into the active encrypted session, which returned `Correct!` along with the password for `bandit16`.

### 3. Exit the SSH session

```bash
exit
```

Closed the remote shell session.

## Concepts Learned

- `openssl s_client` — a command-line diagnostic utility for connecting to services over SSL/TLS.
- `-connect host:port` — specifies the target host and destination port for an OpenSSL connection.
- **SSL/TLS Encryption** — wraps network traffic in an encrypted tunnel, unlike plain-text TCP connections used by `nc`.
- **Self-Signed Certificates** — certificates not signed by a trusted Certificate Authority (returning `verify return code: 18`), which still allow encrypted communication in diagnostic tools.

## Troubleshooting and Debugging

### Attempt 1: Connecting with plain-text Netcat

```bash
nc localhost 30001
```

**Observation:** The connection did not accept plain-text password input properly because the service expected an SSL/TLS handshake first.

**Cause:** `nc` sends raw, unencrypted TCP traffic and cannot negotiate TLS encryption.

**Correction:** Switched to `openssl s_client -connect localhost:30001` to establish the encrypted handshake before sending the password.

### Final Solution

```bash
openssl s_client -connect localhost:30001
```

Successfully established an encrypted connection, submitted the current password, and retrieved the credentials for `bandit16`.

## Alternative Simpler Method

Instead of typing the password interactively after opening the connection, you can pipe the password file directly into OpenSSL using `-ign_eof` (so the connection stays open long enough to receive the server's reply) and `-quiet` (to hide certificate handshake output):

```bash
cat /etc/bandit_pass/bandit15 | openssl s_client -connect localhost:30001 -ign_eof -quiet
```

## Result

Successfully retrieved the password for Bandit Level 16.
