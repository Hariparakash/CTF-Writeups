# Bandit Level 14 → Level 15

## Objective

The next level's password is obtained by submitting the current level's password to port `30000` on `localhost`.

## 1. Understand Netcat

```bash
nc
man nc
```

Running `nc` without arguments displayed its usage information. I checked the manual to understand its options and syntax.

Netcat (`nc`) is a networking utility that can establish connections to services using TCP or UDP.

## 2. Attempt to Connect to the Port

```bash
nc localhost -p 3000
```

This attempt displayed the usage information because I used `-p` incorrectly.

The `-p` option specifies the source port, not the destination port.

## 3. Connect to the Service

```bash
nc localhost 30000
```

The connection was established, but the service responded that the current password had not been supplied.

This helped me understand that I needed to send the password through the connection.

## 4. Submit the Current Password

```bash
cat /etc/bandit_pass/bandit14 | nc 30000
```

This attempt failed because I omitted `localhost` as the destination.

I corrected the command:

```bash
cat /etc/bandit_pass/bandit14 | nc localhost 30000
```

The service accepted the current password and returned the next level's password.

## 5. Exit

```bash
exit
```

## Concepts Learned

- **Netcat (`nc`):** A command-line networking utility used to establish connections to network services.
- **Localhost:** Refers to the local machine, usually through the loopback interface.
- **Ports:** Identify specific services or applications on a host.
- **Destination port:** The port number of the service you want to connect to.
- **`-p` option:** Specifies the source port in this Netcat implementation.
- **Pipes (`|`):** Pass the output of one command as input to another.
- **Client connection:** `nc localhost 30000` connects to a service listening on port `30000`.

## Result

Successfully submitted the current level's password to the service running on `localhost:30000` and retrieved the next level's password.
